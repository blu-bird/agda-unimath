---
title: Necklaces
---

```agda
{-# OPTIONS --without-K --exact-split #-}

module univalent-combinatorics.necklaces where

open import elementary-number-theory.natural-numbers

open import foundation.dependent-pair-types
open import foundation.equivalences
open import foundation.function-extensionality
open import foundation.functions
open import foundation.homotopies
open import foundation.identity-types
open import foundation.structure-identity-principle
open import foundation.universe-levels

open import structured-types.types-equipped-with-endomorphisms

open import synthetic-homotopy-theory.cyclic-types

open import univalent-combinatorics.finite-types
open import univalent-combinatorics.standard-finite-types
```

## Idea

A necklace is an arrangement of coloured beads. Two necklaces are considered the same if one can be obtained from the other by rotating.

## Definition

### Necklaces

```agda
necklace : (l : Level) → ℕ → ℕ → UU (lsuc l)
necklace l m n = Σ (Cyclic l m) (λ X → type-Cyclic m X → Fin n)

module _
  {l : Level} (m : ℕ) {n : ℕ} (N : necklace l m n)
  where

  cyclic-necklace : Cyclic l m
  cyclic-necklace = pr1 N

  endo-necklace : Endo l
  endo-necklace = endo-Cyclic m cyclic-necklace

  type-necklace : UU l
  type-necklace = type-Cyclic m cyclic-necklace

  endomorphism-necklace : type-necklace → type-necklace
  endomorphism-necklace = endomorphism-Cyclic m cyclic-necklace

  is-cyclic-endo-necklace : is-cyclic-Endo m endo-necklace
  is-cyclic-endo-necklace = mere-equiv-endo-Cyclic m cyclic-necklace

  colouring-necklace : type-necklace → Fin n
  colouring-necklace = pr2 N
```

### Necklace patterns

```agda
necklace-pattern : (l : Level) → ℕ → ℕ → UU (lsuc l)
necklace-pattern l m n =
  Σ (Cyclic l m) (λ X → Σ (UU-Fin n) (λ C → type-Cyclic m X → type-UU-Fin C))
```

## Properties

### Characterization of the identity type

```agda
module _
  {l1 l2 : Level} (m : ℕ) {n : ℕ}
  where
  
  equiv-necklace :
    (N1 : necklace l1 m n) (N2 : necklace l2 m n) → UU (l1 ⊔ l2)
  equiv-necklace N1 N2 =
    Σ ( equiv-Cyclic m (cyclic-necklace m N1) (cyclic-necklace m N2))
      ( λ e →
        ( colouring-necklace m N1) ~
        ( ( colouring-necklace m N2) ∘
          ( map-equiv-Cyclic m
            ( cyclic-necklace m N1)
            ( cyclic-necklace m N2)
            ( e))))

module _
  {l : Level} (m : ℕ) {n : ℕ}
  where

  id-equiv-necklace :
    (N : necklace l m n) → equiv-necklace m N N
  pr1 (id-equiv-necklace N) = id-equiv-Cyclic m (cyclic-necklace m N)
  pr2 (id-equiv-necklace N) = refl-htpy

module _
  {l : Level} (m : ℕ) {n : ℕ}
  where
  
  extensionality-necklace :
    (N1 N2 : necklace l m n) → Id N1 N2 ≃ equiv-necklace m N1 N2
  extensionality-necklace N1 =
    extensionality-Σ
      ( λ {X} f e →
        ( colouring-necklace m N1) ~
        ( f ∘ map-equiv-Cyclic m (cyclic-necklace m N1) X e))
      ( id-equiv-Cyclic m (cyclic-necklace m N1))
      ( refl-htpy)
      ( extensionality-Cyclic m (cyclic-necklace m N1))
      ( λ f → equiv-funext)

  refl-extensionality-necklace :
    (N : necklace l m n) →
    Id (map-equiv (extensionality-necklace N N) refl) (id-equiv-necklace m N)
  refl-extensionality-necklace N = refl
```