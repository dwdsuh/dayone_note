# Deep Contextualized Word Representations





## 1. Motivation

### Difficulty of language modeling

+ complex characteristics of word use(syntax and semantics)
+ how these uses vary across linguistic contexts



## 2. Model Description

### ElMo(Embedding from Language Models)

+ the high-level LSTM state capture context-dependent aspects of word meaning 
  eg) they can be used to perform well on supervised word sense disambiguation tasks
+ the lower-level state capture aspects of syntax. (part-of-speech tagging)

+ ELMo word representations are functions of the entire input sentence



### ELMo in detail

#### 1. Bidirectional Language Models

![biLM](/Users/kakao/kakao_project/paper_review/images/biLM.png)

+ ;  in the probability notation means "parameterized"



#### 2. ELMO

+ representation

  + each token t[k] has a set of 2L+1 representations computed by L-layer biLM.

  

  ![representation](/Users/kakao/kakao_project/paper_review/images/representation.png)

  

+ ELMo
  + Collapses all alyers in R into a single vector

![elmo_formula](/Users/kakao/kakao_project/paper_review/images/ELMo_formula.png)

​				s_task[j]: softmax-normalized weights 
​				gamma_task: scale the entire ELMo vector. --> importance in regards to the optimization process	



+ ELMo에 대한 감을 잡기 위해 다음과 같이 생각해보자
  1. 머리 속에 RNN 구조 기반 인코더를 생각해보자. Bidirectional RNN이면 더 좋다.
  2. RNN의 Layer를 L개로 확대해보자
  3. 각 Layer마다 Hidden State이 생기기 때문에 한 토큰에 대해서 L개의 히든 유닛이 생긴다.
  4. 각 토큰마다 원래 토큰의 값을 포함하여 L개의 layer에 있는 hidden unit의 가중 평균을 내면 ELMo를 통해 학습한 word embedding이 된다.
  5. 가중 평균을 낼때 쓰이는 확률이 s[j]이다. 



## 3. Implementation

+ run the biLM and record all of the layer representations for each word. 
+ end task model learn a linear combination of these representations. 
+ Process
  1. freeze the weights of the biLM 
  2. concatenate the ElMo vector ELMo_task[k] with x[k]
  3. pass the ELMo enhanced representation into the task RNN. 











