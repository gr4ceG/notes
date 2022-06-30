
<style>
r { color: #F95050 }
navy { color: #000099 }
b { color: #50C1F9 }
o { color: #F7A63B }
g { color: #48D03E }
</style>

# 7 Expected Value and Variance (+5.9 study)
## 7.1 Summarizing Data on Random Variables
Mean of a sample: 
$$\bar{x}=\sum_{i=1}^{n}\displaystyle\frac{x_i}{n}$$

Median of a sample: a value such that half the results are above it and half are above it, when the results are *arranged in numerical order*

Mode of a sample: value which occurs the most

## 7.2 Expectation of a Random Variable

Let X be a discrete random varuable with $range(X)=A$ and the probability function $f(x)$. Then <r>**expected value** (*mean or expectation*) of $X$</r> is given by: 
$$\mu=E(X)=\sum_{x\in A} xf(x)$$

The <r>**expected value of some function $g(X)$** of $X$</r> is given by:
$$E[g(X)]=\sum_{x\in A}g(x)f(x)$$

Notes:
- Can interpret $E[g(X)]$ as the average value of $g(X)$ in an infinite series of repitions of the process where $X$ is defined
- Expected value ($E(x), \mu$) is NOT a random variable, but a non-random constant

### Linearity Properties of Expectation
Following is true for any $g(x), g_1(x)$ and $g_2(x)$:
1. For constants $a$ and $b$, 
$$E[ag(X)+b]=aE[g(X)]+b$$

2. For For constants $a$ and $b$ and two functions $g_1$, $g_2$:
$$E[ag_1(X)+bg_2(X)]=aE[g_1(X)]+bE[g_2(X)]$$

Note: When $g(X)$ is a *non-linear* function, it is NOT given that $E[g(X)]=g[E(x)]$ (<- only for linear functions)

## 7.3 Some Applications of Expectation
Note:
- Fair game: value of 0
- Random variables assocated with a given problem may be defined in different ways but the expected winnings will remian the same.

## 7.4 Means and Variance of Distributions
### $E(X)$ of a Binomial Random Variable
Let $X$~$Binomial(n,p)$: $E(x)=np$.
- $Var(X)= np(1 - p)$

### $E(X)$ of the Poisson random variable
Let $X$~$Poisson()$: 
- $E(x)=\lambda t$.
- - $Var(X)= \mu = E(X)$
Note: For the Poisson distribution, <r>*the variance equals the mean*</r>

### $E(X)$ of the Hypergeometric
Let $X$~$Hypergeometric(N,n,r)$: 
- $E(x)=\displaystyle\frac{nr}{N}$

### $E(X)$ of the Negative Binomial
Let $X$~$NegBinomial(k,p)$: $E(x)=\displaystyle\frac{k(1-p)}{p}$

### $E(X)$ of the Geometric
Let $X$~$Geometric(p)$: $E(x)=\displaystyle\frac{(1-p)}{p}$

The **variance** of a random variable $X$, denoted by $Var(X) = \sigma^2$, is
$$\sigma^2=Var(X)=E[(X-\mu)^2]$$ 

Note: 
1. $Var(X)=E(X^2)-[E(X)]^2=E(X^2)-\mu^2$
2. $Var(X)=E[X(X-1)]+E(X)-[E(X)]^2=E[X(X-1)]+\mu-\mu^2$

Then, the *standard deviation of a random variable X* is
$$\sigma=sd(X)=\sqrt(Var(X))=\sqrt(E[(X-\mu)^2])$$

### Properties of Mean and Variance
If $a$ and $b$ are constants and $Y=aX+b$, then
$$\mu_y=E(Y)=aE(X)+b=a\mu_x+b$$

and 
$$\sigma_y^2=Var(Y)=a^2Var(X)=a^2\sigma_x^2$$

**<r>Note: Variance is not affected by (+), only by (*)</r>**
