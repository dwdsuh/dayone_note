[link](https://arxiv.org/pdf/1212.5701.pdf)

# Optimizer



AdaDelta: an adaptive learning rate method

## Contribution

- no manual setting of a learning rate
- insensitive to hyperparameters
- separate dynamic learning rate per-dimension
- minimal cimputation over gradient descent
- robust to large gradients, noise, and architecture choice
- applicable in both local or distributed environment



### 2.1 Learning Rate Annealing

- speed up learning when suitable
- slow down learning near a local minima 
- manually designate the number of steps before the learning rate starts to decay

### 2.2 Per-dimension First Order Methods

+ Momentum
  + accelerate progress along dimesions in which gradient consistently point in the same direction and to slow progress along dimensions where the sign of the gradient continues to change. 
  + delta(x[t]) = decay_rate * delta(x[t-1]) - learning_rate * gradient[t]
  + useful when objective surface is valley-shape
+ Adagrad
  + the denomiator compute the L2 norm of all previous gradients on a per-dimension basis
  + delta(x[t]) = - learning_rate * gradient[t] / L2_norm(gradient[:t-1])
  + the progress along each dimension evens out over time.
  + the scale of the gradeients in each layer is often different by several orders of magnitude, so the optimal larning rate should take that into account. 
  + the accumulation of gradients in the denominator has the same effects as annealing, reducing the learning rate over time.
  + drawbacks
    + sensitive to initial conditions of the parameters and the corresponding gradients
    + the learning rate will continue to decrease throughout training, eventually decreasing to zero and stopping traning completely. 
+ Using Second Order Information
  + expensive 



## 3. AdaDelta Method

+ Idea1: accumulate over window
  + While adagrad accumulate all squared gradients, AdaDelta restricted the window of past gradients that are accumulated to be some fixed size ***w***.
  + Since storing ***w*** previous grads is inefficient, AdaDelta implements the accumulation as an exponentially decaying average of the squared gradients. 
  + E[g^2]\[t] = decay_rate * E[g^2]\[t-1] + (1-decay_rate) * g[t]^2