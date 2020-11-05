---
title: Dependent Type Theory and Agda
date: 2020-10-28
id: agda
status: published
readtime: 5m
---

```agda
module _ where
```

*Read more on literate Agda [here](https://agda.readthedocs.io/en/latest/tools/literate-programming.html#literate-markdown)*

# Dependent Type Theory and Agda

Agda is dependently-typed (DT) programming language. Dependent types allow the programmer to encode logical information into the type system. For example, "the type of sorted lists" and "the type of trees with maximum depth 5" are expressible. This form of type checking makes DT languages a powerful tool for writing programs with strong theoretical guarantees.

DT languages are often called proof assistants because the type-checker can be used to verify a logical proof mechanically. In this post, I construct some basic data types in Agda and demonstrate a few simple proofs about these data types.

## Primer on Type Theory

Type Theory was formalized by Per Martin-Löf in his 1972 paper [An Intuitionistic Theory of Types](https://github.com/michaelt/martin-lof/blob/master/pdfs/An-Intuitionistic-Theory-of-Types-1972.pdf). Martin-Löf wanted a way to describe *constructive* mathematics, a form of logic that requires the prover to build something in order to prove that it exists. In other words, we do not use [proofs by contradiction](https://en.wikipedia.org/wiki/Law_of_excluded_middle) or other non-constructive axioms.

The form of type theory defined in Martin-Löf's original paper is called *extensional* type theory as the fundamental objects under study are classical sets―that is to say, the theory *extends* an existing logical framework. The *intensional* formulation of the theory defines types axiomatically and this is the form that Agda uses.

## Basic Agda Syntax

Agda supports Unicode characters and mixfix notation. This makes writing concise and readable programs easy. For example

```agda
data Bool : Set where
  true  : Bool
  false : Bool

if_then_else_ : {A : Set} → Bool → A → A → A
if true  then x else y = x
if false then x else y = y
```

Here we define a data type `Bool` that has type `Set`. `Set` is a built-in type whose inhabitants are [small types](https://ncatlab.org/nlab/show/type+of+types). You can construct an inhabitant of `Bool` in one of two ways: `true` and `false`.

`if_then_else_` is a function that accepts one implicit argument `A` of type `Set` and three explicit arguments of type `Bool`, `A`, and `A`. Finally it returns an inhabitant of type `A`. Notice that each `_` represents a slot for an argument. The function is defined by case-matching on its arguments.

## "hello world" in Agda

The first thing that most Agda guides teach you is actually *not* the familiar IO-based program that prints "hello world" to the console. One reason may be that IO in Agda is actually somewhat subtle because Agda is a [purely functional language](https://en.wikipedia.org/wiki/Purely_functional_programming). I suspect the other reason is that most people who are interested in Agda are logicians who only need the type-checker to verify the logical content of the program.

Since the purpose of this article is to demonstrate some basic proofs in Agda, I too will be side-stepping "hello world" and beginning with a definition for Natural numbers.

```agda
data Nat : Set where
  zero : Nat
  suc  : Nat → Nat
```

Here we define a type called `Nat` to represent natural numbers. It has two constructors: `zero` takes no arguments and is used to create a `Nat` from nothing. `suc` takes a single `Nat` and returns another `Nat`, allowing you to "build" a sequence of natural numbers.

It's now possible to define the proposition stating that one `Nat` is less than or equal to another `Nat`.

```agda
data _≤_ : Nat → Nat → Set where
  ≤-zero : {n   : Nat} → zero ≤ n
  ≤-suc  : {m n : Nat} → m ≤ n → suc m ≤ suc n
```

We use the notion of [propositions as types](https://en.wikipedia.org/wiki/Curry%E2%80%93Howard_correspondence) in the sense that `≤` is a function that accepts two `Nat`s and returns a type. The type is interpreted as a proposition and the inhabitants of that type are interpreted as proofs of that proposition.

The two constructors create proof objects from `Nat`s and represent the semantic notion of `≤`:

1. `≤-zero` states that `zero` is less than or equal to any `Nat`
2. `≤-suc` states that if `m` and `n` are both `Nat`s and `m` is than or equal to `n`, then the successor of `m` is less than or equal to the successor of `n`.

The second constructor can also be interpreted as a function that takes a proof of `m ≤ n` and returns a proof of `suc m ≤ suc n`.

## Basic Proofs in Agda

Let's prove the trivial statement that any `Nat` is less than or equal to itself.

```agda
n≤n : (n : Nat) → n ≤ n
n≤n zero    = ≤-zero
n≤n (suc n) = ≤-suc (n≤n n)
```

The logical interpretation of the type signature is that `n≤n` is the statement "being a `Nat` implies that `n ≤ n`". This statement is then proven by case-matching and recursion where we see the constructors of `≤` in action.

`n≤n zero` generates a proof `zero ≤ zero` by calling the `≤-zero` constructor while `n≤n (suc n)` by calling the `≤-suc` constructor on the proof returned by `n≤n n`.

We can also prove `n ≤ suc n`:

```agda
n≤Sn : (n : Nat) → n ≤ suc n
n≤Sn zero    = ≤-zero
n≤Sn (suc n) = ≤-suc (n≤Sn n)
```

## Conclusion

In this post we introduced some basic features of the Agda language including data types and functions. We also demonstrated that the type-checker can be used to verify proofs using the propositions-as-types interpretation.

## References

* [Agda Tutorial](http://www.cse.chalmers.se/~ulfn/papers/afp08/tutorial.pdf)
* [Dependent Types at Work](http://www.cse.chalmers.se/~peterd/papers/DependentTypesAtWork.pdf)

