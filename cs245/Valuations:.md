<style>
r { color: #F95050 }
navy { color: #000099 }
b { color: #50C1F9 }
o { color: #F7A63B }
g { color: #48D03E }
</style>

# Valuations
Valuations: 
- A domain (non-empty set)
- An interpretation for every rel/fun/ind symbol, free var

## Values of terms $Term(L)$
| Meaning | Notation |
|---|---|
| $t$ is an individual symbol a | \(t^v = a^v\) |
| $t$ is an free var u | $t^v = u^v$ |
| $t$ is $f(t_1 ... t_n)$ | $t^v = f^v({t_1}^v...{t_n}^v)$  $F:D^n \to D$ |

**Fact**: If $t\in term(L)$ and $t^v$ is defined, then $t^v \in D$. (By structural indiction)

## Values of Formulas $Form(L)$

| A | Truth valuations |
|---|---|
| A is an atomic formula $R(t_1 ... t_n)$ | $R(t_1 ... t_n) = \begin{cases} 1, \text{if } ({t_1}^v...{t_n}^v \in R^v \subseteq D^n \\ 0, \text{otherwise} \end{cases}$ |
| A is $(\neg B)$ | $(\neg B) = \begin{cases} 1, \text{if } B^v=0,\\ 0, \text{otherwise} \end{cases}$ |
| A is $(B*C)$ | $(B*C)^v=...$ (Inherited from prop logic) |
| A is $\forall xB(x)$ | $\forall xB(x) = \begin{cases} 1, \text{if for every } d\in D, B(d)^v=1,\\ 0, \text{otherwise} \end{cases}$ |
| A is $\exists xB(x)$ | $\exists xB(x) = \begin{cases} 1, \text{if thereexists } d\in D, B(d)^v=1,\\ 0, \text{otherwise} \end{cases}$ |

**Fact**: If $A\in Form(L)$, and if $A^n$ is defined, then $A^v\in \{0,1\}$ (By structural induction).

*Example*. Let $A=\exists x(F(x) \to G(x))$. Let $D=\{a,b\}$. Create a valuation $v_1$ such that $A^{v_1}=1$.

*Solution:*
$$\text{Define } F^{v_1}=\{a\}, G^{v_1}=\{a\},$$
$$ \text{Then } G(a)^{v_1}=1 \implies {(F(a) \to G(a))}^{v_1}=1.$$
$$\text{So } {\exists x(F(x) \to G(x))}^{v_1}=1$$

*Solution':* 

$$\text{Define } F^{v_1}= \emptyset$$
$$\text{Then } F(a)^{v_1}=0 \implies {(F(a) \to G(a))}^{v_1}=1$$
$$\text{So } {\exists x(F(x) \to G(x))}^{v_1}=1$$

Create a valuation $v_2$ such that $A^{v_2}=0$.
$$ \text{Want: for every } d\in D {(F(d)\to G(d))}^{v_2}=0$$
$$ F(d)^{v_2}=1, G(d)^{v_2}=1 $$
$$ \text{Let } G^{v_2}=\emptyset. \text{ Let } F^{v_2}=\{a,b\}=D$$. 

Then 
$$ F(a)^{v_2} = 1, G(a)^{v_2} = 0 \implies (F(a) \implies G(a))^{v_2} = 0 \\ F(b)^{v_2} = 1, G(b)^{v_2} = 0 \implies (F(b) \implies G(b))^{v_2} = 0$$

So for every $d\in D$, $(F(d) \implies G(d))^{v_2}=0$.

# Satisfiability 
> <r>**Defn**: $A \in Form(L)$ is **satisfiable** if there exists $v$ such that $A^v=1$.</r> 

\(A\) \in Form(L)$ is unsatisfiable if for every $v$, $A^v=0$

> <r>$A \in Form(L)$ is **universally valid** (tautologically true) is for every $v$, $A^v=1$.</r> 

| Example | Satifiabilty/validity |
|---|---|
| $\exists x(F(x)\to G(x))$ | Satisfiable, but not valid |
| $\forall x\forall y (F(x,y) \vee F(y,x))$ | satisfiable, but not valid <ul><li>Proving satisfiability: Let domain $D=\N$. $F^{v_1} = \leq$</li><li>Proving not valid: Let domain $D=\N$. $F^{v_1} = <$</li></ul> | 
| $\forall x\forall y(F(x,y) \wedge \neg F(x,y))$ | unsatisfiable |
| $\forall x\forall y(F(x,y) \vee \neg F(x,y))$ | valid |


> <r>**Defn**: $\Sigma \subseteq Form(L)$. $\Sigma^v=\begin{cases} 1, \text{if for every } A\in \Sigma, A^v=1,\\ 0, \text{otherwise}\end{cases}$.</r>

> <r>**Defn**: $\Sigma \subseteq Form(L)$ is satisfiable if there is a $v$ such that $\Sigma^v=1$.</r>

*Example*. (Partial order): $\Sigma=\begin{cases}
\forall xF(x,x) \text{ Reflexitive}\\
\forall x \forall y(F(x,y) \wedge F(y,y) \implies x=y) \text{ Anti-symmetric}\\
\forall x\forall y\forall z (F(x,y) \wedge F(y,z) \implies F(x,z)) \text{ Transitive}
\end{cases}$

$$ \text{Let } D=\N, F=\leq. \text{ So } \Sigma \text{ is satisfiable} $$
$$ \text{Let } D=\N, F=<. \text{ So } \Sigma \text{ is invalid}. $$. 

*Example*. (Equivalence relation): $\Sigma=\begin{cases}
\forall xF(x,x) \text{ Reflexitive}\\
\forall x \forall y(F(x,y) \wedge F(y,y) \iff x=y) \text{ Symmetric}\\
\forall x\forall y\forall z (F(x,y) \wedge F(y,z) \implies F(x,z)) \text{ Transitive}
\end{cases}$

$$ \text{Let } D=\N, F= =. \text{ So } \Sigma \text{ is satisfiable} $$
$$ \text{Let } D=\N, F=\leq. \text{ So } \Sigma \text{ is invalid}. $$. 

## Logical Consequences ($\vDash$)
> <r>$\Sigma \subseteq Form(L), A \subset Form(L)$. $\Sigma \tau A \iff$ for every $v$ such that $\Sigma^v=1$ we have $A^v=1$</r>

**Facts**: 
- $\emptyset$ is valid
- $\emptyset \vDash A \iff A$ is valid 
- If $\Sigma$ is unsatisfiable, then $\Sigma \vDash A$ is always true

## Logical Equivalence ($|\!\!\!=\!\!\!|$)
> $A |\!\!\!=\!\!\!| B \iff A \vDash B \text{ and } B \vDash A$

*Example*. Prove $\forall x(\neg A(x)) |\!\!\!=\!\!\!| \neg(\exists xA(x))$

*Intuition*: $\neg A(d_1) \vee ... \vee \neg A(d_n) |\!\!\!=\!\!\!| \neg(A(d_1) \wedge ... \wedge A(d_n))$

*Proof*: $\vDash$ direction

For the sake of contradiction, suppose there is a $v$ such that 

$$ (\forall x \neg A(x))^v = 1 \\$$ (1)
$$ (\neg \exists xA(x))^v=0 \\$$ (2)
$$ \text{By (2), } (\exists xA(x))^v=1 $$ (3)
$$ \text{By (3), there is a } d\in D \text{ such that } A(d)^v=1 $$ (4)
$$  \text{By 4, } \neg (A(d))^v=0, \text{ which contradicts (1) }$$ (5)

*Example*. Prove 
$$ \forall x(A(x) \to B(x)) \vDash \forall xA(x) \to \forall xB(x) \\ \forall xA(x) \to B(x) \nvDash \forall x(A(x) \to B(x)) $$. 

*Intuition*: 
$$ {\forall x(A(x) \to B(x))}^v=1 \text{ says } A^v \subseteq B^v \\ {\forall xA(x) \to \forall xB(x)}^v=1 \text{ says if }A^v=D \text{ then } B^v=D $$

*Proof*: $\forall xA(x) \implies \forall xB(x) \nvDash \forall x(A(x) \implies B(x))$

Want: find $A, B, v$ such that 
- $(\forall xA(x) \implies \forall xB(x))^v=1$ ($\forall xA(x)^v=0$)
- $(\forall x(A(x) \to B(x)))^v=0$

Let $D=\{a,b\}$. Let $A(u)=F(u)$ and let $B(u)=G(u)$, where $F,G$ are relation symbols

$F^v=\{a\}$. $F(d)^v=1 \iff d=a$
$G^v=\emptyset$. 

$G(d)^v=0 \text{ for all } d\in D$

Then $F(b)^v=0$. So $(\forall xA(x))^v=(\forall xF(x))^v = 0$

So $(\forall xA(x) \implies \forall xB(x))^v=1$

...








