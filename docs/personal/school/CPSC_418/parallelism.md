---
title: Parallelism
description: Overview of parallel principles on Erlang
---

This page is more for my own reference when taking CPSC 418: Parallel Computing. The library used is based off of ones already implemented by my school. I'm just writing down some of the conventions and templates needed for common parallel algorithms. 

## Reduce

Relevant functions

``` erlang
wtree:reduce(Workertree, LeafFunction, CombineFunction)

% Setting up for parallel operations
wtree:create(N_Workers)
wtree:get(ProcessState, Key)
workers:update(Workertree, atomKey, SplitData)
misc:cut(Data_List, Workertree) % SplitData
```

### Examples

``` erlang title="Parallel Add"
add(Workertree, Key) where is_atom(Key) -> 
    wtree:reduce(Workertree, 
        fun (ProcState) -> 
            add_leaf_function(ProcState, Key),
        fun (Left, Right) -> 
            add_combine_function(Left, Right).
    ).

add_leaf_function(ProcState, Key) ->
    lists:sum(wtree:get(ProcState, Key)).

add_combine_function(Left, Right) ->
    Left + Right.

test_add(N_Workers, List) -> 
    Workertree = wtree:create(N_Workers), 
    workers:update(Workertree, data, misc:cut(List, Workertree)), 
    add(Workertree, data).
```
