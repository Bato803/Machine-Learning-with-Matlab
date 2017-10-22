## Introduction 

In this mini-project, we will implement anomaly detection algorithm and apply it to detect failing servers on a network. In the second 
part, we will use collaborative filtering to build a recommender system for movies. 

## Anomaly Detection

The features we use to measure server computer are throughout(mb/s) and latency (ms) response of each server. While the servers
were operating, we collect `m=307` examples of how they are behaving, and thus obtain an unlabeled dataset. We expect most of 
them are operating normally. 

We would first fit a Gaussian model to detect anomalous examples in a 2D dataset. The dataset is visualized as below:

<img src="https://user-images.githubusercontent.com/17235054/31862939-a606e36a-b714-11e7-87d6-dfdb767ccd51.jpg" width=400 height=300>

On this dataset, we are going to fit a Gaussian distribution and then find values that have very low probability. 


### Gaussian Distribution

In this case, we have two features, and for each feature we need to find parameters *mu* and *sigma* to fit the training set in that dimension. 

<a href="https://www.codecogs.com/eqnedit.php?latex=p(x;&space;\mu,&space;\sigma^2)&space;=&space;\frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?p(x;&space;\mu,&space;\sigma^2)&space;=&space;\frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}" title="p(x; \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}" /></a>

### Estimating Parameters for a Gaussian

We estimate the parameters *mu* and *sigma* for each features by using the following equations:

<a href="https://www.codecogs.com/eqnedit.php?latex=\mu_i&space;=&space;\frac{1}{m}\sum^m_{j=1}x_i^{(j)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\mu_i&space;=&space;\frac{1}{m}\sum^m_{j=1}x_i^{(j)}" title="\mu_i = \frac{1}{m}\sum^m_{j=1}x_i^{(j)}" /></a>


<a href="https://www.codecogs.com/eqnedit.php?latex=\sigma_i^2=\frac{1}{m}\sum_{j=1}^{m}(x_i^{(j)}-\mu_i)^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\sigma_i^2=\frac{1}{m}\sum_{j=1}^{m}(x_i^{(j)}-\mu_i)^2" title="\sigma_i^2=\frac{1}{m}\sum_{j=1}^{m}(x_i^{(j)}-\mu_i)^2" /></a>

Then we visualize the contours of the fitted Gaussian distribution. 

<img src="https://user-images.githubusercontent.com/17235054/31863130-184c27b2-b717-11e7-8b4a-fae1d3a31924.jpg" width=400 height=300>


### Select the Threshold

At this part, we will implement an algorithm to select the threshold using the f1 score on a cross validation set. Below are the steps we have taken to select the threshold.

- Use the Multivariate Gaussian we computed in the last part to compute the probability density of every example in the validation set. 
- Use a loop to try many different values of threshold, and select the best threshold based on f1 score. 
- Using the threshold from cross validation set, figure out which data points in the training set are anomaly and visualize it. 

The best f1 score we found is 0.875. 
<img src="https://user-images.githubusercontent.com/17235054/31863642-531930b8-b71e-11e7-9350-e5d64349b076.jpg" width=400 height=300>

### High dimensional dataset

Later we re-train and run our model on a more realistic and much harder dataset. Each example is described by 11 features, capturing many more properties of our computer servers. 

We found 117 anomalies in our examples and the best f1 score is 0.62. 


## Recommender Systems

In this part of the mini-project, we will implement collaborative filtering learning algorithm and apply it to a dataset of movie ratings. This dataset consisted of ratings on a scale of 1 to 5. The dataset has 943 users and 1682 movies. The objective of collaborative filtering is to predict movie ratings for the movies that users have not yet rated. This will allow us to recommend the movies with the highest predicted ratings to the user.


### Collaborative filtering learning algorithm

The collaborative filtering cost function is given by

<a href="https://www.codecogs.com/eqnedit.php?latex=J(x^{(1)},...,&space;x^{(n_m)},&space;\theta^{(1)},&space;...,&space;\theta{(n_u)})&space;=&space;\frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^Tx^{(i)}-y^{(i,j)})^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?J(x^{(1)},...,&space;x^{(n_m)},&space;\theta^{(1)},&space;...,&space;\theta{(n_u)})&space;=&space;\frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^Tx^{(i)}-y^{(i,j)})^2" title="J(x^{(1)},..., x^{(n_m)}, \theta^{(1)}, ..., \theta{(n_u)}) = \frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^Tx^{(i)}-y^{(i,j)})^2" /></a>

The parameters that we are going to learn is *X* and *theta*, they are (number of movie, 100) and (number of user, 100) matrix respectively. And we'll use off-the-shelf minimizer *fmincg* to minimize the cost. 


### Collaborative filtering gradient

The gradient of the cost function is given by

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{\partial&space;J}{\partial&space;x_k^{(i)}}&space;=&space;\sum_{j:r(i,j)=1}((\theta^{(j)})^T&space;-&space;y^{(i,j)})\theta_k^{(j)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\frac{\partial&space;J}{\partial&space;x_k^{(i)}}&space;=&space;\sum_{j:r(i,j)=1}((\theta^{(j)})^T&space;-&space;y^{(i,j)})\theta_k^{(j)}" title="\frac{\partial J}{\partial x_k^{(i)}} = \sum_{j:r(i,j)=1}((\theta^{(j)})^T - y^{(i,j)})\theta_k^{(j)}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{\partial&space;J}{\partial&space;\theta_k^{(j)}}&space;=&space;\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)}&space;-&space;y^{(i,j)})x_k^{(i)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\frac{\partial&space;J}{\partial&space;\theta_k^{(j)}}&space;=&space;\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)}&space;-&space;y^{(i,j)})x_k^{(i)}" title="\frac{\partial J}{\partial \theta_k^{(j)}} = \sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})x_k^{(i)}" /></a>


### Regularized cost function

Add regularization to the original cost function and their graident.

### Learning movie recommendations

**We can add our own movie preference, so that later when we train the algorithm, it can recommend movie for us!**



