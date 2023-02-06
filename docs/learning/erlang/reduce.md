---
title: Breakdown of Reduce
---

Breakdown of my class's library to implement reduce in Erland

# Create

``` erlang
% Call create/3 to spawn N_Leaves child processes
create(N_Leaves) when is_integer(N_Leaves), N_Leaves > 0 ->
    create(N_Leaves, 0, none).

% Create N_Leaves child processes
%   Each worker gets a dictionary with already defined keys
create(N_leaves, BaseIndex, ParentPid) ->
    spawn(fun() ->
        Children = create_kids(N_leaves, BaseIndex, ParentPid),
        worker([
            {parent, ParentPid},
            {index, BaseIndex},
            {n_leaves, N_leaves},
            {children_td, Children},
            {children_bu, lists:reverse(Children)}
        ])
    end).

% If there is only one child, don't create any new children
create_kids(1, _, _) ->
    [];

% If there are multiple children, return a list of "child" processes
create_kids(N_leaves, Index, ParentPid) ->
    {N_left, N_right} = split(N_leaves),
    [
        create(N_right, Index + N_left, self())
        | create_kids(N_left, Index, ParentPid)
    ].
```
