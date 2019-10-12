# Batch Normalization in NN



[link](https://towardsdatascience.com/batch-normalization-in-neural-networks-1ac91516821c)



+ Batch Normalization reduces the amount by what the hidden unit values shift around(covariance shift)



## Covariance shift

If an algorithm learned some X to Y mapping, and if the distribution of X changes, then we might need to retrain the learning algorithm by trying th align the distribution of X with the distribution of Y. 



## How does Batch Norm work?

+ Batch Norm normalizes the output of a previous activation layer by subtracting the batch mean and dividing by the batch standard deviation
+ After the shift/scale of activation outputs by some randomly initialized parameters, the weights in the next layer are no longer optimal. 
+ SGD undoes this normalization if it's a way for it to minimize the loss function
+ Batch Normlization adds two trainable parameters to each layer, so the normalized output is multiplied by a sd parameter and add a "mean" parameter. 
+ batch normalization lets SGD do the denormalization by changing only these two weights for each activation, instead of losing the stability of the network by changing all the weights. 





[Further Reading](https://arxiv.org/pdf/1502.03167v3.pdf)

