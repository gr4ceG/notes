*Example. **FOL** of integer aritmetics*
- Domain: $\Z$
- Individual Sym: $0, 1, -42, ...$
- Function sym: $+, *$
- Relation sym: $=, <$

Translation from English sentences to FOL formula: 
| English | FOL formula |
|---|---|
| The sum of any number with itself is even | $\forall x \exists y (x+x=2*y)$ |
| There are infinitely many even numbers | $\forall x \exists y (x < y \wedge \exists z(y=2 * x))$ |
| There is not a unique number x such that for every $y, x+y=0$ | $\neg \exists x (\forall y(x+y=0) \wedge \forall x'(\forall y(x'+y)=0 \implies x=x'))$ |
| There is a number $x$ such that for every $y, x+y=0$, but this number is unique | $\exists x( \forall y(x+y=0) \wedge \forall x'(\forall y(x'+y=0) \implies x=x'))$|

Order of Quantifiers matter: Ex. For every y there exists $x$ such that $x+y=0$: 
$$\forall y, \exists x (x+y=0) \neq \exists x \forall y (x+y=0)$$

> $Form(L^p)$: 6 rules, (prop. symbols, connectives, pucntuatiion "()")

> $Form(L)$: individual symbols, function symbols, relational symbols, variables, connectives, quantifiers, punctuation "`()`", "`,`"

## Inductive Definition of $Form(L)$
- Constructed using atoms, connectives, quantifiers and punctuations 
- Formation rules (8 rules) - Rules 1-6 are inherited from $Form(L^p)$
1. An atom in $Atom(L)$ is in $Form(L)$
2. If $A \in Form(L)$, then $\neg A \in Form(L)$

3-6. If $A,B \in Form(L)$, then $A \ast B \in Form(L)$, where $* \in \{\wedge ,\vee , \implies ,\iff \}$

7. If $If A(u)\in Form(L)$, then $\forall xA(x) \in Form(L)$
8. If $If A(u)\in Form(L)$, then $\exists xA(x) \in Form(L)$

## Free vs. Bound Variables
| X | Type of Variable |
|---|---|
| $\forall x$ $A(x)$ | $x$ - bound variable |
| $\exists xA(x)$ | $\forall x, \exists x$  - quantifiers/binders |
| | $A(x)$ - the scope of the quantifiers |
| $A(u)$ | $u$ - from vars |

## Inductive Definition of $Atom(L)$
- Constructed using relational symbols, terms, and punctuation
- Smallest bind of formulas having truth values $0, 1$

Rules: 
- If R is a relational symbol, and id $t_1,...,t_n\in Term(L)$, then $R( t_1,...,t_n ) \in Term(L)$
- If $t_1,t_2 \in Term(L)$, then $t_1 \approx t_2 \in Atom(L)$

## Inductive Definition of $Term(L)$
- constructed using free variables, func/cnd symbols, punctation 
- placeholder for domain elements 

3 rules (recursive): 
1. *Base case 1*: If $a$ is an individual symbol, then $a \in Term(L)$
2. *Base case 2*: If $u$ is a free variable, then $u \in Term(L)$
3. If $f$ is a function symbol, and if $t_1,...,t_n \in Term(L)$, then $f( t_1,...,t_n ) \in Term(L)$

*Example*: 

Terms: 
- $u$ (Base case 2: free variable), 
- $0$ (Base case 1: individual symbol)
- $f(u)$ (Recursive case: function)
- $f(1)+f(f(u))$

Atoms: 
- 1 = 0 (connecting two terms using a relation symbol)
- 1 < 0
- 1 < f(1) + 3

### Open vs. Closed Formulas , Opened vs. Closed Terms
> **Open**: if the formula/term has $\geq 1$ free variable; **closed** otherwise

Closed formulas = sentences $Sent(L)$

| Example | Type of Formula (Open, closed) explanation |
|---|---|
| $(u> 0)$ | atomic formula, $u$ is a free variable; hence open formula |
| $\exists x$ $(x + 1 = f(o))$ scope of $\exists x$ | closed formula sentence, $x$ is bounded by $\exists x$ |
| $\forall x \exists y f(u)=x+y$ | $u$ is free $\implies$ open formula |

Note: 
- $\forall x \exists y$ $f(u)=x+y$ scope of $\exists y$
- $\forall x$ $\exists y f(u)=x+y$ scope of $\forall x$

### Precedence Rules
$\forall x, \exists x$ bind tightest (have highest precedence)
- $\forall x$ $A(x)$ $\wedge B(x) \implies C(x)$ scope of A
  - Also equivalent to $(\forall x A(x)) \wedge B(x) \implies C(x)$
- $\forall x(...)$

### Syntax tree 

```mermaid
graph TD;
    $\forall x (F(x) \implies \exists y G(y, f(x)))$-->$(F(u) \implies \exists y G(y, f(u)))$;
    $(F(u) \implies \exists y G(y, f(u)))$-->$F(u)$;
```
...

## Semantics of FOL formulas
A *valuation* $v$ consists of: 
- A domain $D$
- an interpretation/assignment for: 
  - each individual symbol $a$, $a^v \in D$
  - each function symbol $f, f^v: D^n \to D$
  - each Relational symbol $R, R^v \subseteq D^n$
  - each free variable $u, u^v \in D$

### Cartesian Product 
$D^n = D \times D \times ... \times D = \{ (x_1,...,x_n) | x_i \in D \text{ for all } i \in\{ 1, ..., n\}\}$ (n D's)

Example: $D=\{0,1\}$ 

$D^2=\{0,1\} \times \{0,1\} = \{(0,0), (0,1), (1,0), (1,1)\} =^v$ defined as $\{ (0,0), (1,1)\} \subseteq D^2$ (Binary relation)

$\{0\}, \{1\}, \emptyset , \{0,1\} \subseteq D$ (urnanry relations)

## Evaluate Terms, Atoms, and Formulas

