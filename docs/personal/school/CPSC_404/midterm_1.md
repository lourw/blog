---
title: Midterm 1
---

## Disk & Storage

### Disk Geometry

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
You incur the penalty only when you first search for a set of data, then all following pages can just be found with T_transfer.

$$
    T_{seqAccess} = (T_{seek} + T_{rotate}) + (T_{transfer} \cdot n_{pages})
$$

## External Sorting

### Sort Phase

### Merge Phase

## B+Trees

## Hashing

### Dynamic Hashing

### Extensible Hashing

### Linear Hashing

## Query Optimization
