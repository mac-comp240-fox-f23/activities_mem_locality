# Activity 18: Spatial and Temporal Locality
## Put your name(s) here

In this activity, you will observe good and bad locality using example code and timing functions. You will be asked questions as you complete this activity, which you should answer in this README file.
You will:
- Experiment with different access patterns for one-dimensional arrays
- Explore 2d and 3d arrays and how different access patterns affect locality and running time


## Provided Code

You start with:
- `Makefile`
    - a makefile to automate the compilation process
- `sumvec.c`
    - 1-d array manipulation with varying degrees of good and poor locality
- `sumarrayrows.c`
    -  2-d matrix manipulation with good locality
- `sumarraycols.c`
    - 2-d matrix manipulation with poor locality
-  `sumarray3d.c`
    - 3-d matrix manipulation with varying degrees of good and poor locality
- `timing.h`
    - A header file containing a library of utility functions for timing code


### Timing Code

You can observe the impact of poor localty by timing how long your code takes to execute. This is a skill you should use as you work on larger projects. You will use a helper file called `timing.h` for this purpose.

Files ending in `.h` are designed to be included in `.c` code files. We have used header
files to include standard C library files, as well as headers that declare things defined
in a separate `.c` file.On many C software development teams, there are a certain core set of these `.h` files that are shared within the team for different reasons. One reason is that the file contains utility functions that are used routinely in a project. This `timing.h` file falls into this category.

In `timing.h`, you have two functions and a typedef that allow you to time code more simply. Each of the four code files will include this file

The basic concept behind using these functions is the following:

1. Mark a start time just before a section of code that you want to time.
2. Execute code to be timed.
3. Retrieve the amount of time that has passed since the mark at step 1.

## Your Tasks

### Task 1: One-Dimensional Arrays: `sumvec.c`

The `sumvec.c` program creates an array of a given size, initializes it to have
1 or -1 in each cell, and then times how long it takes to access the values in the 
array, sequentially, and then using a particular "stride." Two separate functions
are timed: 
1. `sumvec`, which performs a simple stride-1 sequential loop over the array values
2. `sumvec_stride`, which accesses the array in a special pattern when the value of the argument called **stride** is greater than 1. The default value of 1 establishes a baseline for how much time this function takes.
`

**Command-Line Arguments:** The `main` function here is set up to take in and process
command-line arguments. This version of `main` has two inputs: `argc`, the number of 
items in the call to the executable, and `argv`, an array of strings containing the
items in the call. Here we allow the user to change the size of the array and the stride
by passing one or two optional values to the program at run-time.

You can call `sumvec` in three ways:

With the default configuration:

    ./sumvec

With a new array size:

    ./sumvec 100000000

Or with both a new array size and a new stride length:

    ./sumvec 10000 5


- Read through the `sumvec.c` code file, and discuss it with your teammates until
you feel confident you understand what it is doing
- Look carefully at the code for the function `sumvec_stride`. Work with your neighbors 
to describe and visualize how the elements in the array are being accessed when the stride is greater than 1.
- What is the pattern of accesses for the `sumvec_stride` function? What does "stride" 
mean in this context? 

(Put your answers here in the README)

- Compile the program, and run it with the default configuration. Look closely at the
output of the function. Record the timing values it prints.
- Discuss with your teammates: 
    - Why does it take longer to run `sumvec_stride` than `sumvec` even when the stride length is 1?

(Record your observations here)

- Change the stride by running the code as shown below, and record the timings reported:

    ./sumvec 1048576 4

- Experiment with other stride lengths, such as 8, 16, 32, and 64 (recording the timings)
- Experiment with other shorter or longer array lengths
- Record your observations: what changes affect the running time for the `sumvec_stride` function? How do the changes vary over different stride lengths? How does array length affect timing?
What is happening as you run this code? What explains the running time of `sumvec_stride` as compared to the original `sumvec`?

- (Optional challenge) Add code to `main` check how long it takes to initialize the 1-d array (in the middle of `main` where the loop is). This is also a stride-1 reference pattern. What
are the results?


### Task 2: Two-Dimensional Arrays

Now you will observe access patterns of 2D arrays. You have 2 programs, each of which statically creates a 2D array, or matrix, of M rows and N columns. You start with M and N being equal, so that the matrix is square. The code files are called:

    sumarrayrows.c
    sumarraycols.c
    
Look at the `.c` files for each to determine the difference in their access patterns as they sum values in the array. 

Before running the code, discuss and record your predictions: 
- Which of these do you expect to have better locality of reference when it comes to the array `a`, which is considered data?
- Will either of them have better locality of reference when considering the instructions being executed in main and the function called from main? Explain whether the locality will be good or bad for the instruction references.

- Execute `sumarrayrows` several times to determine an approximate average time to execute the `sumarrayrows` function. Record your observations.

- Execute `sumarraycols` several times to determine an approximate average time to execute the `sumarraycols` function.  Record your observations.

- Which one runs faster and why?

- (Optional Challenge) Add code to check how long it takes to initialize the 2-D array (at the beginning of `main` where the nested loop is). This is row-major order reference pattern in each case.  What do you notice about the timings here?

### Task 3: Three-Dimensional Arrays

- Open the file called `sumarray3d.c`. Read and discuss it with your teammates
until you understand it.

The function `sumarray3d` in `summarray3d.c` should run about as fast as possible when accessing all the elements in the 3D array. 

- Run it a few times to see how much time it takes to access the 3D array using the access pattern in the `sumarray3d` function. Record your obsservations.

- Complete the function `sumarray3d_slower` by changing the access pattern to one that demonstrates poor locality. There are a few ways to do this--experiment with different possibilities. Time your slower function in `main`. Then make and run the code a few times to observe your slower access pattern. Record your observations.

- How much of a difference does good locality versus poor locality make?

### Optional Task 4: Costs of Recursion

- Make a copy of `sumvec.c` and replace the `sumvec_stride` function with 
a recursive function, `sumvec_recur` that takes in three inputs:

    int sumvec_recur(int *v, int pos, int length);

This function should implement the following pseudocode:
1. Check if pos >= length; if so, return 0
2. Otherwise, add v[pos] to the result of calling sumvec_recur recursively, adding
one to pos

- Modify the `main` function so that it doesn't refer to stride, and so it
calls `sumvec_recur` instead of `sumvec_stride`.

- Examine the results. Why might recursion take longer than plain iteration? Are
there reasons based in the principles of locality? Are there other reasons?

## References

- Command-line arguments in C
    - [*Dive into Systems*, Section 2.9.2](https://diveintosystems.org/book/C2-C_depth/advanced_cmd_line_args.html#_c_cmd_line_args_)
    - [Command-Line Arguments in C/C++, by Geeks for Geeks](https://www.geeksforgeeks.org/command-line-arguments-in-c-cpp/)
- Time library in C
    - [CPlusPlus time.h](https://cplusplus.com/reference/ctime/)
    - [time.h header file in C with Examples (Geeks for Geeks)](https://www.geeksforgeeks.org/time-h-header-file-in-c-with-examples/)
