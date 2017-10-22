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

In this case, we have two features, and for each feature we need to find parameters *mu* and *sigma* to fit the data in that 
dimension. 

<a href="https://www.codecogs.com/eqnedit.php?latex=p(x;&space;\mu,&space;\sigma^2)&space;=&space;\frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?p(x;&space;\mu,&space;\sigma^2)&space;=&space;\frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}" title="p(x; \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}" /></a>
