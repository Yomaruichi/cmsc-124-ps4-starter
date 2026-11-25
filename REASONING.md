<!--no-pdf-->
# Why Each Prediction Reads the Way It Does

Write one entry per case, before you run anything. Name the rule behind each
predicted field. One to three sentences is enough. The grader checks the
structure of this file only. It does not judge how good the reasoning is, and
it sets no word minimum.

For every case you predicted wrongly, add a short correction after you run
it: what you predicted, what the language printed, and which rule you
misread. Corrections belong in the correction commits, not the prediction
commit.

Commit this file together with `predictions.tsv`, before any case runs:

```bash
git add predictions.tsv REASONING.md
```

The example below uses a query that is not one of the 16 cases, so it gives
no answer away. `parent(pat, X)` reads `parent(pat, jim)` because the `parent`
facts are tried top to bottom and only that one has `pat` as its first
argument:

```text
## parent(pat, X)

The goal scans the parent facts in source order. Only parent(pat, jim) has
pat first, so X binds to jim. No second fact matches, so the first ; finds
nothing and the search ends after one answer.
```

## P01

Prolog first scans the parent facts and takes the ones whose first argument is `tom`. Since `parent(tom, liz)` and `parent(tom, bob)`
matches, `X` binds to `liz` and `bob`. No fact is left, so the count is
`2`.

## P02

Prolog first scans the parent facts in source order and finds a match
of `parent(tom, bob)`. Since there's only one that matches, it succeeds with `true` for first and none on second, it also only counts just `1` valid solution.

## P03

Prolog first scans the parent facts and takes the ones whose second
argument is `tom`. No fact has `tom` as its second argument so the search
returns `false`. Since there are no successful searches, it has `0`
count.

## P04

Prolog first binds `X` to `f(Y,a)`, with `Y` still unbound. Then it binds
`Y` to `b`, which makes `X` represent `f(b,a)`. Both bindings belong to
only `1` solution which makes the second part none.

## P05

Unification matches the arguments of both f terms, binding X to a and Y to b. This gives one solution and no second answer.

## P06

The distinct atoms a and b cannot unify, so the query fails with zero solutions and no second answer.

## P07

The path through liz fails because she has no children. The path through bob gives ann, then pat, for two solutions.

## P08

The base clause gives liz and bob first. Recursion then finds ann, pat, sue, and jim, for six solutions in total.

## P09

The recursive clause searches deeper before trying direct children, so sue comes first and jim second. The remaining answers are ann, pat, liz, and bob, for six solutions.

## P10

The base clause finds pat first. Recursion then finds tom through bob and pat, followed by bob through pat, for three solutions.

## P11

The first clause calls reach again before reading an edge, so it recurses forever without producing an answer. All three fields are unbounded.

## P12

Reading edges first reaches d through a, b, and c, giving true. Backtracking repeatedly follows the cycle and reaches d again, so the second answer is true and the count is unbounded.

## P13

The len function removes the head of the element with `[ _ | T ]` and
recursively traverses the tail. `N is M + 1` calculates the current
length of the list as it traverses, this produces `one` binding for `N`
since theres no alternative clauses to produce another solution.

## P14

The join function's first clause `join([], L, L)`, gives the first result
since `X` is the first argument, it combines with an `empty list`, while
`Y` and `[1,2]` combines as it binds to L which creates `X=[],Y=[1,2]`.
Then the second clause recursively joins the 3 arguments again, creating
`X=[1],Y[2]` and `X=[1,2],Y=[]`. which creates a total of `3` successes.

## P15

Prolog evaluates goals left to right, so it attemps `X > 2` first which
creates an error since there's no binding for X, this throws an error and
ends the execution before reaching elem.

## P16

The elem function creates the binding `X=1`, `X=2`, and `X=3`.  The `,`
signifies `and` so it compares each binding to `X > 2`, `X=1` and `X=2`
fails this so it produces no answer, while `X=3` succeeds which creates
`1` success. No other bindings left so the check ends.

