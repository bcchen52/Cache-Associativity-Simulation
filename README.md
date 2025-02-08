# Cache-Associativity-Simulation

This project uses C++ to model the hit rate and accuracy of different types of cache systems, including direct-mapped, set-associative, fully-associative, and two-level caches, and examines the accuracy of different write-policies for stores instructions. 

Based on the simulated results, higher associativity tends to lead to a higher hit rate. 

In this directory are three .txt files containing a few million rows of instruction addresses and type, expected results (given by course instructor), and results files that are populated by running this program. 

Direct-mapped caches map groups of addresses to a block using index bits. When an address in memory needs to be accessed, it checks that corresponding cache block to see if that address is in that block. If not, the most recently used block will replace the current cache block. Caches take advantage of space locality, as data that is close in memory to previously accessed data will be readily available in the cache. 

Set-associative caches use a set of cache blocks, where the range of addresses in a typical block would be shared by a set. This allows for blocks/addresses that are used more often to be saved in the cache, potentially increasing hit rate depending on how the data is structured. If many addresses are in the same range as one block, but not in the other blocks in the set, then a set-associative cache will offer better performance. In order to choose which block in a set to replace, a least recently used mechanism needs to be employed. A genuine least recently used mechanism that always replaces the least recently used block may not be efficient. In this project, we examine the use of a hot and cold bit least recently used approximation mechanism compared to an actual least recently used mechanis. 

A fully-associative cache can be through of as a set-associative cache where there is 1 set, and all addresses map to the same set. 

In this project, the compare the accuracy and hit rate of these types of caches. 

Caches are smaller and faster than their underlying memory, which is why they are utilized to save time by reducing the number of memory accesses. In practice, there are multiple levels of caches that get larger and slower as they get close to memory. In this project, we simulate two level caches, where the L2 cache is a 4-way set associative cache that implements different L1 cache structures. 

Write-policies refer to how store instructions are tracked with caches. When a load instruction is called, we are only accessing the data. However, when we change data, it is only changed locally in the cache, and we need to figure out how to get it into main memory, as that needs to be consistent. We examine write-back and write-through policies, where write-back changes the data in L1 and L2 when the instruction is executed, while write-through policies mark changed data as "dirty", which will change the data when the block is swapped back memory. 

More specifics below.

This project is from Binghamton University’s CS 320, Advanced Computer Architecture. 

This code is 100% original, and the test files and expected outputs were given by the course instructor. 

# Table of contents
- [Files](#files)
- [How To Use](#how-to-use)
- [Specifics](#specifics)

# Files
1. `caches.cpp` is the predictor program.

2. `makefile` holds instructions to compile and clean.

3. `traces/` contains .txt files of instructions and their type. 

4. `correct_outputs/` contains expected branch prediction results.

5. `output.txt` is the .txt files that our program writes to.

# How To Use

1. In this directory, run...
    ```
    $ make
    ```

2. To test this on any trace, run...
    ```
    $ ./caches traces/filename output.txt
    ```
    For example...
    ```
    $ ./caches traces/trace1.txt output.txt
    ```
    ```
    $ ./caches traces/trace2.txt output.txt
    ```
    ``` 
    $ ./caches traces/trace3.txt output.txt
    ```

3. Examine the results file with the corresponding expected results file.

4. When you’re finished…
    ```
    $ make clean
    ```

# Specifics

The outputs are either in the form of `X; L1 accesses` or `X; L2 accesses`. The latter is only for the second half of outputs. 

1. Line 1 shows the hit rate and accesses of a basic direct-mapped cache with 5, 7, 9, and 10 target bits. More target bits tend to increase specificity, resulting in an increased hit rate.

2. Line 2 shows the hit rate of a 2, 4, 8, and 16-way set-associative cache, all with 5 target bits. Based on our instruction sets, these caches are more accurate than direct-mapped caches, and are better with more associativity. 

3. Line 3 shows the hit rate of two fully-associative caches with a genuine LRU and a hot and cold bit psuedo-LRU, respectively. Though an actual LRU mechanism tends to be more accurate, hot and cold bits are close. 

4. Line 4 shows the L1 and L2 hit rate and accesses of a two level cache with an 8-way set associative cache for L1 and a 4-way set associative cache for L2. This cache implements a write-through policy that updates written to blocks in L1 to L2 once they are written. 

5. Line 5 uses the same caches as line 4, but instead implements a write-back policy that marks written to blocks as dirty, only applying them to L2 once those blocks are changed out an access needs to happen anyway. There are much less L2 accesses in this than the previous. The number to the right is L2 cache utilization. 

6. Line 6 implements the same caches as 5 but instead uses a fully associative L1 cache, offering very similar L2 hit rate and utilization as 6.

7. Line 7 implements the same caches as 5 and 6 but with a direct-mapped cache, with a significant lower L2 hitrate and utilization than the previous 2.