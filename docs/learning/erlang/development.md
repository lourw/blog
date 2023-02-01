---
title: Erlang
description: Overview of common themes when developing in Erlang
---

This section is a little reminder to me about the conventions and best practices to use when coding in Erlang. I'm currently taking senior level university course on parallel programming and have a love-hate relationship with this functional language. 

## Conventions
### Variables
* Function names: 
    * `lower_case()`
* Variables:  
    * `Capitalized` for parameters
    * `_Underscored` for unused
    * `[Hd | Tl]` for arrays

### Pattern Matching
* `_` matches all

## Conditionals
When controlling code flow, most languages use `if` and `switch` statements

### Guard Statements
The most basic form of boolean statements are known as guard expressions

```erlang
% Guard examples
Eshell
> is_number(24). % true
> true
> is_list(1). % false
> false
> X = 10.
> 10
> X =:= 13.
> false
```

## If Statements
If statements can control code flow based on the output of guard expressions

```erlang
func(X) ->
    if 
        is_number(X) ->
            number;
        X > 120 ->
            greater
        true ->
            finish
    end.
```

If there are no valid expressions, `if` will throw an error. You can prevent this by adding `true` to act like an `else` block.

## Case Of Statements
If you would rather control code flow by pattern matching, use `case of` statements intead

```erlang
func(X) ->
    case X of
        12 -> number
        _ when is_list(X) -> list
    end.
```

## Functions
You can define functions in erlang using the following syntax:

```erlang
sum(X, Y) -> X + Y
```

Function names can be overloaded, but not function signatures. 
```erlang
sum(X) -> X + 1.
sum(X, Y) -> X + Y.
```

### Higher Order Functions
#### Fold-Left
```erlang
% foldl takes three parameters: function, initial accumulator value, list to fold over
sum_array(List) -> lists:foldl(
    fun (X, Acc) ->
        X + Acc
    end, 0, List).
```
