---
title: Univalent Mathematics in Agda
---

# Equivalences between standard finite types

```agda
{-# OPTIONS --without-K --exact-split #-}

module elementary-number-theory.equivalences-standard-finite-types where

open import elementary-number-theory.addition-natural-numbers using (add-ℕ)
open import elementary-number-theory.multiplication-natural-numbers using
  ( exp-ℕ; mul-ℕ)
open import elementary-number-theory.natural-numbers using (ℕ; zero-ℕ; succ-ℕ)
open import elementary-number-theory.standard-finite-types using (Fin)
open import foundation.type-arithmetic-coproduct-types using
  ( inv-assoc-coprod; right-distributive-prod-coprod)

open import foundation.cartesian-product-types using (_×_)
open import foundation.contractible-types using (equiv-is-contr)
open import foundation.coproduct-types using (coprod)
open import foundation.equivalences using (_≃_; id-equiv; _∘e_; inv-equiv)
open import foundation.functoriality-cartesian-product-types using
  ( equiv-prod)
open import foundation.functoriality-coproduct-types using (equiv-coprod)
open import foundation.type-arithmetic-empty-type using
  ( right-unit-law-coprod; left-absorption-prod; inv-left-unit-law-coprod)
open import foundation.type-arithmetic-unit-type using (left-unit-law-prod)
open import foundation.unit-type using (unit; is-contr-unit)
open import foundation.universal-property-coproduct-types using
  ( equiv-universal-property-coprod)
open import foundation.universal-property-empty-type using
  ( universal-property-empty')
open import foundation.universal-property-unit-type using
  ( equiv-universal-property-unit)
```

# Equivalences between standard finite types

```agda
coprod-Fin :
  (k l : ℕ) → coprod (Fin k) (Fin l) ≃ Fin (add-ℕ k l)
coprod-Fin k zero-ℕ = right-unit-law-coprod (Fin k)
coprod-Fin k (succ-ℕ l) =
  (equiv-coprod (coprod-Fin k l) id-equiv) ∘e inv-assoc-coprod

Fin-add-ℕ :
  (k l : ℕ) → Fin (add-ℕ k l) ≃ coprod (Fin k) (Fin l)
Fin-add-ℕ k l = inv-equiv (coprod-Fin k l)

prod-Fin : (k l : ℕ) → ((Fin k) × (Fin l)) ≃ Fin (mul-ℕ k l)
prod-Fin zero-ℕ l = left-absorption-prod (Fin l)
prod-Fin (succ-ℕ k) l =
  ( ( coprod-Fin (mul-ℕ k l) l) ∘e
    ( equiv-coprod (prod-Fin k l) left-unit-law-prod)) ∘e
  ( right-distributive-prod-coprod (Fin k) unit (Fin l))

Fin-mul-ℕ : (k l : ℕ) → (Fin (mul-ℕ k l)) ≃ ((Fin k) × (Fin l))
Fin-mul-ℕ k l = inv-equiv (prod-Fin k l)

function-Fin :
  (k l : ℕ) → (Fin k → Fin l) ≃ Fin (exp-ℕ l k)
function-Fin zero-ℕ l =
  ( inv-left-unit-law-coprod unit) ∘e
  ( equiv-is-contr (universal-property-empty' (Fin l)) is-contr-unit)
function-Fin (succ-ℕ k) l =
  ( ( prod-Fin (exp-ℕ l k) l) ∘e
    ( equiv-prod (function-Fin k l) (equiv-universal-property-unit (Fin l)))) ∘e
  ( equiv-universal-property-coprod (Fin l))

Fin-exp-ℕ : (k l : ℕ) → Fin (exp-ℕ l k) ≃ (Fin k → Fin l)
Fin-exp-ℕ k l = inv-equiv (function-Fin k l)
```