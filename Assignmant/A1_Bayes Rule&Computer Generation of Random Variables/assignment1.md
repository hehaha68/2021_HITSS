## Assignment 01: Bayes Rule and Computer Generation of Random Variables  

##### HE Yongyi from USTC

### 1. A posteriori probability:  

There are $K$ = 11 urns labeled by $u\in\{0,1,2,...,10\}$, each  containing L = 10 balls. Urn $u$ contains $u$ black balls and $10-u$ white balls. Fred selects an urn $u$ at random and draws $N$ times with replacement from that urn, obtaining $N_B$ blacks and $N-N_B$ whites. Fred's friend, Bill, looks on. If after $N$ = 10 draws $n_B$ = 3 blacks have been drawn, what is the probability that the urn Fred is using is urn $u$, from Bill's point of view? (Bill doesn't know the value of $u$.) Calculate the numerical values of the probability for $u={0,1,2,...,10}$ and tabulate the probability values. Indicate the value of $u$ for which the probability is maximum.  

**solution**

The probability that Fred draws a black ball from urn $u$ is $\frac{u}{L}=\frac{u}{10}$. So when after 10 times draws and 3 black balls have been drawn, we have
$$
P\left(N=10, N_{B}=3 \mid \text { label }=u\right)=\binom{10}{3}\times(\frac{u}{10})^3\times(\frac{10-u}{10})^7=\frac{1.2u^3(10-u)^7}{10^8}.
$$
Then we use Bayes rules:
$$
\begin{aligned}
P(\text{label}=u|N=10,N_B=3)&=\frac{P\left(N=10, N_{B}=3 \mid \text { label }=u\right) P(\text { label }=u)}{\sum P\left(N=10, N_{B}=3 \mid \text { label }=u\right) P(\text { label }=u)} \\
&=\frac{P\left(N=10, N_{B}=3 \mid \text { label }=u\right)}{\sum P\left(N=10, N_{B}=3 \mid \text { label }=u\right)}\\
&=\frac{u^{3}(10-u)^{7}}{\sum u^{3}(10-u)^{7}}
\end{aligned}
$$
After calculating, the probability table is：

| $u$  |  0   |   1    |   2    |   3    |   4    |   5    |   6    |   7    |          8           |          9           |  10  |
| :--: | :--: | :----: | :----: | :----: | :----: | :----: | :----: | :----: | :------------------: | :------------------: | :--: |
| $P$  |  0   | 0.0631 | 0.2212 | 0.2932 | 0.2363 | 0.1288 | 0.0467 | 0.0099 | 8.642$\times10^{-4}$ | 9.613$\times10^{-6}$ |  0   |

When $u$ = 3, the probability is maximum.  

-------------



### 2.  Computer Generation of Random Variables  

Suppose that $p(u)$ denotes a valid probability density function(PDF) for a (continuous) real-valued random variable and let $F(u) = \int _{-\infty}^u p(v)dv$ denote the  corresponding  cumulative distribution function. Let $X$ be a random variable that is uniformly distributed over the interval between 0 and 1, i.e. , its probability density function(PDF) is given by  
$$
p_{X}(x)=\left\{\begin{array}{ll}
1 & 0 \leq x<1 \\
0 & \text { otherwise }
\end{array}\right.
$$
Now let $G(\cdot)$ denote a function for which the function $F(\cdot)$ is an inverse, i.e. for each $t\in[0,1]$,  
$$
F(G(t))=t
$$
and define the random variable $Y$ as a function of  $X$ by the relation:  
$$
Y=G(X)
$$
(a) Show that the PDF $p_Y (y)$ for the random variable $Y$ is given by $p_Y (y)$ = $p(y)$, i.e., the PDF of the random variable matches the probability density function $p(\cdot)$. In order to simplify your proof, you may assume that $G(t)$ is differentiable with a non-zero derivative for all $t\in[0,1]$ (though this condition is not required).
Hint: You  can solve this problem either by  computing the  cumulative distribution function for $Y$ and  computing the PDF from that or by using the formula for the PDF of a function of a random variable along with the  chain rule for derivatives.

**solution**

The given the function $G(\cdot)$ being an inverse for $F(\cdot)$ and then we assume that $G(t)$ is differentiable with a non-zero derivative for all $t\in[0,1]$.

Because the function $F(\cdot)$ is monotonically increasing, so $G(t)$ is monotonically increasing, i.e. for each $t\in[0,1]$,
$$
G^{'}(t)>0.
$$
Using the formula for the PDF of a function of a random variable, we have:
$$
p_Y(y)=\frac{p_X(x)}{|G^{'}(x)|}=\frac{p_X(x)}{G^{'}(x)}.
$$
Using  the  chain rule for derivatives, for each  $x\in[0,1]$ we have:
$$
\int_{-\infty}^{y}p_Y(u)du=\int_{-\infty}^{x}\frac{p_X(t)}{G^{'}(t)}G^{'}(t)dt=\int_{-\infty}^{x}p_X(t)dt=\int_{0}^{x}dt=x
$$
So,  we know :
$$
F(y)=F(G(x))=x=\int_{-\infty}^{y}p_Y(u)du
$$
The PDF of the random variable Y matches the probability density function $p(\cdot)$.

-----------



(b) **Matlab**: For computer simulation, the above observation suggests a method for generating a random variable with a desired PDF $p(u)$, using the mapping Y = G(X) on a uniformly distributed random variable X. Consider the PDF  
$$
p(u)=\left\{\begin{array}{ll}
2u & 0 \leq u\leq1 \\
0 & \text { otherwise }
\end{array}\right.
$$
Generate $N = 10^4$ realizations of the random variable with the above PDF using the method suggested above. Plot and  compare the suitably normalized histogram of these realizations you generate against the above PDF in a single plot. Explain your normalization procedure. Repeat this exercise for $N = 10^6$.

**solution**

When $N=10^4$:

<img src="C:\Users\Yorick He\Documents\Tencent Files\908536269\FileRecv\1(1).jpg" alt="1(1)" style="zoom:50%;" />

```matlab
N=10.^4;
X=rand(1,N);
G = sqrt(X);
[histFreq,histXout] = hist(G,50);
binWidth = histXout(2) - histXout(1);
figure;
bar(histXout, histFreq/binWidth/sum(histFreq));
title('N=10^4');
xlabel('x');
ylabel('PDF:p(x)');
hold on
p = 2*histXout;
plot(histXout,p);
```

The normalization procedure:

1. Calculate the width of the interval of each bin: `binWidth = histXout(2)-histXout(1)`;
2. Calculate the area of all bins: `binWidth/sum(histFreq)`;
3. Calculate the height after normalizing the area (width remains the same): `histFreq/binWidth/sum(histFreq)`.

When $N=10^6$:

<img src="C:\Users\Yorick He\Documents\Tencent Files\908536269\FileRecv\2(1).jpg" alt="2(1)" style="zoom:50%;" />

