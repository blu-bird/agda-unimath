# Subgroups generated by elements of a group

```agda
module group-theory.subgroups-generated-by-elements-groups where
```

<details><summary>Imports</summary>

```agda
open import elementary-number-theory.group-of-integers
open import elementary-number-theory.integers

open import foundation.dependent-pair-types
open import foundation.function-types
open import foundation.identity-types
open import foundation.images-subtypes
open import foundation.logical-equivalences
open import foundation.singleton-subtypes
open import foundation.subtypes
open import foundation.universe-levels

open import group-theory.free-groups-with-one-generator
open import group-theory.groups
open import group-theory.homomorphisms-groups
open import group-theory.images-of-group-homomorphisms
open import group-theory.integer-powers-of-elements-groups
open import group-theory.subgroups
open import group-theory.subgroups-generated-by-subsets-groups
open import group-theory.subsets-groups
open import group-theory.trivial-subgroups
```

</details>

## Idea

The **subgroup generated by an element** `x` of a
[group](group-theory.groups.md) `G` is the
[subgroup generated by](group-theory.subgroups-generated-by-subsets-groups.md)
the [standard singleton subset](foundation.singleton-subtypes.md) `{x}`. In
other words, it is the least [subgroup](group-theory.subgroups.md) of `G` that
contains the element `x`. In informal writing, the subgroup generated by `x` is
denoted by `⟨x⟩`.

## Definitions

### The predicate of being a subgroup generated by an element

```agda
module _
  {l1 : Level} (G : Group l1) (x : type-Group G)
  where

  is-subgroup-generated-by-element-Group :
    {l2 : Level} (H : Subgroup l2 G) → UUω
  is-subgroup-generated-by-element-Group H =
    {l : Level} (K : Subgroup l G) → is-in-Subgroup G K x ↔ leq-Subgroup G H K

  contains-element-is-subgroup-generated-by-element-Group :
    {l2 : Level} (H : Subgroup l2 G) →
    is-subgroup-generated-by-element-Group H →
    is-in-Subgroup G H x
  contains-element-is-subgroup-generated-by-element-Group H α =
    backward-implication (α H) (refl-leq-Subgroup G H)

  leq-subgroup-is-subgroup-generated-by-element-Group :
    {l2 l3 : Level} (H : Subgroup l2 G) (K : Subgroup l3 G) →
    is-subgroup-generated-by-element-Group H →
    is-in-Subgroup G K x → leq-Subgroup G H K
  leq-subgroup-is-subgroup-generated-by-element-Group H K α =
    forward-implication (α K)
```

### The subgroup generated by an element of a group

```agda
module _
  {l1 : Level} (G : Group l1) (x : type-Group G)
  where

  generating-subset-subgroup-element-Group : subset-Group l1 G
  generating-subset-subgroup-element-Group =
    subtype-standard-singleton-subtype (set-Group G) x

  subgroup-element-Group : Subgroup l1 G
  subgroup-element-Group =
    subgroup-subset-Group G generating-subset-subgroup-element-Group

  subset-subgroup-element-Group : subset-Group l1 G
  subset-subgroup-element-Group = subset-Subgroup G subgroup-element-Group

  is-in-subgroup-element-Group : type-Group G → UU l1
  is-in-subgroup-element-Group = is-in-Subgroup G subgroup-element-Group

  is-closed-under-eq-subgroup-element-Group :
    {y z : type-Group G} → is-in-subgroup-element-Group y →
    y ＝ z → is-in-subgroup-element-Group z
  is-closed-under-eq-subgroup-element-Group =
    is-closed-under-eq-Subgroup G subgroup-element-Group

  is-closed-under-eq-subgroup-element-Group' :
    {y z : type-Group G} → is-in-subgroup-element-Group z →
    y ＝ z → is-in-subgroup-element-Group y
  is-closed-under-eq-subgroup-element-Group' =
    is-closed-under-eq-Subgroup' G subgroup-element-Group

  contains-unit-subgroup-element-Group :
    contains-unit-subset-Group G subset-subgroup-element-Group
  contains-unit-subgroup-element-Group =
    contains-unit-Subgroup G subgroup-element-Group

  is-closed-under-multiplication-subgroup-element-Group :
    is-closed-under-multiplication-subset-Group G subset-subgroup-element-Group
  is-closed-under-multiplication-subgroup-element-Group =
    is-closed-under-multiplication-Subgroup G subgroup-element-Group

  is-closed-under-inverses-subgroup-element-Group :
    is-closed-under-inverses-subset-Group G subset-subgroup-element-Group
  is-closed-under-inverses-subgroup-element-Group =
    is-closed-under-inverses-Subgroup G subgroup-element-Group

  abstract
    is-subgroup-generated-by-element-subgroup-element-Group :
      is-subgroup-generated-by-element-Group G x subgroup-element-Group
    is-subgroup-generated-by-element-subgroup-element-Group H =
      logical-equivalence-reasoning
        is-in-Subgroup G H x
        ↔ ( subtype-standard-singleton-subtype (set-Group G) x ⊆
            subset-Subgroup G H)
          by
          inv-iff
            ( is-least-subtype-containing-element-Set
              ( set-Group G)
              ( x)
              ( subset-Subgroup G H))
        ↔ leq-Subgroup G subgroup-element-Group H
          by
          inv-iff
            ( is-subgroup-generated-by-subset-subgroup-subset-Group G
              ( subtype-standard-singleton-subtype (set-Group G) x)
              ( H))

  abstract
    contains-element-subgroup-element-Group :
      is-in-subgroup-element-Group x
    contains-element-subgroup-element-Group =
      contains-subset-subgroup-subset-Group G
        ( subtype-standard-singleton-subtype (set-Group G) x)
        ( x)
        ( refl)

  abstract
    leq-subgroup-element-Group :
      {l : Level} (H : Subgroup l G) →
      is-in-Subgroup G H x → leq-Subgroup G subgroup-element-Group H
    leq-subgroup-element-Group H =
      forward-implication
        ( is-subgroup-generated-by-element-subgroup-element-Group H)
```

### The image of the initial group homomorphism `f : ℤ → G` such that `f(1) ＝ g`

An alternative definition of the subgroup generated by one element `g` is as the
image of the
[initial group homomorphism](group-theory.free-groups-with-one-generator.md)
`f : ℤ → G` that satisfies `f(1) ＝ g`.

```agda
module _
  {l1 : Level} (G : Group l1) (x : type-Group G)
  where

  image-hom-element-Group : Subgroup l1 G
  image-hom-element-Group = image-hom-Group ℤ-Group G (hom-element-Group G x)

  subset-image-hom-element-Group : subset-Group l1 G
  subset-image-hom-element-Group =
    subset-Subgroup G image-hom-element-Group

  is-in-image-hom-element-Group : type-Group G → UU l1
  is-in-image-hom-element-Group = is-in-Subgroup G image-hom-element-Group

  is-closed-under-eq-image-hom-element-Group :
    {y z : type-Group G} → is-in-image-hom-element-Group y →
    y ＝ z → is-in-image-hom-element-Group z
  is-closed-under-eq-image-hom-element-Group =
    is-closed-under-eq-Subgroup G image-hom-element-Group

  is-closed-under-eq-image-hom-element-Group' :
    {y z : type-Group G} → is-in-image-hom-element-Group z →
    y ＝ z → is-in-image-hom-element-Group y
  is-closed-under-eq-image-hom-element-Group' =
    is-closed-under-eq-Subgroup' G image-hom-element-Group

  is-image-image-hom-element-Group :
    is-image-hom-Group ℤ-Group G (hom-element-Group G x) image-hom-element-Group
  is-image-image-hom-element-Group =
    is-image-image-hom-Group ℤ-Group G (hom-element-Group G x)

  contains-values-image-hom-element-Group :
    (k : ℤ) → is-in-image-hom-element-Group (map-hom-element-Group G x k)
  contains-values-image-hom-element-Group =
    contains-values-image-hom-Group ℤ-Group G (hom-element-Group G x)

  contains-element-image-hom-element-Group :
    is-in-image-hom-element-Group x
  contains-element-image-hom-element-Group =
    is-closed-under-eq-image-hom-element-Group
      ( contains-values-image-hom-element-Group one-ℤ)
      ( right-unit-law-mul-Group G x)

  leq-image-hom-element-Group :
    {l : Level} (H : Subgroup l G) →
    is-in-Subgroup G H x → leq-Subgroup G image-hom-element-Group H
  leq-image-hom-element-Group H u =
    leq-image-hom-Group
      ( ℤ-Group)
      ( G)
      ( hom-element-Group G x)
      ( H)
      ( λ k → is-closed-under-powers-int-Subgroup G H k x u)

  is-subgroup-generated-by-element-image-hom-element-Group :
    is-subgroup-generated-by-element-Group G x image-hom-element-Group
  pr1 (is-subgroup-generated-by-element-image-hom-element-Group K) =
    leq-image-hom-element-Group K
  pr2 (is-subgroup-generated-by-element-image-hom-element-Group K) u =
    u x contains-element-image-hom-element-Group
```

## Properties

### The subgroup generated by an element `g` contains all the integer powers `gᵏ`

```agda
module _
  {l1 : Level} (G : Group l1) (x : type-Group G)
  where

  contains-powers-subgroup-element-Group :
    (k : ℤ) → is-in-subgroup-element-Group G x (integer-power-Group G k x)
  contains-powers-subgroup-element-Group k =
    is-closed-under-powers-int-Subgroup G
      ( subgroup-element-Group G x)
      ( k)
      ( x)
      ( contains-element-subgroup-element-Group G x)
```

### Any two subgroups generated by the same element contain the same elements

```agda
module _
  {l1 l2 l3 : Level} (G : Group l1) (x : type-Group G)
  (H : Subgroup l2 G) (Hu : is-subgroup-generated-by-element-Group G x H)
  (K : Subgroup l3 G) (Ku : is-subgroup-generated-by-element-Group G x K)
  where

  has-same-elements-is-subgroup-generated-by-element-Group :
    has-same-elements-Subgroup G H K
  pr1 (has-same-elements-is-subgroup-generated-by-element-Group y) =
    leq-subgroup-is-subgroup-generated-by-element-Group G x H K Hu
      ( contains-element-is-subgroup-generated-by-element-Group G x K Ku)
      ( y)
  pr2 (has-same-elements-is-subgroup-generated-by-element-Group y) =
    leq-subgroup-is-subgroup-generated-by-element-Group G x K H Ku
      ( contains-element-is-subgroup-generated-by-element-Group G x H Hu)
      ( y)
```

### Any subgroup that has the same elements as the subgroup generated by `x` satisfies the universal property of the subgroup generated by `x`

```agda
module _
  {l1 l2 : Level} (G : Group l1) (x : type-Group G) (H : Subgroup l2 G)
  where

  is-subgroup-generated-by-element-has-same-elements-Subgroup :
    has-same-elements-Subgroup G (subgroup-element-Group G x) H →
    is-subgroup-generated-by-element-Group G x H
  pr1 (is-subgroup-generated-by-element-has-same-elements-Subgroup p U) u =
    transitive-leq-Subgroup G H (subgroup-element-Group G x) U
      ( forward-implication
        ( is-subgroup-generated-by-element-subgroup-element-Group G x U)
        ( u))
      ( leq-has-same-elements-Subgroup' G (subgroup-element-Group G x) H p)
  pr2 (is-subgroup-generated-by-element-has-same-elements-Subgroup p U) q =
    q x
      ( forward-implication (p x) (contains-element-subgroup-element-Group G x))
```

### The subgroup generated by an element has the same elements as the image of the initial group homomorphism `ℤ → G` mapping `1` to `g`

```agda
module _
  {l : Level} (G : Group l) (x : type-Group G)
  where

  has-same-elements-image-hom-element-subgroup-element-Group :
    has-same-elements-Subgroup G
      ( image-hom-element-Group G x)
      ( subgroup-element-Group G x)
  has-same-elements-image-hom-element-subgroup-element-Group =
    has-same-elements-is-subgroup-generated-by-element-Group G x
      ( image-hom-element-Group G x)
      ( is-subgroup-generated-by-element-image-hom-element-Group G x)
      ( subgroup-element-Group G x)
      ( is-subgroup-generated-by-element-subgroup-element-Group G x)
```

### The image of the subgroup `⟨x⟩` generated by `x` under a group homomorphism `f` is the subgroup `⟨f(x)⟩` generated by the element `f(x)`

There are three ways to state this fact:

1. The image of the subgroup `⟨x⟩` under the group homomorphism `f` has the same
   elements as the subgroup `⟨f(x)⟩`.
2. The subgroup `⟨f(x)⟩` satisfies the universal property of the image of the
   group `⟨x⟩` under the group homomorphism `f`.
3. The image of the subgroup `⟨x⟩` under the group homomorphism `f` satisfies
   the universal property of the subgroup generated by the element `f x`.

```agda
module _
  {l1 l2 : Level} (G : Group l1) (H : Group l2) (f : hom-Group G H)
  (x : type-Group G)
  where

  compute-image-subgroup-element-Group :
    has-same-elements-Subgroup H
      ( im-hom-Subgroup G H f (subgroup-element-Group G x))
      ( subgroup-element-Group H (map-hom-Group G H f x))
  compute-image-subgroup-element-Group h =
    logical-equivalence-reasoning
      is-in-im-hom-Subgroup G H f (subgroup-element-Group G x) h
      ↔ is-in-subgroup-subset-Group H
          ( im-subtype
            ( map-hom-Group G H f)
            ( subtype-standard-singleton-subtype (set-Group G) x))
          ( h)
        by
        compute-image-subgroup-subset-Group G H f
          ( subtype-standard-singleton-subtype (set-Group G) x)
          ( h)
      ↔ is-in-subgroup-element-Group H (map-hom-Group G H f x) h
        by
        inv-iff
          ( has-same-elements-subgroup-subset-has-same-elements-subset-Group H
            ( generating-subset-subgroup-element-Group H _)
            ( im-subtype
              ( map-hom-Group G H f)
              ( subtype-standard-singleton-subtype (set-Group G) x))
            ( compute-im-singleton-subtype
              ( set-Group G)
              ( set-Group H)
              ( map-hom-Group G H f)
              ( x))
            ( h))

  is-image-subgroup-element-Group :
    is-image-subgroup-hom-Group G H f
      ( subgroup-element-Group G x)
      ( subgroup-element-Group H (map-hom-Group G H f x))
  is-image-subgroup-element-Group =
    is-image-subgroup-has-same-elements-Subgroup G H f
      ( subgroup-element-Group G x)
      ( subgroup-element-Group H (map-hom-Group G H f x))
      ( compute-image-subgroup-element-Group)

  is-subgroup-generated-by-element-image-subgroup-element-Group :
    is-subgroup-generated-by-element-Group H
      ( map-hom-Group G H f x)
      ( im-hom-Subgroup G H f (subgroup-element-Group G x))
  is-subgroup-generated-by-element-image-subgroup-element-Group =
    is-subgroup-generated-by-element-has-same-elements-Subgroup H
      ( map-hom-Group G H f x)
      ( im-hom-Subgroup G H f (subgroup-element-Group G x))
      ( inv-iff ∘ compute-image-subgroup-element-Group)
```

### The subgroup `⟨x⟩` is trivial if and only if `unit-Group G ＝ x`

```agda
module _
  {l1 : Level} (G : Group l1)
  where

  is-trivial-subgroup-unit-Group :
    is-trivial-Subgroup G (subgroup-element-Group G (unit-Group G))
  is-trivial-subgroup-unit-Group =
    leq-subgroup-element-Group G (unit-Group G) (trivial-Subgroup G) refl

  is-trivial-subgroup-element-Group :
    (x : type-Group G) →
    is-unit-Group' G x → is-trivial-Subgroup G (subgroup-element-Group G x)
  is-trivial-subgroup-element-Group ._ refl = is-trivial-subgroup-unit-Group

  is-unit-is-trivial-subgroup-element-Group :
    (x : type-Group G) →
    is-trivial-Subgroup G (subgroup-element-Group G x) → is-unit-Group' G x
  is-unit-is-trivial-subgroup-element-Group x H =
    H x (contains-element-subgroup-element-Group G x)
```
