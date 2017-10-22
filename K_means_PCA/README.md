## Introducation 

In this mini-project, we will implement the K-means clustering algorithm and apply it to compress an image. In the second part, 
we will use principal component analysis to find a low-dimensional representation of face images. 

## K-means Clustering

We started by applying K-means algorithm on a 2D dataset, and implement the two phases of K-means algorithm separately. 

- Assigning each training example to its cloest centroid. 
- Recomputing the mean of each centroid using points assigned to it. 

### Finding Cloest Centroids

In the "cluster assignment" phase, we assign every example to its cloest centroid, given the current position of centroid. Specifically,

<a href="https://www.codecogs.com/eqnedit.php?latex=c^{(i)}&space;:=&space;j&space;\quad&space;minimize&space;\quad&space;||x^{(i)}-\mu_j||^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?c^{(i)}&space;:=&space;j&space;\quad&space;minimize&space;\quad&space;||x^{(i)}-\mu_j||^2" title="c^{(i)} := j \quad minimize \quad ||x^{(i)}-\mu_j||^2" /></a>

### Computing Centroid Means

Given assignment of every point to a centroid, the second phase of the algorithm recomputes the mean of the points that were 
assigned to the same centroid. Specifically, for every centroid k, we set

<a href="https://www.codecogs.com/eqnedit.php?latex=\mu_k&space;:=&space;\frac{1}{|C_k|}&space;\sum_{i\in&space;C_k}x^{(i)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\mu_k&space;:=&space;\frac{1}{|C_k|}&space;\sum_{i\in&space;C_k}x^{(i)}" title="\mu_k := \frac{1}{|C_k|} \sum_{i\in C_k}x^{(i)}" /></a>

### Apply it on sample dataset

We produce a visualization that steps through the algorithm at each iteration. Shown as below:

<img src="https://user-images.githubusercontent.com/17235054/31856805-2bb47080-b699-11e7-9b4b-e693fd7026a2.jpg" width=400 height=300>

## Image Compression with K-means

The image we are going to work on shows as follows:

<img src="https://user-images.githubusercontent.com/17235054/31856833-2eed62e2-b69a-11e7-8864-205f5414eafd.png" width=200 height=150>

This is a `(128,128)` image, with 24-bit color representation. Each pixel is represented as three 8-bit unsigned integers (ranging
from 0 to 255) that specify RGB itensity. And we will reduce the number of colors to 16 colors. 

Specifically, we will store the RGB values of the 16 selected colors, and for each pixel in the image we now need to only store 
the index of the color at that location. (where only 4 bits are necessary to represent 16 possible locations). 

And we will use k-means algorithm to select the 16 colors. Concretely, we will treat every pixel in the original images as a data 
example and use the K-means algorithm to find the 16 colors that best group pixels in the RGB space. Once we have computed the 
cluster centroids on the image, we then use the 16 colors to replace the pixels in the original image. 

The trajectory of the selected colors goes as follows, the x and y axis has value 0 from 1. That's because during the image 
preprocessing stage, we divided their value by 255. 

<img src="https://user-images.githubusercontent.com/17235054/31856887-ed9a8282-b69b-11e7-8eee-81c23e9e7753.jpg" width=400 height=300>


Notice that during this process, we have significantly reduced the number of bits that are required to describe the image. The 
original image require 24 bits for each of the `(128,128)` pixel locations, resulting in total size of 128*128*24 = 393,216 bits.
The new representation requires some overhead storage in form of a dictionary of 16 colors, each of which require 24 bits, but 
the image itself then only requires 4 bits per pixel location. The final number of bits used is 16*24 + 128*128*4=65,920 bits, which 
corresponds to compressing the original image by about a factor of 6. 

The image before and after compression goes as follows:

<img src="https://user-images.githubusercontent.com/17235054/31856913-d0aadda6-b69c-11e7-8881-dac941cf2fab.jpg" width=800 height=300>


## Principal Component Analysis (PCA)

In this part of the mini-project, we will use PCA to perform dimension reduction. We will first experiment it with an example 
2D dataset, and then use it on a bigger dataset of 5000 face image dataset. 

### Example dataset

The example dataset is visualized as below:

<img src="https://user-images.githubusercontent.com/17235054/31856964-e8b1df56-b69e-11e7-9756-c4ac8cbd86c0.jpg" width=400 height = 300> 

### Implementing PCA

To implement PCA, we first compute the covariance matrix of the data. Then, we use MATLAB's built-in function SVD to compute 
the eigenvectors. These will corespond to the principal components of variation in the data. The implementation detail goes as 
follows:

- Normalization. Subtracting the mean value of each feature from the dataset, and scaling each dimension so that they are in 
the same ranage. 
- Compute the covariance matrix of the data:

     <a href="https://www.codecogs.com/eqnedit.php?latex=\sum&space;=&space;\frac{1}{m}X^TX" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\sum&space;=&space;\frac{1}{m}X^TX" title="\sum = \frac{1}{m}X^TX" /></a>
  
- After computing the covariance matrix, we run SVD on ti to compute the principal components. 

The principal component found is plotted as follows:

<img src="https://user-images.githubusercontent.com/17235054/31857019-312ef2a4-b6a0-11e7-97b3-62853e5f9466.jpg" width=400 height=300>

### Dimension Reduction with PCA

After computing the principal components, we use them to reduce the feature dimension of sample dataset by projecting each 
example onto a lower dimension space. 

We first project the 2 dimension data into one dimension, concretely, we are projecting each sample data into the top component in the eigenvector. We got an vector with the size of dataset after this step.

Then we recover the data by projecting them back onto the original high dimensional space. And it's visualized as follows, the original data points are indicated with the blue circles, while the projected data points are indicated with red circles. The projection effectively retains the information in the direction given by the eigenvector. 

<img src="https://user-images.githubusercontent.com/17235054/31857089-41aaf144-b6a2-11e7-994e-840abddbee0d.jpg" width=400 height=300>

## Face Image Dataset

In this part of mini-project, we will run PCA on face images. The dataset contains 5000 faces and each `(32,32)` in grayscale. 
Each row of the matrix contains one face image. First 100 faces are shown below:

<img src="https://user-images.githubusercontent.com/17235054/31857104-c51c5d88-b6a2-11e7-8375-a784422c66a4.jpg" width=400 height=300>

### PCA on Faces

To run PCA on faces, we first normalize the dataset by substracting the mean of each feature from the data matrix. And then run to PCA to obtain principal components of the dataset. Each row in the principal component is a vector of length 1024, and we reshape them back into `(32,32)`. And some of them are visualized as follows:

<img src="https://user-images.githubusercontent.com/17235054/31857137-ad904aca-b6a3-11e7-9fca-9df0f80b6037.jpg" width=400 height=300>

### Dimension Reduction on Faces

Now that we have the principal components of face dataset, this allows us to reduce the dimension from 1024 dimension into a 
smaller dimension to speed up our learning algorithm if we wanted to train our model on this dataset. 

Next, we project the face dataset onto only the first 100 principal components. After that, each face is now described by a vector of 100, instead of 1024. 

To understand what is lost in the dimension reduction, we recover the data using only the projected dataset. The result is shown below:

<img src="https://user-images.githubusercontent.com/17235054/31857162-6cda1708-b6a4-11e7-842e-504273e1f06f.jpg" width=800 height=600>

We can infer from the reconstruction that, the general structure and appearance of the face are kept while the fine details are lost. 
