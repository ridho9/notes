# Pipa Lang Notes

## Prelude

To create a competitive-programming friendly language, somewhat for code golfing (while not being really short).
Take inspiration from Elixir, APL.

I like the pipe (|>) operator in Elixir, it makes chaining up computation process easy to understand.
But since Elixir is a immutable language, everything have to be copied, which affects the performance.
I like APL since it has many default algorithms that useful for code golfing.
But APL is, well, APL. While it could make your code looks really short and cool, I could hardly understand what I wrote the day before.
Also array programming is surprisingly fun. Trains are fun when you could understand it (which I don't).

The idea is to design a language where you could use piping to pipe data between functions,
but where it doesn't have to copy data everytime.
And also include a lot of useful builtins.

## Basics

The basic syntax looks like Elixir:

```
> input <- enum 1 5
[1, 2, 3, 4, 5]

> reverse input
[5, 4, 3, 2, 1]

> input |> reverse
[5, 4, 3, 2, 1]

> input # not changed
[1, 2, 3, 4, 5]
```

This language is expression based

## Types

- Integer: 1, 5, -20
- Float: 20.5
- Boolean: true, false

## Data Structures

- Array: [1, 2, 3, 4, 5]
- Tuple: ??
- Dictionary: ??

## Operator

Basic arithmetics operator are like usual:

```
+ - * /
< <= > >= == !=
| & (binary)
|| && (boolean)
! (negation)
```

## Function declaration

```
# normal
square <- :{x ->
    x * x
}

# oneliner
square <- :{x -> x * x}
> square 5
25
> :{x -> x * x} 5
25

# anonymous parameter
square <- :{&0 * &0}

# use in pipe
> map input :{&0*&0}
[1, 4, 9, 16, 25]
```

No param function
```
:{->}
```

## Mutability

### 1. Mutability on function call syntax/name

Declared variables could have their content be mutated.
The prelude includes mutating primitives.

```
> input <- enum 1 5
[1, 2, 3, 4, 5]
> set input 0 6
[6, 2, 3, 4, 5]
> input # not changed
[1, 2, 3, 4, 5]

> set! input 0 6
[6, 2, 3, 4, 5]
> input # changed
[6, 2, 3, 4, 5]
```

Function calls arguments are always passed as values.
Function with `<name>!` passes the first argument as reference which could be mutated.
(Will this looks like Rust's `mut` tracking)

```
> input <- enum 1 5
> map input :{&0*2}
[2, 4, 6, 8, 10]
> input == enum 1 5
true
> map! input :{&0*2}
[2, 4, 6, 8, 10]
> input
[2, 4, 6, 8, 10]
```

### 2. Mutability syntax on passed parameter.

In this proposal, instead of having a mutating function.
We explicitly pass the value as ref/pointer (like Go's &var)

```
> input <- enum 1 5
[1, 2, 3, 4, 5]
> set @input 0 6
[6, 2, 3, 4, 5]
> input # changed
[6, 2, 3, 4, 5]
```

??: Should the function be able to specify whether it expects references or not. And whether to warn/error/allow?

```
> set <- {@arr idx val ->
    # somehow modify @arr, like arr[idx] <- val
    # ??: only allow ref declaration on explicit function param
    # or allow on numbered param, but how
}
> set @input 0 6
# success, input is mutated
> set input 0 6
# ??: warning? error? or return the normal value but input not mutated?
```

## Conditionals and Loop

### If

```
>if cond value_true value_false

>if cond {
    # code block
} {
    # code block
}
```

```
```
