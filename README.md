

# Arbitrage and the Law of One Price

Let $V$ be a portfolio and $V_t$ represent the value of this portfolio at time $t$. Arbitrage is feasible if 


$V_0 = 0$

$\\Pr(V_t \ge  0) = 1$

$Pr(V_t >  1) > 0$


The Law of One Price states that:

$Pr(V^a_t = V^b_t) = 1$ for any portfolios $V^a$ and $V^b$ and time $t$

$\Rightarrow$
 
$V^a_0 = V^b_0$



# One Step Binomial Pricing Model

### Variables:
$S:$ Stock

$B$: Asset with continous compounding

$C$: Option

$r$: risk free rate

$u$: up factor

$d$: down factor

$p$: up probability

$T$: time to maturity

$N$: time steps




In the simple one-step model, the price of the stock either increases or decreases with probabilities $p$ and $1-p$ respectively.

<img src="pictures/binomial_onestep.jpg" width="200">



| Description | Initial state          | Stock goes up   |  Stock goes down |
| -------------- | ----------------- | --------------------------- | --------------------------- |
|Stock price| $S_0$    | $S_u = u \cdot S_0$                 | $S_d = d \cdot  S_0$                 |
|Risk free asset | $B_0 =1$    | $B_1 = e^{r}$                  | $B_1 = e^{r}$                |
|Option Value |$C_0=?$   | $C_u = max\{0, S_u - K\}$                  | $C_u = max\{0, S_d - K\}$$                   |




We aim to construct a portfolio $V = \alpha S + \beta B$ that mirrors the option value in both scenarios. From the portfolio's value, we can infer the option's value.

Thus, it must hold that $Pr(V_1 = C_1) = 1$.

For this to be the case, it must follow that:

$C_u = \alpha B_1 + \beta S_u$ and 

$C_d = \alpha B_1 + \beta S_d$

Solving for $\alpha$ and $\beta$, we find:

$\beta = \frac{C_u - C_d}{S_u - S_d}$

$\alpha = \frac{1}{B_1}(C_d - \beta  S_d) = e^{-rT} (C_d - (\frac{C_u - C_d}{S_u - S_d})  S_d) = e^{-rT} (C_d - (\frac{C_u - C_d}{u - d})  d)$



Because of the law of one price it must follow that:

$C_0 = V_0 = \alpha B_0 + \beta S_0 \\$

simplified:


$= e^{-rT}\left[qC_u + (1-q) C_d\right] \quad $ with $\quad q = \frac{e^{rT} - d}{u-d}\quad$
(*1)


## Example: Call option
- $S_0 = 100$
- $K=103$
- $u=1.1$
- $d=0.9$
- $C_u = 7$
- $C_d = 0$
- $r=0.05$


Using the derived formula we find:

$\alpha = e^{-0.05}(- 0.35 * 90) = -29.9637$ 

$\beta = \frac{7-0}{110-90} = 0.35$


<img src="pictures/option_hedge.png">


As shown, our portfolio mirrors the value of the option in both scenarios. Due to the Law of One Price, it must follow that:


$C_0 = V_0 = \alpha + \beta * 100 = 5.0363$

If we charge 5.0363$ for this option, we can construct a portfolio that mirrors the value of the option. Thus, the fair value of the option is 5.0363$.


# Multi Step Binomial Pricing Model

The multi step model is based on the same logic as the single step model.
In the multi step model I make the simplification, that $u = 1/d$ to make computation more efficient.

<img src="pictures/multi_binom.png">


To compute the value of $C_0$, we need to traverse backward through the tree, treating each step as a single-step binomial model. 
The first task is to calculate all possible values of the stock and option at maturity. 
Once we have those, we can work backward through the tree.




We view each possible state as a node in a graph.
Let t be the time step and i be the Node from bottom to the top starting with $0$.

$S_{i, j} = u^{i} \cdot d^{j-i}S$

 $S_{1,2}$ would then be $u^{1} \cdot d^{2-1}S_0 = S_0$

$C_{i,j}$: Value of option at node (i,j)

We can now recursively define the value of the option using the formulas derived earlier.

$C_{i,t}= e^{-rT}\left[qC_{i+1,j+1} + (1-q) C_{i,j+1}\right] \quad$ with $\quad q = \frac{e^{rT} - d}{u-d}$

With $C_0 = C_{0,0}$



----------
## Notes:


(*1)




$C_0 = V_0 = \alpha B_0 + \beta S_0$


$= e^{-rT} (C_d - (\frac{C_u - C_d}{u - d}) d) + \frac{C_u - C_d}{S_u - S_d} \cdot S_0$ 

$= e^{-rT}\left[C_d + (\frac{C_u - C_d}{u - d})(e^{rT} - d)\right]$

$q = \frac{e^{rT} - d}{u-d}$

$= e^{-rT}\left[C_d + (C_u - C_d)q\right]$

$= e^{-rT}\left[qC_u + (1-q) C_d\right]$