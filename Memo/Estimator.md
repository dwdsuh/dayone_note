## Implementing tf.estimator/ 텐서플로우 에스티메이터 활용하기

출처(source): 텐서플로와 머신러닝으로 시작하는 자연어처리. 전창욱 외 2명. 위키북스. 2019

### 1. Overview

+ What is Estimator? High level API that helps developers train, evalutate, predict and export the models.

+ 2 major factors

  + model_function

    ```python
    def model_fn(features, labels, mode, params, config):
      # construct your model here
      return tf.estimator.EstimatorSpec(...)
    ```

    > features: Input data. The data structure of those should be either tf.Tensor or dictionary
    >
    > labels: Target
    >
    > mode: Signifies whether the model is used for  train, evaluate, or predict
    >
    > params: Hyperparameters for the model.
    >
    > config: Other configuration of the model. 

    

    + generating tf.estimator.Estimator object

    ```python
    estimator = tf.estimator.Estimator(model_fn = model_fn,
                                      model_dir = model_dir,
                                      configs = ...,
                                      params = ...)
    ```

    

  + data_input_function

    ```python
    def train_input_fn():
      # make a data pipeline
      return features, labels
    ```





### 2. Deep Neural Network utililzing tf.estimator

+ First, define train_input_function

  ```python
  Epoch = 100
  BatchSize = 16
  
  def train_input_fn():
    dataset = tf.data.Dataset.from_tensor_slices((sequences, label))
    #sequeces and label are pre-loaded ndarray variables
    dataset = dataset.repeat(Epoch)
    dataset = batch(Batch_size)
    dataset = dataset.shuffle(len(sequences))
    iterator = dataset.make_one_shot_iterator()
    
    return iterator.get_next()
    
  ```

+ Second, define model_function

  ```python
  VocabSize = len(word_index)+1 #word_index will be determine by the dataset you use
  EmbSize = 128
  
  def model_fn(features, labels, mode):
    
    TRAIN = mode == tf.estimator.ModeKeys.TRAIN
    EVAL = mode == tf.estimator.Modekeys.EVAL
    PREDICT = mode == tf.estimator.ModeKeys.PREDICT
    
    embed_input = tf.keras.layers.Embedding(VocabSize, EmbSize)(features)
    embed_input = tf.reduce_mean(embed_input, axis = 1)
    
    hidden_layer = tf.keras.layers.Dense(128, activation = tf.nn.relu)(embed_input)
    output_layer = tf.keras.layers.Dense(1)(hidden_layer)
    output = tf.nn.sigmoid(output_layer)
    
    loss = tf.losses.mean_squeared_error(output, labels)
    
    if TRAIN:
      global_step = tf.train.get_global_step()
      train_op = tf.train.AdamOptimizer(1e-3).minimize(loss, global_step)
      
      return tf.estimator.EstimatorSpec(mode = mode. 
                                        train_op = train_op,
                                        loss = loss)
  ```

- At last, define run.py

  ```python
  DataOutPath = "someplace/inyourdir/"
  
  import os
  
  if not os.path.exists(DataOutPath):
    os.makedirs(DataOutPath)
   
  estimator = tf.estimator.Estimator(model_fn = model_fn,
                                     model_dir = DataOutPath+"checkpoint/dnn")
  estimator.train(train_input_fn)
  ```

