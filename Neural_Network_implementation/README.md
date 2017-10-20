## Introduction

In this mini-project, I implement the backpropagation algorithm for fully connected neural network and apply it to the task of 
hand written digit recognition. 

## Implementation Steps by Steps

### Loading and Visualizing Data
A Visualization of the data is shown below. The data is stored in ex4data1.mat, and there are 5000 training example, where each 
training example is a 20 pixel by 20 pixel grayscale image of the digit. Later, the 20 by 20 grid of pixels is unrolled into 
a 400-dimension vector. 

<img src="https://user-images.githubusercontent.com/17235054/31751466-70404c6c-b453-11e7-8407-e374555a6579.jpg" width="400" height="300"/>

### Implement forward propagation
Forward propagation at this time is implemented without regularization for the purpose of easy debugging. The model we use here
is fully connected neural network, and we choose to use the classic cross-entropy loss function.(*K* is the number of labels, *m*
refers to the number of examples)
<a href="https://www.codecogs.com/eqnedit.php?latex=J(\theta)&space;=&space;\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[-y_{k}^{(i)}log((h_{\theta}(x^{i}))_k)-(1-y_k^{i})log(1-(h_{\theta}(x^{(i)}))_k)]" target="_blank"><img src="https://latex.codecogs.com/png.latex?J(\theta)&space;=&space;\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[-y_{k}^{(i)}log((h_{\theta}(x^{i}))_k)-(1-y_k^{i})log(1-(h_{\theta}(x^{(i)}))_k)]" title="J(\theta) = \frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[-y_{k}^{(i)}log((h_{\theta}(x^{i}))_k)-(1-y_k^{i})log(1-(h_{\theta}(x^{(i)}))_k)]" /></a>

### Add regularization to cost function.
25 is the number of hidden units, and 400 is the size of our input, while 10 refers to the number of output units. 
<a href="https://www.codecogs.com/eqnedit.php?latex=J(\theta)&space;=&space;\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[-y_{k}^{(i)}log((h_{\theta}(x^{i}))_k)-(1-y_k^{i})log(1-(h_{\theta}(x^{(i)}))_k)]&plus;\frac{\lambda}{2m}[\sum^{25}_{j=1}\sum^{400}_{k=1}(\theta^{(1)}_{j,k})^2&plus;\sum^{10}_{j=1}\sum^{25}_{k=1}(\theta^{(2)}_{j,k})^2]" target="_blank"><img src="https://latex.codecogs.com/png.latex?J(\theta)&space;=&space;\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[-y_{k}^{(i)}log((h_{\theta}(x^{i}))_k)-(1-y_k^{i})log(1-(h_{\theta}(x^{(i)}))_k)]&plus;\frac{\lambda}{2m}[\sum^{25}_{j=1}\sum^{400}_{k=1}(\theta^{(1)}_{j,k})^2&plus;\sum^{10}_{j=1}\sum^{25}_{k=1}(\theta^{(2)}_{j,k})^2]" title="J(\theta) = \frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[-y_{k}^{(i)}log((h_{\theta}(x^{i}))_k)-(1-y_k^{i})log(1-(h_{\theta}(x^{(i)}))_k)]+\frac{\lambda}{2m}[\sum^{25}_{j=1}\sum^{400}_{k=1}(\theta^{(1)}_{j,k})^2+\sum^{10}_{j=1}\sum^{25}_{k=1}(\theta^{(2)}_{j,k})^2]" /></a>


### Implement Backpropagation
We compute the gradients for the cost function, and minimize the cost function using advanced optimizer *fmincg*. For the output
node, we directly measure the difference between the network's activation and the ground truth. And for the hidden units, we 
compute their error using chain rule.


### Gradient Checking
Before we go into training the network, which might take some time, we do a safety check to make sure we are computing the 
correct gradient. We compare the numeric gradient with the gradient that we got from the previous step. 

### Leraning parameters using *fmincg*
Obtain a training accuracy about 95.3%


