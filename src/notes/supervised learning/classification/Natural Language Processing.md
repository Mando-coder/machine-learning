# Natural Language Processing



## Bayes Theorem
In set theory the probability of an event $A$ occuring given that event $B$ occured can be described as the ratio of the probability of $A$ and $B$ occuring (intersect) and the probability of $B$ occuring

$$ P(A|B) = \frac{P(A \cap B)}{P(B)} $$

$$ P(B|A) = \frac{P(B \cap A)}{P(A)} $$


<p align="center">
  <img src="../../images/bayes_theorem.png" alt="bayes_theorem" width="450px"/>
</p>

Since the intersect of $A$ and $B$ is the same as the intersect of $B$ and $A$, Bayes Theorem may be rewritten as

$$ P(A \cap B) = P(B \cap A) $$

$$ P(A|B)P(B) = P(B|A)P(A) $$

$$ P(A|B) = \frac{P(B|A)P(A)}{P(B)} $$

### Example
There is a building with  faulty smoke alarm that only goes off 10% of the time. The question then becomes, *if you hear the smoke alarm go off, what is the probability there is a dangerous fire*?

In a fire situation the following probabilities are known
* Dangerous fire = 1%
* Smoke alarm goes off = 10%
* Smoke alarm goes off during dangerous fire= 95%

<br>

$S$ = smoke alarm goes off  
$F$ = dangerous fire  

$$P(F|S) = \frac{P(S|F)P(F)}{P(S)} = \frac{0.95 \cdot 0.01}{0.1} = 9.5\% $$




## Naive Bayes Classifier
When performing any sort of classification, you are asking the question: Given a set of data points $X$ what is the probability the label should be classified as class $c_m$

<!-- $$ P(c_m | X) = P(c_m| x_1, x_2, ... , x_n)$$ -->

$$ P(c_m | X) = \frac{P(X|c_m)P(c_m)}{P(X)} = \frac{P(x_1, x_2, ..., x_n|c_m)P(c_m)}{P(X)} $$

The dataset $X$ is a series of datapoints $x_i$, if we make the **naive assumption** that all the data point in $X$ are *independent* of one another, then we can rewrite the above probability as

$$ P(X | c_m) = P(x_1 |c_m ) \cdot P(x_2 | c_m) \cdot ... \cdot P(x_n | c_m) $$

$$ P(c_m | X) =  \frac{(P(x_1 |c_m ) \cdot P(x_2 | c_m) \cdot ... \cdot P(x_n | c_m))P(c_m)}{P(x_1, x_2, ... x_n)} $$

The final classification will be the value of $c_m$ which produces the largest value for said probability. In other words you could loop over every possible class $c_m$ and check what the probability is. For each value of $c_m$ the denumerator remains the same, so in order to find the correct class classification $c_m$, the following expression needs to be maximized. 

$$ P(c_m) \prod_{i=1}^n P(x_i | c_m) $$


## Multinomial Naive Bayes

There are many types of Naive Bayes classifiers, however in the context of Natural Language Processing, the Multinomial Naive Bayes classifier is the most common. To illustrate how this works consider a machine learning model which aims to classify movie reviews as *positive* or *negative* based on the text of the review. 

For this example we consider 35 reviews, 25 of which are positive, and 10 are negative. The word counts of important words have been gathered. 

||Positive Review|Negative Review|
|--|--|--|
|"movie"|10|8|
|"actor"|2|10|
|"great"|8|0|
|"film"|4|2|

### Data Preparation

For the classification the following expression needs to be maximized

$$ P(c_m) \prod_{i=1}^n P(x_i | c_m) $$

The probability of a movie belonging to either class can quite simply be calculated. 

$P(\textrm{pos}) = 25/35 = 0.71$

$P(\textrm{neg}) = 10/35 = 0.29$

Next the probabilities of getting a word $x_i$ given a classification $c_m$ are calculated

|$x_i$| $P(x_i\| \textrm{pos})$ | $P(x_i\| \textrm{neg})$ |
|--|--|--|
|"movie"|10/24 = 0.42|8/20 = 0.4|
|"actor"|2/24 = 0.08|10/20 = 0.5|
|"great"|8/24 = 0.33|0/20 = 0|
|"film"|4/24 = 0.17|2/20 = 0.1|

### Classification
This new data can then be used in order to determine whether a new review is positive or negative. Consider the review of "movie actor", the probability that it belongs to a positive review is proportional to:

$P(\textrm{pos}) \cdot P(\textrm{"movie"}|\textrm{pos}) \cdot P(\textrm{"actor"}|\textrm{pos}) =0.71(0.42)(0.08) = 0.024$

Similarly, the probability that the review belongs to a negative review is proportional to 

$P(\textrm{neg}) \cdot P(\textrm{"movie"}|\textrm{neg}) \cdot P(\textrm{"actor"}|\textrm{neg}) =0.29(0.4)(0.5) = 0.058$

The movie review is therefore a lot more likely to be negative than positive, mainly because (according to the training data set) negative reviews talk about actors a lot more than positive reviews. 

### Smoothing Factor
One may run into problems if one of the word counts $x_k$ is zero, then the entire probability will be zero.  

$$P(x_k |c_m) = 0$$

$$ P(c_m) \prod_{i=1}^n P(x_i | c_m) = 0$$

For example the review "*I had great seats but the movie was horrendous*" will have a zero chance of being a negative review because the word count for "great" in the negative reviews section of the training data is zero. 

$P(\textrm{neg}) \cdot P(\textrm{"movie"}|\textrm{neg}) \cdot P(\textrm{"great"}|\textrm{neg}) = 0$

To remedy this a smoothing factor $ \alpha$ may be applied, which simply increases the word count 


||Positive Review|Negative Review|
|--|--|--|
|"movie"| $10 + \alpha $|$8 +  \alpha$|
|"actor"|$ +  \alpha$|$10 +  \alpha$|
|"great"|$8 +  \alpha$|$0 +  \alpha$|
|"film"|$4 +  \alpha$|$2 +  \alpha$|

## Feature Extraction
Most machine learning algorithms don't have raw text data is input, instead it needs to be converted into numerical data. 

### Count Vectorization / Term Frequency
Count vectorization is the process of simply counting how often a word occurs in each document. Here each document is represented as a single string. The resulting count is in the form of a matrix, where the rows represent the documents and the columns the words/terms. 

<p align="center">
  <img src="../../images/nlp_text.png" alt="nlp_text" width="450px"/>
</p>



<p align="center">
  <img src="../../images/document_term_matrix.png" alt="document_term_matrix" width="450px"/>
</p>


Each cell in the matrix can be computed by the term frequency function $\text{tf}$ based on the term $t$ (word) and document $d$ (string).

$$ \text{count} =  \text{tf}(t, d) $$







A lot of columns will have a lot of zeros, for this reason these matrices are stored as a **sparse matrix**. This is a way of re-writing a matrix with a lot of zeros into a smaller format. A sparse matrix is a collection of 3 arrays which can be used to reconstruct the original $n \times n$ matrix.

* The value array represents all of the non-zero values in the matrix read from each row left to right. 
* The index array represents the matrix column index of each value in the value array
* The row pointer array represents the value array index of the values that appear first in the row


<p align="center">
  <img src="../../images/sparse_matrix.png" alt="sparse_matrix" width="250px"/>
</p>




### Stop-Words
Very common words in the English language (e.g. "a", "the", "in", etc.) are commonly used in every piece of text, but this doesn't mean they should receive a high weight. Most machine-learning applications have a library of stop-words which are automatically filtered out of the count vectorization matrix. 

### TF-IDF Vectorization
One of the main problems with using term frequency on its own is that words that occur often only in certain documents will receive low weights, even though they are key-words we want to include in our analysis. For this reason an additional **Inverse Document Frequency** (IDF) factor is introduced.

This factor diminishes the weights of words that occur often in every document, and amplifies the weights of words that occur often only in specific documents. For example in a set of sports articles the importance of the word "run" should be diminished as every sport involves running, but the word "basketball" should be amplified as it is only relevant for sport articles about basketball. 

First we consider a set $D$, which contains all of the documents $d_i$. 

$$D = \{d_1, d_2, ..., d_n\}$$

From this set we create a subset which is filled with documents $d$ which contain the word/term $t$

$$\{d \in D : t\in d \}$$

The length/size of this set is the number of documents that contain the word/term $t$. The $\text{idf}$ factor considers the inverse ratio of the number of documents that contain $t$ to the total number of documents $N$ and scales it using a logarithmic function. 

$$\text{idf}(t, D) = \log(\frac{N}{|\{d \in D: t\in d\}|})$$

The total TF-IDF factor is then given by

$$\text{tfidf}(t, d, D) = \text{tf}(t,d) \cdot \text{idf}(t, D)$$

It should be noted that there exist many variations for the definitions of  $\text{tf}$ and $\text{idf}$ 


















