---
title: Dependent Type Theory and Agda, part 2
id: agda-pt2
status: published
date: 2020-11-05
---

```agda
module _ where
```

*Read more on literate Agda [here](https://agda.readthedocs.io/en/v2.6.1.1/tools/literate-programming.html#literate-markdown)*

```agda
open import Agda.Builtin.Nat
```

*Instead of defining `Nat` locally, we can import [Agda's builtin version of the type](https://agda.readthedocs.io/en/v2.6.1.1/language/built-ins.html#natural-numbers)*

# Dependent Type Theory and Agda, part 2

[Last time](#agda) I wrote about how depedendent type theory and Agda can express logical information in their type systems. This time I want to demonstrate how to build some basic data structures.

First I will introduce lists and then distinct lists. Finally I will combine the two into a `Map` type.

## Lists

I want `List A` to be interpreted as a list of inhabitants of the type `A`. To do that, it is conventional to use two constructors: one for empty lists and one for appending elements to lists. This syntax allows us to use pattern-matching on these constructors in later definitions.

```agda
data List (A : Set) : Set where
  []  : List A
  _∷_ : A → List A → List A
```

The types of the constructors are interpreted as follows

1. `[]` produces a list of `A`s from nothing.
2. `_∷_` produces a list of `A`s from one `A` and a list of `A`s. Think `head ∷ tail`.

`[]` is interesting because `List A` has the same constructor for every `A`. It seems like `[]` is producing a type that it knows nothing about! I am not sure of the technical details as to why this works, but my intuition is that the type-checker is able to deduce the type from context.

## Distinct Lists

If we want to define a reasonable notion of a `Map`, we should expect that the keys of the map should be distinct. You shouldn't be able to push a new key-value pair into the map unless you have a proof that the key is not already in use.

To handle this, I will introduce distinct lists. These should be interpreted as lists that carry some notion of *not* being an element of that list.

Furthermore, we need some way of *distinguishing* inhabitants of the type on which the list is based.

```agda
data !List⟨_⟩ {A : Set} (_≢_ : A → A → Set) : Set
```

This data type is produced from two arguments. `A` is inferred from type of the other argument `_≢_` which is our distinction relation. Here I assume that producing an inhabitant of `a ≢ b` proves that `a` and `b` are distinct. A more thorough type definition for distinct lists would require additional arguments in the form of proofs about the properties of `_≢_`.

Now we define the `_∉_` proposition for "non-elementhood" or "non-containment".

```agda
data _∉⟨_⟩_ {A : Set} : A → (_≢_ : (A → A → Set)) → !List⟨ _≢_ ⟩ → Set
```

This type is the most complex I've used so far in this series so it warrants some exposé. This type needs to pass around information about which distinction relation it is using. Namely you should not be able to prove `x ∉⟨ ≢₁ ⟩ y ` by supplying `x ≢₂ y`!

I have [separated the type defintitions from the constructor defintions](https://agda.readthedocs.io/en/v2.6.1.1/language/mutual-recursion.html#mutual-recursion-forward-declaration) because the distinct list constructors rely on the type information for `_∉_`, but the constructors for `_∉_` rely on the type information for distinct lists!

```agda
infix 2 _∷_⟨_⟩
data !List⟨_⟩ {A} _≢_ where
  ![]    : !List⟨ _≢_ ⟩
  _∷_⟨_⟩ : (x : A) (xs : !List⟨ _≢_ ⟩) → x ∉⟨ _≢_ ⟩ xs → !List⟨ _≢_ ⟩

infix 1 _∉⟨_⟩_
data _∉⟨_⟩_ {A} where
  ∉-empty : {x : A} {≢ : A → A → Set} → x ∉⟨ ≢ ⟩ ![]
  ∉-x≢head∧x∉tail : {x y : A} {_≢_ : A → A → Set} {xs : !List⟨ _≢_ ⟩}
    → x ≢ y → x ∉⟨ _≢_ ⟩ xs → (y∉xs : y ∉⟨ _≢_ ⟩ xs) → x ∉⟨ _≢_ ⟩ y ∷ xs ⟨ y∉xs ⟩
```

I also use the `infix` syntax which tells the type-checker the order of operations for statements like ` x ∉ y ∷ xs ⟨ y∉xs ⟩`

The inductive construction is that in order to prove that `x` is not in some list, you have to prove that it is distinct from the head and not an element of the tail. The base case says that anything is not in the empty list.

### Another example

Let's look at how to define an inclusion operation, which is much simpler. Here we don't need to define an indistinguishability relation because we can use pattern-matching.

```agda
infix 2 _∷_
infix 1 _∈_
data _∈_ {A} (x : A) : List A → Set where
  ∈-head : {xs : List A} → x ∈ x ∷ xs
  ∈-tail : {xs : List A} {y : A} → x ∈ xs → x ∈ y ∷ xs
```

We can then use this to compute the index of an element!

```agda
indexOf : {A : Set} (x : A) (xs : List A) → x ∈ xs → Nat
indexOf t xs       ∈-head        = zero
indexOf t (x ∷ xs) (∈-tail x∈xs) = suc (indexOf t xs x∈xs)
```

Note that because `indexOf` require a proof of `x ∈ xs`, we are guarnteed to produce a `Nat`. No need for returning `-1` in languages like javascript or using a `Maybe` type *a la* haskell.

## Maps

I'm now ready to define `Map`s, i.e. lists of key-value pairs. Similar to the distinct list and vector types, we require a proof of that key we want to add is not already in the map's keys.

```agda
data Map (K V : Set) (≢ₖ : K → K → Set) : Set
```

Again we separate the type definition from the constructors because we need a crucial fucnction similar to `_∉_` for distinct lists. For the `Map` type we need `keys`.

Instead of writing `{K V : Set} {≢ₖ : A → A → Set}` we can let the type-checker infer the type using `∀`.

```agda
keys : ∀ {K V ≢ₖ} → Map K V ≢ₖ → !List⟨ ≢ₖ ⟩
```

The map type follows:

```agda
data Map K V ≢ₖ where
  ⟦⟧         : Map K V ≢ₖ
  <_⇒_>¦_⟨_⟩ : (k : K) → V → (m : Map K V ≢ₖ)
               → k ∉⟨ ≢ₖ ⟩ keys m → Map K V ≢ₖ

keys ⟦⟧ = ![]
keys < k ⇒ _ >¦ m ⟨ k∉ks ⟩ = k ∷ (keys m) ⟨ k∉ks ⟩
```
`<_⇒_>¦_⟨_⟩ ` allows us to produce a new map with an extra key-value pair given a proof that the key we want to use isn't already in thes keys.

`vals` is the counterpoint to keys:

```agda
vals : ∀ {K V ≢ₖ} → Map K V ≢ₖ → List V
vals ⟦⟧ = []
vals < _ ⇒ v >¦ m ⟨ _ ⟩ = v ∷ (vals m)
```

## Conclusion

In this post I defined some basic data structures in Agda. My key takeaway from this exercise was the way that encoding proof-relevant information into data types can create guarantees that aren't possible in languages without dependent types.

