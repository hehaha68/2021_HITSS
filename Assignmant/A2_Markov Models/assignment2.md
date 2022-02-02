## Assignment 02: Markov Models  

##### HE Yongyi from USTC

### 1. Temporal subsampling of a Discrete Time Markov Process

Suppose $X_1,X_2,...$ forms a Markov process (that is **not** necessarily homogeneous for the purpose of this problem). Then recall that as per our definition,  
$$
p\left(x_{n} \mid x_{n-1}, x_{n-2}, \ldots x_{1}\right)=p\left(x_{n} \mid x_{n-1}\right)\tag{1}
$$
for all $n\ge1$. It  can be seen that the definition in (1) is also equivalent to the  condition that 
$$
\begin{align}
p\left(x_{n}, x_{n-1}, x_{n-2}, \ldots x_{1}\right)=p\left(x_{1}\right) p\left(x_{2} \mid x_{1}\right) p\left(x_{3} \mid x_{2}\right) \cdots p\left(x_{n} \mid x_{n-1}\right)\tag{2}
\end{align}
$$
for all $n\ge1$.

(a)	Show that for any positive integer $n$ and any $k < n $ 
$$
\begin{align}
p\left(x_{n}, x_{n-1}, \ldots x_{n-k}\right)=p\left(x_{n-k}\right) p\left(x_{n-k+1} \mid x_{n-k}\right) p\left(x_{n-k+2} \mid x_{n-k+1}\right) \cdots p\left(x_{n} \mid x_{n-1}\right).\tag{3}
\end{align}
$$
This result is equivalent to the  condition that for any positive integer $ n$ and any $ k < n $ 
$$
p\left(x_{n} \mid x_{n-1}, x_{n-2}, \ldots x_{n-k}\right)=p\left(x_{n} \mid x_{n-1}\right)\tag{4}.
$$
It is also immediately obvious that (3) and (4) imply (1) and (2), respectively. Thus the  conditions in (1), (2), (3), and (4) are all equivalent and any of these  an be used as the defining condition for a Markov process.  

**Solution**:
$$
\begin{aligned}
p\left(x_{n}, x_{n-1}, \ldots x_{n-k}\right)&=\sum _{x_{n-k-1}\in A_{n-k-1}}p\left(x_{n}, x_{n-1}, \ldots x_{n-k},x_{n-k-1}\right)\\
&=\sum _{x_{1}\in A_{1}}...\sum _{x_{n-k-1}\in A_{n-k-1}}p\left(x_{n}, x_{n-1}, \ldots x_{n-k},x_{n-k-1},...,x_1\right)\\
&=\sum _{x_{1}\in A_{1}}...\sum _{x_{n-k-1}\in A_{n-k-1}}p\left(x_{1}\right) p\left(x_{2} \mid x_{1}\right) p\left(x_{3} \mid x_{2}\right) \cdots p\left(x_{n} \mid x_{n-1}\right)\\
&=\sum _{x_{1}\in A_{1}}...\sum _{x_{n-k-1}\in A_{n-k-1}}p\left(x_{2} , x_{1}\right) p\left(x_{3} \mid x_{2}\right) \cdots p\left(x_{n} \mid x_{n-1}\right)\\
&=\sum _{x_{2}\in A_{2}}...\sum _{x_{n-k-1}\in A_{n-k-1}}p\left(x_{2}\right) p\left(x_{3} \mid x_{2}\right) \cdots p\left(x_{n} \mid x_{n-1}\right)\\
&=\sum _{x_{n-k-1}\in A_{n-k-1}}p\left(x_{n-k},x_{n-k-1}\right) p\left(x_{n-k+1} \mid x_{n-k}\right)  \cdots p\left(x_{n} \mid x_{n-1}\right)\\
&=p\left(x_{n-k}\right) p\left(x_{n-k+1} \mid x_{n-k}\right)  \cdots p\left(x_{n} \mid x_{n-1}\right)\\
\end{aligned}
$$

----------------

(b)	For $n = 4$, (2) becomes
$$
p\left(x_{1}, x_{2}, x_{3}, x_{4}\right)=p\left(x_{1}\right) p\left(x_{2} \mid x_{1}\right) p\left(x_{3} \mid x_{2}\right) p\left(x_{4} \mid x_{3}\right)\tag{5}
$$
Formally show that (1) also implies that 
$$
p\left(x_{1}, x_{3}, x_{4}\right)=p\left(x_{1}\right) p\left(x_{3} \mid x_{1}\right) p\left(x_{4} \mid x_{3}\right)\tag{6}
$$
**Solution**:
$$
\begin{aligned}
p\left(x_{1}, x_{3}, x_{4}\right)&=\sum _{x_2\in A_2}p\left(x_{1}, x_{2}, x_{3}, x_{4}\right)\\
&=\sum _{x_2\in A_2}p(x_4\mid x_1,x_2,x_3)p(x_1,x_2,x_3)\\
&=\sum _{x_2\in A_2}p(x_4\mid x_3)p(x_1,x_2,x_3)\\
&=p(x_4\mid x_3)p(x_1,x_3)\\
&=p(x_4\mid x_3)p(x_3\mid x_1)p(x_1)
\end{aligned}
$$

----------------

(c)	From the result of the preceding part,  conclude that
$$
p\left(x_{4} \mid x_{3}, x_{1}\right)=p\left(x_{4} \mid x_{3}\right)\tag{7}
$$
**Solution**:
$$
\begin{aligned}
p\left(x_{1}, x_{3}, x_{4}\right)&=p(x_4\mid x_3)p(x_3\mid x_1)p(x_1)
\end{aligned}
$$
Also, we can get:
$$
\begin{aligned}
p\left(x_{1}, x_{3}, x_{4}\right)&=p\left(x_{4} \mid x_{3}, x_{1}\right)p(x_3,x_1)\\
&=p\left(x_{4} \mid x_{3}, x_{1}\right)p(x_3\mid x_1)p(x_1)
\end{aligned}
$$
So, we can conclude the equation
$$
p\left(x_{4} \mid x_{3}, x_{1}\right)p(x_3\mid x_1)p(x_1)=p(x_4\mid x_3)p(x_3\mid x_1)p(x_1)\\
p\left(x_{4} \mid x_{3}, x_{1}\right)=p\left(x_{4} \mid x_{3}\right)
$$

----------------

(d)	Using the results from the preceding parts, formally show that
$$
p\left(x_{1}, x_{2}, x_{4}\right)=p\left(x_{1}\right) p\left(x_{2} \mid x_{1}\right) p\left(x_{4} \mid x_{2}\right)\tag{8}
$$
**Solution**:

From part (a), for any positive integer $n$ and any $k < n $ :
$$
p\left(x_{n} \mid x_{n-1}, x_{n-2}, \ldots x_{1}\right)=p\left(x_{n} \mid x_{n-1}\right)=p\left(x_{n} \mid x_{n-1}, x_{n-2}, \ldots x_{n-k}\right) 
$$
For $n=4$ and $k=2$, the equation becomes
$$
p(x_4 \mid x_3, x_2,x_1)=p(x_4 \mid x_3, x_2)
$$

$$
\begin{aligned}
p\left(x_{1}, x_{2}, x_{4}\right)&=\sum _{x_3\in A_3}p\left(x_{1}, x_{2}, x_{3}, x_{4}\right)\\
&=\sum _{x_3\in A_3}p(x_4\mid x_1,x_2,x_3)p(x_3\mid x_1,x_2)p(x_1,x_2)\\
&=\sum _{x_3\in A_3}p(x_4\mid x_2,x_3)p(x_3\mid x_2)p(x_1,x_2)\\
&=\sum _{x_3\in A_3}p(x_4,x_3\mid x_2)p(x_1,x_2)\\
&=p(x_4\mid x_2)p(x_1,x_2)\\
&=p(x_4\mid x_2)p(x_2\mid x_1)p(x_1)
\end{aligned}
$$

----------------

(e)	From the result of the preceding part,  conclude that
$$
p\left(x_{4} \mid x_{2}, x_{1}\right)=p\left(x_{4} \mid x_{2}\right) \text { . }\tag{9}
$$
**Solution**:


$$
\begin{aligned}
p\left(x_{1}, x_{2}, x_{4}\right)&=p\left(x_{1}\right) p\left(x_{2} \mid x_{1}\right) p\left(x_{4} \mid x_{2}\right)
\end{aligned}
$$
Also, we can get:
$$
\begin{aligned}
p\left(x_{1}, x_{2}, x_{4}\right)&=p\left(x_{4} \mid x_{2}, x_{1}\right)p(x_2,x_1)\\
&=p\left(x_{4} \mid x_{2}, x_{1}\right)p(x_2\mid x_1)p(x_1)
\end{aligned}
$$
So, we can conclude the equation
$$
p\left(x_{4} \mid x_{2}, x_{1}\right)p(x_2\mid x_1)p(x_1)=p(x_4\mid x_2)p(x_2\mid x_1)p(x_1)\\
p\left(x_{4} \mid x_{2}, x_{1}\right)=p\left(x_{4} \mid x_{2}\right)
$$

----------------

(f)	By continuing this line of reasoning, we can argue that if $k$ is some positive integer and $n_1, n_2,...n_k$ is any strictly increasing sequence of positive integers, then
$$
p\left(x_{n_{1}}, x_{n_{2}}, \ldots x_{n_{k}}\right)=p\left(x_{n_{1}}\right) p\left(x_{n_{2}} \mid x_{n_{1}}\right) p\left(x_{n_{3}} \mid x_{n_{2}}\right) \cdots p\left(x_{n_{k}} \mid x_{n_{k}-1}\right)\tag{10}
$$
State the above relation in words.  

**Solution**:

$$
\begin{aligned}
&p\left(x_{n_{k}}, x_{n_{k-1}}, \ldots, x_{n_{1}}\right)\\&=p\left(x_{n_{k}} \mid x_{n_{k-1}}, \ldots, x_{n_{1}}\right) p\left(x_{n_{k-1}}, \ldots, x_{n_{1}}\right) \\
&=p\left(x_{n_{k}} \mid x_{n_{k-1}}, \ldots, x_{n_{1}}\right) p\left(x_{n_{k-1}} \mid x_{n_{k-2}} \ldots, x_{n_{1}}\right) p\left(x_{n_{k-2}} \ldots, x_{n_{1}}\right) \\
&=p\left(x_{n_{k}} \mid x_{n_{k-1}}\right) p\left(x_{n_{k-1}} \mid x_{n_{k-2}}\right) p\left(x_{n_{k-2}} \ldots, x_{n_{1}}\right)\\
&=\cdots \\
&=p\left(x_{n_{k}} \mid x_{n_{k-1}}\right) p\left(x_{n_{k-1}} \mid x_{n_{k-2}}\right) \cdots p\left(x_{n_{2}} \mid x_{n_{1}}\right) p\left(x_{n_{1}}\right)
\end{aligned}
$$
We can state this relationship in such a way that for an ordered sequence of states, each state is only related to its nearest state.

----------------

### 2.  Markov Models for Text: Seuss and Saki  

The files "spamiam.txt" and "saki_story.txt" available on the website have poetry and prose of specific genres. For this problem, use the text in these files to empirically estimate probabilities and transition probabilities as indicated. Ignore any  characters in these files other than the 26 alphabets 'a'-'z' (use white space and  carriage returns as indicated in specific parts). Also ignore any case distinctions among alphabets (example 'C' and 'c' are equivalent). 

**For each of the sub-parts indicated below, print the 100 words that you generate in the form of a $10\times10$ array and  circle any valid English words that you recognize.**

(a)	Assuming that the 26 letters of the alphabet are equiprobable. Generate one hundred random 4 letter words by selecting the 4 individual letters of each word independently.

**Solution**:

```python
import random

def word_print(str):
    for i in range(100):
        print(str[i],end=' ')
        if (i+1)%10 == 0:
            print('\n')
            
alphabet = "abcdefghijklmnopqrstuvwxyz"
for i in range(100):
    word = ''.join(random.choices(alphabet,k=4))
    solution_a[i] = word
word_print(solution_a)
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210723210413576.png" alt="image-20210723210413576" style="zoom:67%;" />

**Score: 2/100**

----------------

(b)	Estimate the probabilities of individual letters using "spamiam.txt". Generate one hundred random 4 letter words by selecting the 4 individual letters of each word independently according to the estimated probability distribution.

**Solution**:

```python
file = open('spamiam.txt', 'r')
text = file.read().lower()
text = re.split(r'[\',-.?!;\n\t 1234567890]+',text)

temp = ''.join(text)

solution_b=[0]*100
for i in range(100):
    word = ''.join(random.choices(temp,k=4))
    solution_b[i] = word
    
word_print(solution_b)
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210724131627546.png" alt="image-20210724131627546" style="zoom: 67%;" />

**Score: 4/100**

----------------

(c)	Again using the file "spamiam.txt", estimate the transition probabilities, $ P(x_{n+1}|x_n)$, for all 26 possible values of $x_n$ - the $n^{th}$ letter in a word and $x_{n+1}$ - the $(n+1)^{th}$ letter in a word (assume that these probabilities are independent of $n$). Also for this part and the next, for your estimation of transition probabilities, use only the letters inside a word for the computation and do not incorporate letters from adjacent words (with a blank in between). Generate one hundred random 4 letter words by first generating a letter at random according to the probability mass function (pmf) in 2b and then generating remaining letters according to appropriate transition probabilities. *Note: You may default to the model of 2b if you end up with a situation where your estimate of  $ P(x_{n+1}|x_n)$ is zero for all values of $x_{n+1}$.*

**Solution**:

```python
file = open('spamiam.txt', 'r')
text = file.read().lower()
text = re.split(r'[\',-.?!;\n\t 1234567890]+',text)

# train
cfd=nltk.ConditionalFreqDist()
for k in range(len(walden)):
    if len(walden[k])-1 < 0:
        continue
    for i in range(len(walden[k])-1):
        cfd[walden[k][i]][walden[k][i+1]] += 1
        
# test
temp = ''.join(text)
solution_c=[0]*100
for n in range(100):
    letter=random.choice(temp)
    word=letter
    for i in range(3):
        arr = []
        if len(cfd[letter]) == 0:
            letter = random.choice(temp)
        else:
            for j in cfd[letter]:
                for k in range(cfd[letter][j]):
                    arr.append(j)
            letter = random.choice(arr)
        word += letter
    solution_c[n]=word

word_print(solution_c)
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210724163221732.png" alt="image-20210724163221732" style="zoom:67%;" />

**Score: 15/100**

----------------

(d)	Once again use the file "spamiam.txt", to estimate the transition probabilities  $ P(x_{n+1}|x_n,x_{n-1})$, for all possible values of the successive letters. Generate one hundred random four letter words using these estimated probabilities. Make reasonable assumptions that generalize what was indicated in 2c.

**Solution**:

```python
file = open('spamiam.txt', 'r')
text = file.read().lower()
text = re.split(r'["\',-.?!;\n\t 1234567890]+',text)

# train
cfd2=nltk.ConditionalFreqDist()
for k in range(len(text)):
    if len(text[k])-1 < 1:
        continue
    for i in range(len(text[k])-2):
        cfd2[text[k][i]+text[k][i+1]][text[k][i+2]] += 1
        
# test
temp = ''.join(text)

solution_d=[0]*100

for n in range(100):
    letter=random.choice(temp)
#     letter=''.join(random.choices(temp,k=2))
    for j in cfd[letter]:
        for k in range(cfd[letter][j]):
            arr.append(j)
    letter += random.choice(arr)
    word=letter
    for i in range(2):
        arr = []
        if len(cfd2[letter]) == 0:
            letter = letter[-1] + random.choice(temp)
        else:
            for j in cfd2[letter]:
                for k in range(cfd2[letter][j]):
                    arr.append(j)
            letter = letter[-1] + random.choice(arr)
        word += letter[-1]
    solution_d[n]=word

word_print(solution_d)
```

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210724224519428.png" alt="image-20210724224519428" style="zoom:67%;" />

**Score: 28/100**

----------------

(e)	Repeat parts 2b-2d using the file "saki_story.txt".

**Solution**:

Repeat part 2b:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210724164017440.png" alt="image-20210724164017440" style="zoom:67%;" />

**Score: 6/100**

Repeat part 2c:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210724165510039.png" alt="image-20210724165510039" style="zoom:67%;" />

**Score: 20/100**

Repeat part 2d:

<img src="C:\Users\Yorick He\AppData\Roaming\Typora\typora-user-images\image-20210724233529244.png" alt="image-20210724233529244" style="zoom:67%;" />

**Score: 30/100**

----------------

(f)	Comment on your results.

In 2c we first use the probability mass function in 2b to generate the first letter, and then use the first-order Markov model. We can see, the results of training with the first-order Markov model in 2c are significantly better than the results of training with just the probability mass function in 2b.

In 2d, we first use the probability mass function in 2b to generate the first letter, then use the first-order Markov model in 2c to generate the second letter, and finally use the second-order Markov model. We can see, the results of training with the second-order Markov model in 2d are significantly better than the results of training with first-order Markov model in 2c. And the generated words are highly correlated with the training text, in other words, the generated words have the style of the training text.

Next, we do a side-by-side comparison of the two texts. For the same model, the number of valid words generated by training "saki_story.txt" is always more than the number of valid words generated by training "spamiam.txt". "spamiam.txt" contains 771 words (2434 letters), and "saki_story.txt" contains 1727 words (7292 letters). So we can infer that, within a certain range, the larger the training set is, the better the results obtained.

----------------

(g)	**Extra Credit**: Estimate the entropy rate for each of the Markov models you developed and  compare these both across models for a single data file and across the two data files. You may need to make suitable assumptions in order to determine your answers (which may be hard/impossible to validate) 

**Solution**: 

The entropy rate of the stochastic process ${X_i}$ is defined when the following limit exists:
$$
H(\mathcal{X})=\lim _{n \rightarrow \infty} \frac{1}{n} H\left(X_{1}, X_{2}, \cdots, X_{n}\right)\quad\text{or}\quad H^{\prime}(\mathcal{X})=\lim _{n \rightarrow \infty} H\left(X_{n} \mid X_{n-1}, X_{n-2}, \cdots, X_{1}\right).
$$
The above two equations reflect two different aspects of the concept of entropy rate. The first refers to the entropy of each character of the $n$ random variables. The second refers to the conditional entropy of the last random variable in the case where the previous $n-1$ random variables are known. For a stationary process, both of these limits exist and are equal, i.e,  $H(\mathcal{X})=H^{\prime}(\mathcal{X})$.

For a stationary Markov chain, the entropy rate is
$$
\begin{aligned}
H(\mathcal{X}) &=H^{\prime}(\mathcal{X})=\lim H\left(X_{n} \mid X_{n-1}, \cdots, X_{1}\right)=\lim H\left(X_{n} \mid X_{n-1}\right) \\
&=H\left(X_{2} \mid X_{1}\right)
\end{aligned}
$$
where the conditional entropy can be calculated from the stationary distribution.

The entropy rate convergence theorem for Markov chains is described formally below. Let ${X_i}$ be a stationary Markov chain with a stationary distribution $\mu$ and a transition matrix $P$. Then the entropy rate is
$$
H(\mathcal{X})=\sum_{i} \mu_{i}\left(\sum_{j}-P_{i j} \log P_{i j}\right)=-\sum_{i j} \mu_{i} P_{i j} \log P_{i j}.
$$
So for this part, we assume that this Markov process is a stationary process  and the distribution of each letter of the training text (data file) is stationary distribution.

Model 2c (Model Ⅰ):

- The file "spamiam.txt"(File Ⅰ):

  Count and calculate the distribution of each letter through this file, and generate the transition matrix by training. Then  estimate the entropy rate $H(\mathcal{X})\approx 1.9503$.

- The file "saki_story.txt"(File Ⅱ):

  The method and process are the same as above. Then  estimate the entropy rate $H(\mathcal{X})\approx 1.0314$.

For Model Ⅰ, the entropy rate in File Ⅱ is lower than in File Ⅰ, may because File Ⅱ's training set is larger. By training in File Ⅱ, uncertainty may be effectively reduced.

Model 2d (Model Ⅱ):

- The file "spamiam.txt"(File Ⅰ):

  Count and calculate the distribution of each two letters through this file, and use only the letters inside a word for the computation and do not incorporate letters from adjacent words. For example, in the string 'want to', we only count 'wa', 'an', 'nt' and 'to'. Generate the transition matrix, and we mark the transition step for $P(x_{n+1}|x_n,x_{n-1})$ as  $(x_{n-1},x_{n})\to (x_{n},x_{n+1})$. Then  estimate the entropy rate $H(\mathcal{X})\approx 0.8900$.

- The file "saki_story.txt"(File Ⅱ):

  The method and process are the same as above. Then  estimate the entropy rate $H(\mathcal{X})\approx 6.1801$.

For Model Ⅱ, the entropy rate in File Ⅰ is lower than in File Ⅱ. In this model, we use two states to predict one state. So the larger training set is, the more kinds of state model may have. For this model, large training set may lead to overfitting problem.

For File Ⅰ, the entropy rate in Model Ⅱ is lower than in Model Ⅰ. File Ⅰ's training set is not large, so not many states may be generated with Model Ⅱ and the uncertainty may be effectively reduced.

For File Ⅱ, the entropy rate in Model Ⅰ is lower than in Model Ⅱ. By training the Model Ⅱ, too many states may be generated, uncertainty may be significantly increased.

