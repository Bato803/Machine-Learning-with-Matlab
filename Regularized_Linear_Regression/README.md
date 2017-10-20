## Introduction

In the mini-project, we implement regularized linear regression and use it to play with bias-variance property. The task is to 
predict the amount of water flowing out of a dam using the change of water level in a reservoir. 


## Visualizing the dataset

We begin by visualizing the dataset whereas the x axis representing the change of water level, and y axis the amount of water
flowing out of the dam.

<img src="https://user-images.githubusercontent.com/17235054/31827020-b8cedc96-b584-11e7-8149-8af8c2cd9838.jpg" width="400" height="300"/>

## Regularized Cost Function
We use square error function as shown:

<a href="https://www.codecogs.com/eqnedit.php?latex=J(\theta)&space;=&space;\frac{1}{2m}(\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y{(i)})^2)&space;&plus;&space;\frac{\lambda}{2m}(\sum_{j=1}^{n}\theta_j^2)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?J(\theta)&space;=&space;\frac{1}{2m}(\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y{(i)})^2)&space;&plus;&space;\frac{\lambda}{2m}(\sum_{j=1}^{n}\theta_j^2)" title="J(\theta) = \frac{1}{2m}(\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y{(i)})^2) + \frac{\lambda}{2m}(\sum_{j=1}^{n}\theta_j^2)" /></a>

where lambda is a regularization parameter controling the degree of regularization to prevent overfitting. It puts a penalty on 
the cost so that all thetas would be kept small, which tends to give us a simpler hypothesis.  The code is in *linearRegCostFunction.m*


## Regularized gradient
To minimize cost function, we need to compute the partial derivative of the regularized cost function:

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{J(\theta)}{\theta_0}=\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\frac{J(\theta)}{\theta_0}=\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}" title="\frac{J(\theta)}{\theta_0}=\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\frac{J(\theta)}{\theta_j}=\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}&space;&plus;&space;\frac{\lambda}{m}\theta_j" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\frac{J(\theta)}{\theta_j}=\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}&space;&plus;&space;\frac{\lambda}{m}\theta_j" title="\frac{J(\theta)}{\theta_j}=\frac{1}{m}\sum^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)} + \frac{\lambda}{m}\theta_j" /></a>



## Train Linear Regressor 
Use linear regression to find the best line to fit the dataset using the cost function and gradient we got from above. 
This model is not expected to fit the data very well since it's the water level and the amount of flowing water has 
a non-linear relation. The learning curve also reflect that this model underfits the data. 

<img src="https://user-images.githubusercontent.com/17235054/31834950-c6f8093c-b59d-11e7-88a9-8ebc76ba2f61.jpg" width="400" height="300">
<img src="https://user-images.githubusercontent.com/17235054/31835193-a6715b0e-b59e-11e7-8fe4-424018624fdf.jpg" width="400" height="300">


## Polynomial Regression
From the previous analysis we know that our model is not complex enough to represent our data, therefore we further address
this problem by adding more features. And the hypothesis has the form:

<a href="https://www.codecogs.com/eqnedit.php?latex=h_\theta(x)&space;=&space;\theta_0&space;&plus;&space;\theta_1*(waterLevel)&space;&plus;&space;\theta_2&space;*&space;(waterLevel)^2&space;&plus;&space;...&space;&plus;\theta_p&space;*&space;(waterLevel)^p" target="_blank"><img src="https://latex.codecogs.com/gif.latex?h_\theta(x)&space;=&space;\theta_0&space;&plus;&space;\theta_1*(waterLevel)&space;&plus;&space;\theta_2&space;*&space;(waterLevel)^2&space;&plus;&space;...&space;&plus;\theta_p&space;*&space;(waterLevel)^p" title="h_\theta(x) = \theta_0 + \theta_1*(waterLevel) + \theta_2 * (waterLevel)^2 + ... +\theta_p * (waterLevel)^p" /></a>


## Learning Polynomial Regression 
Using the cost function and gradient we wrote above, the unregularized polynomial model shows that the training error is almost 
zero but the cross validation error is still very high, indicating the model obviously overfit the data. 

<img src="https://user-images.githubusercontent.com/17235054/31837383-985290e4-b5a6-11e7-9d32-ba9b23b04339.jpg" width="400" height="300">
<img src="https://user-images.githubusercontent.com/17235054/31837382-983c8754-b5a6-11e7-9679-7d82c4f6c6bc.jpg" width="400" height="300">


## Regularization
We decided to add regularization to this model, but we need to figure out to what degree should the regularization be. Therefore
we implement an automated method (*validationCurve.m*) to select the best lambda parameter. We use the cross validation set to evaluate how good each
lambda is. The result is shown below:

<img src="https://user-images.githubusercontent.com/17235054/31837791-12608480-b5a8-11e7-9add-7d3a2400c3b5.jpg" width="400" height="300">

## Result
From the figure above we can see that, the best value of lambda is 3, at that time the cross validation error reach the lowest
point. And as lambda grows larger and larger, the model graduately become underfit the data. That's because we add a large 
penalty term for every feature, and the feature would be close to zero in order to minimize the cost function. The result is shown 
below:

<img src="https://user-images.githubusercontent.com/17235054/31838054-230c4962-b5a9-11e7-98b3-d1f26f4213fd.jpg" width="400" height="300">
<img src="https://user-images.githubusercontent.com/17235054/31838052-22fcd6c6-b5a9-11e7-8b58-23b80f183b86.jpg" width="400" height="300">
