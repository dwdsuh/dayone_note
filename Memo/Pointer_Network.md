# Pointer Network

## Motivation

+ It is hard to apply Seq2Seq models to combinatorial problem

  + Seq2Seq models requires the size of the ouput dictionary(vocab size in NLP) to be fixed a priori

  + in combinatorial probelms, the size of the output dictionary depends on the length of the input sequence.

    

    ## Idea

+ repurposing the attention mechanism to create pointers to input elements. (Ptr-Nets)



## Contributions

+ satisfactory solutions to three combinatorial optimization problems:
  + planar convex hulls
  + delaunay triangulations
  + symmetric planar Travelling Salesman Problem
+ Proposal of a new architecture: Pointer Net. 
  + using a softmax probability distribution as a "pointer"



## Seq2Seq Model

+ Equation

![rnn equation](images/pointer_network_1.png)



+ C_i in the equation is a fixed length vector representing the indices in the output dictionary





## Pointer Network

+ Equation

![rnn equation](images/pointer_network_2.png)

+ our problem: the size of the output dictionary is equal to the length of the input sequence. 
+ u-i_j is the energy of j-th input to predict the i-th output

+ it's a simple modification!!

