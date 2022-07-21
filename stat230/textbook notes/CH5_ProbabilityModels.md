# 5. Discrete Random Variables
## 5.1 Random Variables and Probability Functions 

> **Random Variable (r.v)**: Variable that represents outcomes in a experiment
> **Range**: set of possible values for the variable

Let $X$ be a discrete random variable with $range(X)=A$: 

> **Probability function ($p.f$) of $X$:** 
$f(x)=P(X=x)$, defined for all $x\in A$

**Probability distribution of X**: the set of pairs $\{(x,f(x)):x \in A \}$

*All probability functions must have two properties:* 
1. $1 \leq f(x) \geq 0$ for all $x \in A$
2. $\sum_{all x \in A} f(x) = 1$

> **Cumulative distributive function ($c.d.f$) of $X$:** 
$f(x)=P(X \leq x)$, defined for all $x\in \mathbb{R}$

In general, $F(x)$ can be obtained from $f(x)$ using $F(x)=P(X \leq x)=\sum_{u \leq x}f(u)$

*All cumulative diributive functions must have three properties:* 
1. $F(x)$ is a *non-decreasing* function of $x$ for $x \in \mathbb{R}$
2. $0 \leq F(x) \leq 1$ for all $x \in \mathbb{R}$
3. $\lim_{x \to -\infty}F(x)=0$ and $\lim_{x \to \infty}F(x)=1$.

Note: $f(x)=F(x)-F(x-1)$ $\implies P(X=x) = P( X \leq x) - P(X \leq x-1)$

---
## 5.2 Discrete Uniform Distribution 
Suppose the range of X is $\{a, a+1, ..., b\}$, where all values are ==equally probable==. Then $X$ has a *Discrete Uniform Distribution* on the set $\{a, a+1, ..., b\}$.

> **Probability function:** There as $b-a+1$ values in the set $\{a, a+1, ..., b\}$, so the probability of each value is $\displaystyle\frac{1}{b-a+1}$ so that $\sum_{x=a}^{b}f(x)=1$
$$ f(x)=\begin{cases}
    \displaystyle\frac{1}{b-a+1} &  \text{for } x=a,a+1,...,b \\
    0 & \text{otherwise}
\end{cases} $$

*Example*: Suppose a fair die is thrown once and let $X$ be the number of face on the die. Find the p.f and c.d.f of $X$. 
$p.f$: 
$f(x)=P(X=x)=\begin{cases}
    \frac{1}{6} &  \text{for } x=1,2,...,6 \\
    0 & \text{otherwise}
\end{cases}
$

$c.d.f$: 
$
F(x)=P(X \leq x)=\begin{cases}
    0 &  \text{for } x < 1 \\
    \frac{[x]}{6} &  \text{for } 1< x < 6 \\
    1 &  \text{for } x \geq 6
\end{cases}
$

----
## 5.3 Hypergeometric Distribution 
We have $N$ objects that can be classifed into two types: 
1. $r$ number of "successes" (S)
2. $N-r$ number of "failures" (F)
Pick $n$ objects at random **without replacement**. 

Let X be the number of sucesses obtained. $X~HG(N,r,n)$.

Observe:  
- $\displaystyle{N \choose n}$: total points in sample space S
- $\displaystyle{r \choose x}$: ways to choose x success object from r available
- $\displaystyle{N-r \choose n-x}$: ways to choose remaining $(n-x)$ objects from $(N-r)$ failures
<style>
r { color: #F95050 }
navy { color: #000099 }
b { color: #50C1F9 }
o { color: #F7A63B }
g { color: #48D03E }
</style>

{% comment %} 
     Character or lines for Jekyll to skip.
    > **Probability function**: $f(x)=P(X=x)=\displaystyle\frac{{r \choose x}{N-r \choose n-x}}{N \choose n}$
{% endcomment %}

Note: $x \geq \max\{0, n-N+r\}$, $x \leq \min\{r, n\}$

---
## 5.4 Binomial Distributions 
Experiment has two types of distinct outcomes: 
1. "success" (S)
2. "failure" (F)
Where $P(S)=p$ and $P(F)=1-p$. Repeat the experiment ==**$n$ independent times**==. 

Let X be the number of successes obtained. $X~Binomial(n,p)$

> **Probability function:** $f(x)=P(X=x)=\displaystyle{n \choose x}p^x(1-p)^{n-x}$
> for $x=0,1,...,n$ and $0 < p < 1$

### Binomial vs. Hypergeometric Distribution 
If $N$ is large enough, the number $n$ being drawn is relatively smaller ($N \geq n$), then the Binomial distibution can be used to approximate the hypergeometric distribution. 

---
## 5.5 Negative Binomial Distribution 
Experiment has two types of distinct outcomes: 
1. "success" (S)
2. "failure" (F)
Where $P(S)=p$ and $P(F)=1-p$. Continue doing this experiment until $k$ successes have been obtained.

Let $X$ be the number of failures obtained before the $k$th success (Or, equivalently, the total number of trials needed to get the $k$th success). Then $X~Negative Binomial(k,p)$.

Observe: 
- There are $x+k$ trials and the last trial must be a sucess$\implies$ In the first $x+k-1$ trials there must be $x$ failures and $(k-1)$ successes
- There must be $\displaystyle{{x+k-1}\choose{x}}$ different orders 

> **Probability function:** $f(x)=P(X=x)= \displaystyle{{x+k-1}\choose{x}} p^k(1-p)^{n-k}$ for
> $x=0,1,...,n$ and $0 < p < 1$

### Binomial vs. Negative Binomial Distribution 
- **Binomial**: Know the number of $n$ trials in advance, but do not know number of successes that will be obtained until after the experiment 
- **Negative binomial**: Know the $k$ number of successes but do not know the number of trials that will needed to obtain this number of successes until after the experiment

---
## 5.6 Geometric Distribution (NBD with 1 success)
Basically the Negative Binomial Distribution but stop until first success ($k=1$).

Let X be the number of failures obtained before the first success. $X \sim Geometric(p)$.

> **Probability function:** $f(x)=P(X=x)=\displaystyle{{x+1-1}\choose x}p^1(1-p)^x=(1-p)^xp$

---
## 5.7 Poisson Distribution from Binomial 
The Poisson distribution arises where the random variable X represents the number of events of some type. $X \sim Poisson(\mu) = X \sim Poisson(np)$.

Note: $\mu = np$

1. One way the poisson distribution arises is as a limiting case of the binomial districution as $n \to \infty$ and $p \to 0$, where the product $\mu=np$ is kept a constant

> **Probability function:** $f(x)=P(X=x)=\displaystyle\frac{e^{-\mu}\mu ^x}{x!}$ for $x=0,1,...$

Note: If $p$ is close to 1, then we can also use the *Poisson distribution* to approximate the binomial (interchange the labels success and failure to get the probability of success [formally failure] close to 0)

---
## 5.8 Poisson Distribution from Poisson Proccess 
Can derive the Poisson distribution as a model for the number of a certain kind of event/occurence that occur at **points in time or in space**

***Poisson Proccess Properties*** (properties of types of events that occur in random points in time that can be considered PP): 
1. **Independence**: each number of occurnaces in non-overlapping intervals are independent 
2. **Individuality**: for a sufficiently short period of time $\Delta t$, the probability of two or more events occuring in the interval us close to 0 (events occur singly and not in clusters)
3. **Homogenity/Uniformity**: events occur in a homogeneous/uniform rate of $\lambda$, so the probability of one occurance over an interval $(t, t+\Delta t)$ is approximately $\lambda t$

$\lambda \to$ rate of occurances, $\mu \to$ average number of occurances in $t$ units of time

Let $X$ be the number of event occurances in a time period of length $t$, then $X$ has a Poisson distribution with $\mu=\lambda t$.

> **Probability function:** $f(x)=P(X=x)=\displaystyle\frac{e^{-\mu}\mu ^x}{x!}=\displaystyle\frac{e^{-\lambda t}{\lambda t}^x}{x!}$ for $x=0,1,...$

### Poisson Distribution vs. all other Distributions 
1. *Can we specific in advance the maximum value X can take*: if yes, then distribution is not Poisson 
2. *Does it make sense to ask how often the event did not occur*: if yes, then distribution is not Poisson 

---
## 5.10 Summary Sheet 











