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
  + sequence: **input token sequence** to BERT, which may be a single sentence or two sentences packed together

+ Embedding 

  ![embedding](images/Bert_Embedding.png)

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

+ **MLM**(Masked LM)

  + In order to train a deep bidirectional representation, BERT simply mask some percentage of the input tokens at random and the predict those masked tokens. 
  + The final hidden vectors corresponding to the mask tokens are fed into an output softmax over the vocabulary. 
  + BERT mask 15% of all WordPiece tokens in each sequence at random. 

  | Downside                                                     | Solution                                                     |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | Mismatch between pre-training and fine-tuning.<br />[Mask] does not appear in downstrean tasks | Not always mask tokens with [Mask]<br />- 80%: [Mask]<br />- 10%: random token<br />- 10%: unchanged |

+ **NSP**(Next Sentence Prediction)

  + Language modeling can **scarcely** cover tasks which require understadning **the relationship between two sentences**, such as Question Answering and Natural Langugae Inference tasks
  + Binarized Next Sentence Prediction Task
    + label: [IsNext] or [NotNext]
    + ***C*** is used for next sentence prediction
    + **Fine-tuning is necessary** so as ***C*** to be a meaningful sentence representation for QA and NLI tasks

+ **DATA** 

  + BooksCorpus(800M words)
  + English Wikipedia(2500M words) : only text passages
  + Billion Word Benchmark: document-level corpus



### 4.2 Fine-tuning

+ Plug in the task-specific inputs and outputs into BERT, and fine-tune all the parameters **end-to-end**
+ Sentence A and Sentence B at the Input is similar to:
  1. sentence pairs in paraphrasing
  2. hypothesis-premise pairs in entailment
  3. question-passage paris in question answering
  4. degenerate text-∅ pair in text classification or sequence tagging
+ At the output:
  1. **For token-level tasks** , such as sequence tagging or question answering, **the token representations** are fed into an output layer.
  2. **For classification**, such as entailment or sentiment analysis, **the [CLS] representation** is fed into an output layer. 





## 5. Experiments

### 5.1. GLUE(the General Language Understanding Evaluation)

| Task Name                                            | Task                                         | Input                                       | Ouput                                                        | Metric                                          |
| ---------------------------------------------------- | -------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- |
| MNLI<br />Multi_Genre Natural Language Inference     | entailment classification                    | a pair of sentences                         | classify whether the second sentence is an ***entailment(i.e sufficient condition), contradiction, or neutral*** w.r.t the first sentece | Acc                                             |
| QQP<br />Quora Question Pairs                        | binary classification (text similarity)      | a pair of sentences(two questions)          | classify whether two questions are semantically equivalent   | F1                                              |
| QNLI<br />Question Natural Language Inference        | binary classification                        | a pair of sentences(question, sentence)     | classify whether the sentence contain answer for the question | Acc                                             |
| SST-2<br />the Standford Sentiment Treebank          | binary single-sentence classification        | a sentence(English) from movie reviews      | classify whether the sentence is positive or negative        | Acc                                             |
| CoLA<br />the Corpus of Linguistic Acceptability     | binary classification                        | a sentence                                  | classify whether the sentence is liguistically acceptable    | Acc                                             |
| STS-B<br />the Sentence Textual Similarity Benchmark | Prediction(0-5 scale)                        | a pair of sentences                         | Predict the semantically similarity of two sentences with a score from 0 to 5 | Spearman  Correlation<br />(Robust to outliers) |
| MRPC<br />Microsoft Research Paraphrase Corpus       | binary classification<br />(text similarity) | a pair of sentences from online news source | classify whether the sentences are semantically equivalent   | F1                                              |
| RTE<br />Recognizing Textual Entailment              | binary classification                        | a pair of sentences                         | classify whether one sentence is entailment of the other.    | Acc                                             |
| WNLI<br />Winograd NLI                               | ***excluded***                               |                                             |                                                              |                                                 |



+ **Fine-tuning**

  ![image](images/Bert_fine_tuning_clf.png) 

  + Final hidden verctor C, corresponding to [CLS] is used 

  + New Parameter: W, the shape of which is (K, H)

    > K: the number of labels
    >
    > H: the hidden vector size

  + Batch_Size= 32, Epoch=3

  + On small datasets, BERT developers ran several random restarts and selected the best model on the Dev set

  + Score

  ![image](images/Bert_Glue_score.png)



### 5.2. SQuAD v1.1

+ the Standford Question Answering Dataset

+ a collection of 100k crowd-sourced question/answer pairs

  | Task               | Input                                                        | Output                         | Metric                                                       |
  | ------------------ | ------------------------------------------------------------ | ------------------------------ | ------------------------------------------------------------ |
  | Question Answering | a pair of a question and a Wiki passage that contains the answer | the answer span in the passage | EM(Exact Match),<br /> F1<br />[Detailed Info](https://arxiv.org/pdf/1606.05250.pdf) |

  

![image](images/Bert_squad1.png)

+ New parameters: **S**(start vector), **E**(end vector), the size of which is (H,)

  + P(Tok i == start) = softmax([dot(T[i],S) for i in range(M)])
  + P(Tok j == end) = softmax([dot(T[j], E) for j in range(M)])
  + start/end span = argmax(dot(T[i], S) + dot(T[j], E)) s.t. j>=i

+ Epoch: 3

+ Batch_Size: 32

+ result

  ![image](images/Bert_squad1_result.png)

  + Some models are fine-tuned on TriviaQA before fine-tuned on SQuAD

