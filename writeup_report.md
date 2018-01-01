# **Traffic Sign Recognition** 
**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./report/Fig_2_1.png "Visualization"
[image2]: ./report/Fig3_1.png "Smoothing"
[image3]: ./report/Fig3_2.png "Convert"
[image4]: ./report/Fig3_2.png "FromWeb"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/GinYoshida/Udacity_CarND_P2_TrafficSignClassifier)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the python library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799.
* The size of the validation set is 4410.
* The size of test set is 12630.
* The shape of a traffic sign image is (32, 32, 3).
* The number of unique classes/labels in the data set is 43.

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data. 

**Fig2.1. Data distribution**
 
![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

 As a first step, I decided to change the number of training data, because it was far from uniform distribution. 
 For labels, which have less number of data, new data was added with smoothing of original file.

Here is an example of a traffic sign image, which was added as new data.

**Fig.3.1 Data added to uniform the number of each data set**
  
![alt text][image2]


As additional step, I normalized the image data, because each data has different quality, i.e. different brightness.

**Fig.3.2 Example of each label**
  
![alt text][image3]:


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

**Table.2.1 Network description**
   
| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 1x1     	| 1x1 stride, same padding, outputs 32x32x6 	|
| Convolution 1x1     	| 1x1 stride, same padding, outputs 32x32x6 	|
| RELU					|												|
| Convolution 5x5     	| 1x1 stride, same padding, outputs 26x26x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6  				|
| Convolution 5x5     	| 1x1 stride, same padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x6    				|
| Dropout   	      	|   							  				|
| Fully connected		| Inputs:400,  outputs: 120						|
| RELU					|												|
| Fully connected		| Inputs:120,  outputs: 84						|
| Dropout   	      	|   							  				|
| RELU					|												|
| Fully connected		| Inputs:84,  outputs: 60						|

#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I applied the following setting

**Table.3.1 Training model setting**
  
| Parameter  | Setting  |
|---|---|
|Optimizer   			| Adam  |
|Batch size   			| 128  |
|Epoch   				| 40 |
|initial Learning rate	| 0.001 |
|Dropout keep probability   | 0.6  |

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:

 - training set accuracy is 99.5%
 - validation set accuracy is 94.5%
 - test set accuracy is 98.0%

 Original network was copy of LeNet. Only input layer and output layer size was modified. This model showed about 89~90%

 First adjustment was implementation of dropout. Since test accuracy didn't reached 93%. I assumed that this is due to less Regularization of my model. With this implementation of dropout, the accuracy reached about 92~93%.
 
 Then, I added 1 additional 1x1 convolution layer before 1st layer and 1 additional fully connected layer in the last. Because I assumed that more parameters improve the accuracy with successfully implementation of Dropout. With this implementation of dropout, the accuracy reached about 93~95%.

 Finally, I decrease "Drop out keep probability" from 0.9 to 0.6 with 0.1 step during fine tuning of the parameter. This is to keep good regularization level. During this fine turning, to converge the training, "initial learning rate" was decreased and "epoch size" was increased. 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

**Fig.1-1 Image data from web**
  
![alt text][image4] 

The first image might be difficult to classify, because there are similar signes in training set as shown in Fig.3.2, i.e. label 1~8.
Also, the last image is difficult to classify, because there is not correct label in training data set. I expected that network decide this data as 38.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

**Table.2.1 Result of prediction for image data from web**
  
| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 5. Speed limit (80km/h)	| 5. Speed limit (80km/h)					| 
| 25,Road work    			| 25,Road work 								|
| 12,Priority road			| 12,Priority road							|
| 14,Stop	      		| 14,Stop						 				|
| Might be 38,Keep right	| 33,Turn right ahead  							|

 The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. 
 This compares favorably to the accuracy on the test set 98%, since I set the irregular data into 5th data. 

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 20th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a stop sign (probability of 0.65), and the image does contain a stop sign. The top five soft max. probabilities were shown below.

**Table.3-1 5 max. probability of 1st image**
  
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .650		 			| 5,Speed limit (80km/h)						| 
| .123     				| 2,Speed limit (50km/h)						|
| .086					| 1,Speed limit (30km/h)						|
| .079	      			| 3,Speed limit (60km/h)		 				|
| .004				    | 7,Speed limit (100km/h)						|

 For the second ~ forth images, the model is relatively sure that top probability images (probability of 0.99~1.00). And top probability images match with input data. The top five soft max. probabilities were shown below.

**Table.3-2 5 max. probability of 2nd image**
  
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.00				| 25,Road work								| 
| 1.24e-05			| 29,Bicycles crossing						|
| 1.36e-06			| 38,Keep right								|
| 1.32e-06   			| 11,Right-of-way at the next intersection	|
| 1.18e-06		    | 28,Children crossing						|

**Table.3-2 5 max. probability of 3rd image**
  
| Probability         	|     Prediction	        					| 
|:---:|:---:| 
| 1.00	 				| 12,Priority road								| 
| 1.55e-09				| 33,Turn right ahead						|
| 1.04e-09				| 34,Turn left ahead						|
| 2.79e-10   			| 14,Stop									|
| 2.01e-10		    	| 1,Speed limit (30km/h)					|

**Table.3-4 5 max. probability of 4th image**
  
| Probability	|     Prediction| 
|:---:|:---:| 
| 0.993	 				| 14,Stop								| 
| 3.48e-03				| 13,Yield								|
| 2.22e-03				| 17,No entry							|
| 3.51e-04   			| 25,Road work							|
| 1.96e-04		    	| 12,Priority road						|

 For the second ~ forth images, the model is relatively sure that this is a turn left ahead (probability of 0.999). This might be due to combination of arrow shape and coloring.

**Table.3-5 5 max. probability of 4th image**
  
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.999	 				| 34,Turn left ahead						| 
| 3.84e-04				| 17,No entry								|
| 2.27e-04				| 38,Keep right								|
| 3.08e-05   			| 12,Priority road							|
| 1.42e-05		    	| 40,Roundabout mandatory					|

 The model seems to struggle to recognize the number in sign more than shape of them, since the top probability of 1st image was lower than other images.
