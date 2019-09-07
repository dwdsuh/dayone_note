## Improving Language Understanding by Generative Pre-Training



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



