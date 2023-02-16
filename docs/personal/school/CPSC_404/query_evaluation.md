---
title: Query Evaluation
---

## Density of Indexes
- indexing process
    - choose a key to build an index on (department is not primary key -> sparse index)
    - write the index entries as you iterate through all the data
    - because of the way data is arranged, you will get your index entries in non sorted order

- sparse index gets created when you index on a secondary key
- dense index gets created when you index on a primary key

- we can't sort the underlying data, but we can sort the data

- sparse: index entry per page of records
- dense index: index entry for every distinct occurrance of a value of indexed attribute in a record

## Query Evaluation
- a query can be evaluated by a number of different strategies called plans, all creating the same result
- when we assess the IO of a plan, we look at reading and writing IO for intermediate results, but never the final result

- three operators for evaluation
  - index probing:
    - use an index to find tuples that match a selection condition, you chase the pointers until you get your result
  - iteration:
      1. table scan: exhaustively read everything in a file
      2. index scan: using the index file, it does not follow the pointers at all. You do an operation on the key of a data entry, not on the underlying data. 
  - partitioning:

Example

Given Table `songs(sid, sname, genre, year)` with 500 pages and 

``` sql
SELECT * 
FROM songs
WHERE genre = 'hiphop' AND year > 2010
```

$$
    \sigma_{genre}(songs)
$$

## Analysis of Evaluation

#### Table Scan
Given a basic table scan, you will have 500 pages and therefore 500 IOs

#### Index Scan
- only makes sense for $alt2$ and $alt3$, if $alt1$, you are basically running a table scan
- a sparse $alt2$ index is not useful because not all key values are reflected in the index, you need a dense index

Example

Given table `ratings(uid, sid, time, rating)` and query
``` sql
SELECT sid 
FROM ratings 
WHERE uid = 123 AND time > 2019
```

#### Index Probe
- cost is ~1.5 I/Os for getting to the DE, +1 I/O if the DE is $alt2$

What would be the cost if the index was a B+tree?

## Terminology
- conjunctive (AND) conditions are vastly more ocommon 
- when you use disjunctive conditions, table scans be the most effective
- when you have a long conjunctive condition, each indvidual expression is called an atomic conjuctive condition
- when does an index match a selection condition $C$? (Hash or B+Tree)
  - hash index: for each search key attribute $A$, $C$ has a conjunct of the form $A = v$
    - #### Examples
        ``` sql title="invalid"
        SELECT * 
        FROM ratings
        WHERE year = 2020
        ```

        ``` sql title="invalid"
        SELECT * 
        FROM ratings
        WHERE uid >= 2010 AND year = 2020
        ```
        In this example, a table read could be better. You only need to random I/O once and then read the rest of the data sequentially. Following pointers would incurr random accesses everytime you go to another pointer.

        ``` sql title="valid"
        SELECT * 
        FROM ratings
        WHERE uid = 123 AND sid = 455
        ```

        ``` sql title="valid (partial)"
        SELECT * 
        FROM ratings
        WHERE uid = 123 AND sid = 455 AND time >= 2020
        ```
  
  - B+tree: for some prefix of search key, for every attribute $A$ in the prefix, $C$ has a conjunct of the form $A$ op $v$ where op is `=, >, <, <=, >=`
    - #### Examples
        Assume the B+tree is sorted on (uid, sid, time)
        ``` sql title="valid"
        SELECT * 
        FROM ratings
        WHERE uid >= 123 AND sid = 455
        ```

        ``` sql title="valid (partial)"
        SELECT * 
        FROM ratings
        WHERE uid >= 123 AND time <= 2022
        ```
        In this case, time doesn't match, but we can load the records into RAM and manage the rest of the query there. 

## Questions
Should you build indexes (given you know your workload) such that you never need to touch the data table?

- depends on your storage and use case requirements (you may need to involve multiple tables)
- when we do updates on the underlying data, you will need to factor in overhead to update the indexes

## Need to Review
- what is an index scan
