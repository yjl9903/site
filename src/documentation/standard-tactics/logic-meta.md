---
title: \module Logic.Meta
---

# `contradiction`

 [scope-meta]: /documentation/standard-tactics/meta#scope-metas

This meta will try to derive a contradiction from the context if no arguments are specified.
Implicit `using`, `usingOnly` and `hiding` (description of these metas can be found [here][scope-meta]) arguments can be used to modify the context it uses.
Examples (requires arend-lib):

{% arend %}
\import Logic.Meta
\import Logic
\import Meta
\import Order.StrictOrder
\import Set

\lemma equality {x y : Nat} (p : x = y) (q : Not (x = y)) : Empty
  => contradiction

\lemma irreflexivity {A : Set#} {x y : A} (p : x # x) : Empty
  => contradiction

\lemma irreflexivity2 {A : Set#} {x y : A} (p : x # y) (q : x = y) : Empty
  => contradiction

\lemma irreflexivity3 {A : StrictPoset} {x y z : A} (p : x < y) (q : x = z) (s : y = z) : Empty
  => contradiction

\lemma usingTest (P Q : \Prop) (q : Q) (e : P -> Empty) (p : P) : Empty
  => contradiction {usingOnly (e,p)}
{% endarend %}

It can also be specified with an implicit argument which itself is a contradiction.
Examples:

{% arend %}
\lemma explicitProof (P : \Prop) (e : Empty) : P
  => contradiction {e}

\lemma explicitProof2 (P : \Prop) (e : P -> Not P) (p : P) : Empty
  => contradiction {e}
{% endarend %}

# `Exists`

The alias of this meta is `∃`, which is convenient to read.

+ {%ard%} Exists {x y z} B {%endard%} is equivalent to {%ard%} TruncP (\Sigma (x y z : _) B) {%endard%}.
+ {%ard%} Exists (x y z : A) B {%endard%} is equivalent to {%ard%} TruncP (\Sigma (x y z : A) B) {%endard%}.

Examples:

{% arend %}
\func test1 : ∃ = (\Sigma) => idp

\func test2 : ∃ Nat = TruncP Nat => idp

\func test3 : ∃ (x : Nat) (x = 0) = TruncP (\Sigma (x : Nat) (x = 0)) => idp

\func test4 : ∃ {x} (x = 0) = TruncP (\Sigma (x : Nat) (x = 0)) => idp

\func test5 : ∃ {x y} (x = 0) = TruncP (\Sigma (x y : Nat) (x = 0)) => idp
{% endarend %}

# `constructor`

Returns either a tuple, a `\new` expression, or a single constructor of a data type depending on the expected type.

{% arend %}
\func tuple0 : \Sigma (\Sigma) (\Sigma) => constructor constructor constructor

\func tuple1 : \Sigma Nat Nat => constructor 0 1

\func tuple2 : \Sigma (x : Nat) (p : x = 0) => constructor 0 idp

\func data2 : D => constructor 0 1 2
  \where
    \data D | con {x : Nat} (y z : Nat)

\record R (x : Nat) (y : Nat) (z : Nat)

\func class1 : R => constructor 1 2 3

\func class2 : R 1 => constructor 2 3
{% endarend %}
