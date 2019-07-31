## Neural Machine Translation by Jointly Learning to Align and Translate

------



[Paper URL](https://arxiv.org/pdf/1409.0473.pdf)



### 1. Motivation: A fixed-length vector is a hurdle

-----

>  "the use of a fixed-length vector is a bottleneck in improving the perfomance of [the] basic encoder-decoder architecture" (1)

>  "Cho et al.(2014b) showed that indeed the performance of a biasic encoder-decoder deteriorates rapidly as the length of an input sentence increases"(1)

#### 1. 1. A basic encoder-decoder architecture:



![Figure 1](/Users/kakao/dayoneblog/content/post/resources/_gen/images/encoder_decoder-architecture.png)

+ Encoder

  ![Basic Encoder](/Users/kakao/dayoneblog/content/post/resources/_gen/images/basicEncoder_pic.png)

  

  + Input: a sequece of vectors: **x**=(x[1], x[1], x[3] . . . x[T])
  + hidden state: h[t]=f (x[t] , h[t-1])
  + A fixed-length vector: c=q({h[1], h[2], . . . h[T]})  (which is **z** in the *Figure 1*)

+ Decoder

  ![Basic Decoder](/Users/kakao/dayoneblog/content/post/resources/_gen/images/basicDecoder_pic.png)

  + hidden state: s[t] = g(s[t-1], y[t-1], c)

  + output: **y** 

    ![Conditional Probability](/Users/kakao/dayoneblog/content/post/resources/_gen/images/basicDecoder.png)

  



#### 1.2. The structural problem with the basic architecture

+ The basic architecture should compress all information in a input sentence into a fixed-length vector, regardless of the sentence length.
+ The inflexibility of a fixed-length vector has a harmful effect on the performance especially when the input sentence gets longer.

![Figure2](/Users/kakao/dayoneblog/content/post/resources/_gen/images/longersentecesucks.png)

​	\* if you are not familiar with the basic encoder-decoder architecture check out [Cho *et al* (2014a)](https://arxiv.org/pdf/1406.1078.pdf) and  [Cho *et al* (2014b)](https://www.aclweb.org/anthology/W14-4012)





### 2. Contribution: the Advent of Alignment model (aka Attention)

------



> "[We] propose to extend [a fixed-length vector] by allowing a model to automatically (soft-)search for parts of a source sentence that are relevant to predicting a target word, without having to form these parts as a hard segment explicitly"(1)

#### 2.1. Model Architecture



![jointlymodel](/Users/kakao/dayoneblog/content/post/resources/_gen/images/jointlymodel.png)

##### Encoder: Bidirectional RNN

+ Why BiRNN? 

  Make h[j] to summerize both the preceding words and the following words, focusing on the neighborhood of x[j].

+ Hidden State **h**: Concatenating *forward hidden state* and *backward hidden state*

  ![BiRNN hidden state](/Users/kakao/dayoneblog/content/post/resources/_gen/images/BiRNNhiddenstate.png)



##### Decoder: Model Jointly Learns How to  Align and Translate

+ Overview

![overview](/Users/kakao/dayoneblog/content/post/resources/_gen/images/jointlyoverview.png)

+ Model in detail: Activation functions

  1. **g** : deep output with a single maxout hidden layer

     ![activationfunctionG](/Users/kakao/dayoneblog/content/post/resources/_gen/images/activationFunctionG.png)

     

     

  2. **f** : GRU

     

     ![activationfunctionG](/Users/kakao/dayoneblog/content/post/resources/_gen/images/activationfunctionF.png)

     

     + z[i] is a update gate that decides the ratio between the previous hidden state and the new hidden state.
     + for the more detailed explanation of GRU, check out  [Cho *et al* (2014a)](https://arxiv.org/pdf/1406.1078.pdf)

     

  3. **a** : single-layer perceptron

     

     ![activationfunctionA](/Users/kakao/dayoneblog/content/post/resources/_gen/images/activationfunctionA.png)

     

#### 2.2 Training Procedure

+ Dataset

  + The bilingual, parallel corpora provided by ACL WMT '14 (English-French parallel corpora)
  + no monolingual corpus

+ Preprocessing

  + usually toknenization(Moses)
  + utilized 30,000 most frequently used words

+ Parameter Initialization

+ Training

  + optimizer: SGD
  + Adapting the learning rate: [Adadelta](https://arxiv.org/pdf/1212.5701.pdf) (ε = 10−6 and ρ = 0.95)
  + batch_size:  80 sentences
  + normalized the L2-norm of the gradient of the cost function each time to be at most a predefined threshold of 1, when the norm was larger than the threshold. [Pascanu *et al* (2013b)](http://proceedings.mlr.press/v28/pascanu13.pdf)

  

#### 2.3 Result

![result](/Users/kakao/dayoneblog/content/post/resources/_gen/images/jointlyresult.png)



