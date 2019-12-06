# Improving Language Understanding by Generative Pre-Training





## Introduction

### Background

+ the lack of annotated resources

+ leveraging more than word-level info from unlabeled text is challenging
  + hard to decide the type of optimization objectives
  + Do not know how to transfer these learned representations to the target task. 
    Recent works tried:
    + making task-specific changes to the model architecture
    + using intricate learning schemes
    + adding auxiliary learning objectives

### GPT

+ two-stage approach:
  1. a language modeling objective on the unlabeled data to learn the initial parameters of a neural model (Transformer)
  2. adapt these parameters to a target task using the corresponding supervised objective (Traversal Style)



### Evaluation

+ Four tasks
  1. natural language inference
  2. question answering
  3. semantic similarity
  4. text classification

+ result
  + SOTA in 9 out of the 12 tasks
  + zero-shot behaviors



# Language Models are Unsupervised Multitask Learners

- GPT2

## Approach

- Our speculation is that a language model with sufficient capacity will begin to learn to infer and perform the tasks demonstrated in natural language sequences in order to better predict them, regardless of the method of procurement. 
- If a language model is able to do this it will be, in effect, performing unsupervised mulltitask learning.

## Model

+ followed detail of GPT
+ Normalization
  + layer normalization was moved to the input of each sub-block
  + additional layer normalization was added after the final self-attention block
+ a modified initialization (which account for the accumulation on the residual path with model depth)
  + the paper scale the weights of residual layers at initialization by a factor of 1/sqrt(N), where N is the number of residual layers. 

residual connnection: the output of each sub-layer is LayerNorm(x+Sublayer(x)), where Sublayer(x) is the function implemented by the sub-layer itself.

weather_en__informOzone