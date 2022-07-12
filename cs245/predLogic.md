# Predicate Logic
## Introduction and Translations
Why do we use predicate logic? There are ideas we cannot express using propsitional logic: 

    Example 1. Every bear likes honey,
    Example 2. Alice is married to Jay and Alice is not married to Leon

However, we can use *predicate logic (first-order logic)* to express all of these.

Predicate logic generalizes propositional logic
    
    New things in predicate logic: Domains, Predicates, Quantifiers

### Domain
`Domain` : A domain is a *non-empty* set of objects. It is a world that our statement is situated in.

Why is it important to specify a domain? The same statement can have different truth values in different domains

### Predicates 
`Predicates` : A predicate represents
- A property of an individual *(urnary predicate)*
- A relationship among multiple individuals *(binary predicate, etc.)*

A predicate is a special type of function that takes in constants/variables as inputs and *outputs T/F*.

*Examples:*
- Define $L(x)$ to mean "$x$ is a lecturer". *(urnary predicate - property of an indiviudal)*
  - Alice is a lecturer: $L(Alice)$
  - Mel is not a lecturer: $\neg L(Mel)$
  - $y$ is a lecturer: $L(y)$

- Define $Y(x,y)$ to mean "$x$ is younger than $y$" *(binary predicate - relation)*
  - Alex is younger than Sam: $Y(Alex, Sam)$
  - $a$ is younger than $b$: $Y(a,b)$

### Quantifiers
`Quantifiers` : 
- The universal quantifier $\forall$: The statement is true forall objects in the domain
- The existential quantifier $\exists$: The statement is true for one or more objects in the domain 

## Interpreting the quantifiers
Let $D=\{d_1, d_2, ..., d_n\}$

| Quantifier | Equivalence |
|---|---|
| $\forall$ is a big AND | $\forall P(x)$ is equivalent to $P(d_1) \wedge P(d_2) \wedge ... \wedge P(d_n)$ |
| $\exists$ is a big OR | $\exists P(x)$ is equivalent to $P(d_1) \vee P(d_2) \vee ... \vee P(d_n)$ |

### Negating a quantifier formula
"Generalized De Morgan's Law"
- $\neg (\forall x P(x)) \equiv \exists x (\neg P(x))$
- $\neg (\exists x P(x)) \equiv \forall x (\neg P(x))$

FOR ALL AND THERE EXISTS DIFFERENCE - PATRICK ROH

### At least, At most, and exactly
Let the domain be the set of animals. Let $B(x)$ be that $x$ is a bear:

    1. There are at least two bears
    2. There are at most one bear
    3. There is exactly one bear 

| English | FOL Translation |
|---|---|
| There are at least two bears. | $\exists x(\exists y((B(x) \wedge B(y)) \wedge (x \neq y)))$ |
| There is at most one bear | <ul><li>$\neg (\exists x(\exists y((B(x) \wedge B(y)) \wedge (x \neq y))))$: There does not exists two different bears</li><li>$\forall x(\forall y((B(x) \wedge B(y)) → (x=y)))$: If there are two bears, they must be the same</li></ul> |
| There is exactly one bear | $\exists x (B(x) \wedge (\forall y (B(y) → (x = y))))$ |

### Multiple Quantifiers - Watch Out!
... love example

## Translating English into Predicate Logic - A Summary
| English | Translation |
|---|---|
