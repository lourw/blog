---
title: Midterm 1
---

## Disk & Storage

### Disk Geometry

??? example "Examples"

    Question: Here

![Disk Geometry Diagram](https://media.licdn.com/dms/image/C4E12AQEuLcYzzAW3xQ/article-cover_image-shrink_600_2000/0/1520227022449?e=2147483647&v=beta&t=ZlkUHLnTSqHZuCX49hQcjyK9UutS6jQSQIH8KXWmbkQ)

#### Access Time
The time needed to read or write a page is made up of seeking, rotating, and transferring

##### Seek Time
$$
\begin{align}
    T_{seekMin} & = T_{setup} \\
    T_{seekMax} & = T_{setup} + (T_{trackSweep} \cdot  n_{totalTracks}) \\
    T_{seekAvg} & = T_{setup} + (T_{trackSweep} \cdot n_{avgTracks})
\end{align}
$$

##### Rotation Time
$$
\begin{align}
    T_{rotateMin} & = 0 \\
    T_{rotateMax} & = T_{rotateOneTrack} \\
    T_{rotateAvg} & = \dfrac{T_{rotateMax}}{2}
\end{align}
$$

##### Transfer Time
$$
\begin{align}
    T_{transfer} & = T_{rotateOneTrack} \cdot \dfrac{n_{sectorsInPage}}{n_{sectorsInTrack}}
\end{align}
$$

#### Prefetching
You can reduce the number of random reads and writes by doing that action on a set of sequential data. In this case, you only incur the random access penalty once. 

##### Random Access
You need to incur the random access penalty for each of the pages.

$$
    T_{randomAccess} = (T_{seek} + T_{rotate} + T_{transfer}) \cdot n_{pages}
$$

##### Sequential Access
You incur the penalty only when you first search for a set of data, then all following pages can just be found with $T_{transfer}$.

$$
    T_{seqAccess} = (T_{seek} + T_{rotate}) + (T_{transfer} \cdot n_{pages})
$$

## External Sorting

??? example "Examples"

    1. You have 500 pages of RAM used to sort a file with 25 000 pages. 
          1. How many SSLs would be produced from the sort phase if you use cylindrification?  
             * let $m$ be the number of times that the data file can fit in RAM
             * $m = \dfrac{25000}{500} = 50$
             * $\therefore$ You would have 50 SSLs 
          2. Assume we do a 10-way merge, how would you allocate space for input and output buffers to minimize I/Os?
             * You have space (500 pages) for 10 sets of input buffers and 1 output buffer
             * In total you have access to 11 buffers. We want to distribute space evenly to input and output.
             * let $n_i$ be the number of buffers in a single set of input buffers
             * $n_i = \lfloor500 buffers / 11 sets\rfloor$ = 45
             * $\therefore$ You have 10 input with 45 each and 1 output with 45 + leftover (50) buffers. 


Why do we need sorting?

* needed for bulk loading of B+Trees
* eliminating duplicates
* useful for aggregate operations like `GROUP BY`
* useful for efficient `JOINS` of relations

Guiding principle
> Random page accesses are very expensive and should be minimized in favour of sequential accesses

How do we put that principle into practice?

* we operate on data in clusters so that we create more opportunities for sequential access
* store those sequential data blocks in memory 

### Sort Phase

The first step of external sorting is sorting sections of data into `sorted sublists (SSLs)`. 

> Let $B$ be the size of RAM  
> Let $m$ be the number of times a data file can be divided into $B$ sections  
> $\therefore S_{data} = m \cdot B$

1. read $B$ pages of data into memory. Maximize the amount of data written into memory by using the entirety of RAM
2. the result is $m$ SSLs that are each *locally sorted*

### Merge Phase

Once you have your locally sorted SSLs, you now have to merge them back onto disk in sorted order. 

The most efficient way to merge large datasets is with a $k$-way merge. Where $k$ is $B - 1$. 
> You allocate $B - 1$ buffers in RAM for each sorted sublist and 1 buffer for the output. 

### External Sorting Optimizations
* `cylindrification`: placing blocks on the same cylinder to reduce seek and rotational latency
* `prefetching`: reading more data in than necessary in order to cache it for future use
* `double buffering`: allocating another set of input/output buffers so that the CPU isn't idle when reading or writing data into RAM

## B+Trees

A B+Tree is an indexing method, a way to quickly retrieve data records from a data file. 

The idea:

* store an index file that will reference data from the actual data file
* traverse the index file instead of iterating through all the data

### Data Entries
The individual records of a database can be stored in three ways:

1. $alt1$: direct access to the data record with the key value $k$
2. $alt2$: a key value $k$ and a single pointer that points to the corresponding data record $k^*$
3. $alt3$: a key value $k$ and a list of pointers that point to the corresponding data records

* what happens if you use $alt2$ or $alt3$ for a primary key?
  * $alt2$ and $alt3$ would essentially return the same result since there is only one value associated with $k$
* what happens if you use $alt2$ with a secondary key?
  * you would get multiple data entries that all have the same key

## Hashing

Hashing is the second indexing method

### Dynamic Hashing

### Extensible Hashing

### Linear Hashing

## Query Optimization
