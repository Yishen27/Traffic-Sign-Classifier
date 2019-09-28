# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report

[//]: # (Image References)

[image1]: ./examples/distribution.png "distribution"
[image2]: ./examples/layers.png "layers"
[image3]: ./final_test/00.ppm "Traffic Sign 0"
[image4]: ./final_test/01.ppm "Traffic Sign 1"
[image5]: ./final_test/02.ppm "Traffic Sign 2"
[image6]: ./final_test/03.ppm "Traffic Sign 3"
[image7]: ./final_test/04.ppm "Traffic Sign 4"

I'm not sure if I'm doing the image insertion right, I cannot see the result but only the codes. If not, the images can be found in the corresponding folder. 

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! 

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 26*25
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

I used numpy to analyze and visualize the data sets. In the "examples" folder you can find a bar chart shows the distribution of the test data set and in the html file you can see that I counted all the different labels in three data sets. I found out the distribution of the classes in data sets are not even. This might cause overfitting to some degree, since some classes have far more data than others.

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

My pre-procssing mainly include normalization by using the fomula (pixel - 128)/ 128 to make the data has mean zero and equal variance. Actually this is the only pre-process step I take in my final code. I tried to use gray scale images and spent a lot of time on it, only to find out that my pipline worked better (slightly thoung, around 1% better) with RGB image.

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

Here is my layer architecture:
![alt text][image2]


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an AdamOptimizer and a batch size of 128 inconsideration of the memory issue. I adjusted the epochs to 25 and learning rate to 0.005 after some experiments. I found this combination can balance traning time and accuracy fairly well.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 99.7%
* validation set accuracy of 94.8% 
* test set accuracy of 93.6%

I started with the LeNet-5 model we practiced in the previous lesson. I chose it because it was proved to be suitable for image classification and I'm familiar with it. I tried it without any change and got a accuracy around 90%. I found out the training set accuracy was high but valid set accuracy was not as high, so I bet it was a overfitting problem. I added a dropout layer before the final layer, tuned the hyper parameters several times and finally got this working model.

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

My testing examples were included in the folder "final_test".

For 00, the truck shape inside might be difficult to be classified. For 01, I think this is the most difficult one to classify since it has some kind of warp. For 02, I think it is quiet clear to indentify. Talking about 03, it is smaller and much more blur after reshaping thus this might cause difficult when fed into the model. Finally for 04, I think the lightness of the figure might be a difficulty.
![alt text][image3] ![alt text][image4] ![alt text][image5] ![alt text][image6] ![alt text][image7]

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

I got a 60% accuracy with the images downloaded from the internet. Here are the results (prediction-real label): 11,Right-of-way at the next intersection -11,Right-of-way at the next intersection; 14,Stop-33,Turn right ahead; 20,Dangerous curve to the right - 38,Keep right; 1,Speed limit (30km/h)-1,Speed limit (30km/h); 16,Vehicles over 3.5 metric tons prohibited-16,Vehicles over 3.5 metric tons prohibited.

Comparing to the accuracy of test set, this result is not as ideal as it should be since my pipeline had a 93% accuracy on the test set. The reason of this might be the small number of test images, also could be the similarity between the predited result and the real images.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)
The result of softmax probabilities can be found in the HTML file and the notebook. 

For example, the first image has a probability of 0.99 refers to "Right-of-way at the next intersection", and it is right.The model is very certain about its judegment.

For the second image, a probability of 0.85 is shown for "Stop", however the right answer is "Turn right ahead". It follows a probability of 0.148 for "No entry", the two close numbers shows the model is not very certain about the prediction I think. But the two top results are indeed kind similar.

Image No.3, a probability of 0.997 for "Dangerous curve to the right", 0.0019 for "Yield", both not right.

Image No.4, a probability of 0.99 for right answer "Speed limit (30km/h)", 5.5 for "Priority road".

Image No.5, a probability of 0.99 for "Vehicles over 3.5 metric tons prohibited" and got it right.13.0 for "No passing for vehicles over 3.5 metric tons". 

The probability numbers can show that in the right case, the first will be much laeger (often over 99.9%) than the others while in false cases difference in numbers aren't that big.


