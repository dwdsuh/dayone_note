# BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding



![bert_image](images/Bert.jpg)



+ [source](https://arxiv.org/pdf/1810.04805.pdf)

## 1. Introduction



### 1.1 Two major strategies for applying pre-trained lanauge representation

+ #### Feature-Based Approach

  + ELMo includes the pre-trained representations as additional features

+  #### Fine-Tuning

  + GPT updates all pre-trained parameters while training on the downstream tasks



### 1.2. Limitation of Existing Approaches(GPT)

+ #### Unidirectionality

  + GPT uses only a left-to-right architecture, where every token can only attend to previous tokens in the self-attention layers of the Transformer
  + sub-optimal for sentence-level tasks

### 1.3. Contribution of BERT

+ #### vs. GPT: Overcomes the contraints by Unidirectionality 

  + **MLM**: masked language modeling

  > The masked language model **randomly masks** some of the tokens from the input and the objective is to **predict the original voacabulary id** of the maked word based only on its context

  + **Next Sentence Prediction Task**: jointly pretrains text-pair representations

+ #### vs. ELMo: Deeper 

  + ELMo usesa shallow concatentation of independently trained biLMs.

+ #### SOTA

  + The fist fine-tuning based representation model that achieves SOTA performance on a large suite of sentence-level and token-level tasks. 
  + 11 nlp tasks



## 2. Related work

### 2.1. Unsupervised Feature-based Approaches (such as ELMo)

+ Pre-trained word embeddings, sentence embedding, paragraph embedding
+ ELMo: generalize traditional word embedding research along a different dimension
  + extract context-sensitive features using biLM.

### 2.2. Unsupervised Fine-Tuning Appraches(such as GPT)

### 2.3. Transfer Learning from Supervised Data(such as ImageNet)





## 3. BERT



### 3.1. Model Architecture

![image](images/Bert_Arch.png)

+ BERT's model architecture is a multi-layer bidirectional Transformer encoder

|            | the number of layers(L) | the hidden size(H) | the number of self-attention heads(A) | Total Params |
| ---------- | ----------------------- | ------------------ | ------------------------------------- | ------------ |
| BERT_BASE  | 12                      | 768                | 12                                    | 110M         |
| BERT_LARGE | 24                      | 1024               | 16                                    | 340M         |

+ Since it adopted Transformer **encoder**, it is bidirectional
+ In case of GPT, it adopted Transformer decoder, every token of which can only attend to context to its left.

### 3.2. Input/Output Representations

+ In order to handle **miscellenous down-stream tasks**, both a single sentence and a pair of sentences(QA) should be clearly represented by our input representation

+ Definition of Sentence and Sequence in this paper

  + sentence: an **arbitrary** span of contiguous text, rather than an actual linguistic sentence
  + sequence: **input token sequence to BERT, which may be a single sentence or two sentences packed together

+ Embedding 

  ![embedding](images/Bert_embedding.png)

  + WordPiece embedding with a 30,000 token vocab.
  + \[CLS]: The first token of every sequece is always a special classification token.
    The final hidden state corresponding to this token is used as the aggregate sequence representation for classification tasks.  
  + \[SEP]: a special token that separate sentence pairs packed together into a single sequence
  + Adding a learned embedding to every token indicating whether it belongs to sentence A or sentence B. 

  > E: Input Embedding
  >
  > C: the final hidden state of the special \[CLS] token, the length of which equals H
  >
  > T[i]: the final hidden vector for the i***th*** input token, the length of which equals H





## 4. Traning Procedure 

### 4.1. Pre-training

+ **MLM**

  + In order to train a deep bidirectional representation, BERT simply mask some percentage of the input tokens at random and the predict those masked tokens. 
  + The final hidden vectors corresponding to the mask tokens are fed into an output softmax over the vocabulary. 
  + BERT mask 15% of all WordPiece tokens in each sequence at random. 

  | Downside                                                     | Solution                                                     |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | Mismatch between pre-training and fine-tuning.<br />[Mask] does not appear in downstrean tasks | Not always mask tokens with [Mask]<br />- 80%: [Mask]<br />- 10%: random token<br />- 10%: unchanged |

  

+ Downside:
  + mismatch between pre-training and fine-tuning