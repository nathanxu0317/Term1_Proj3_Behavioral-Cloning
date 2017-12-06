# **Behavioral Cloning** 

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/placeholder.png "Model Visualization"
[image2]: ./examples/placeholder.png "Grayscaling"
[image3]: ./examples/placeholder_small.png "Recovery Image"
[image4]: ./examples/placeholder_small.png "Recovery Image"
[image5]: ./examples/placeholder_small.png "Recovery Image"
[image6]: ./examples/placeholder_small.png "Normal Image"
[image7]: ./examples/placeholder_small.png "Flipped Image"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* nv_model_github.py containing the script to create and train the model
* nv_generator_github.py containing the script to create the training data
* drive.py for driving the car in autonomous mode
* nv_model_github.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

Layer (type)                     Output Shape          Param #     Connected to                     
====================================================================================================
lambda_1 (Lambda)                (None, 160, 320, 3)   0           lambda_input_1[0][0]             
____________________________________________________________________________________________________
cropping2d_1 (Cropping2D)        (None, 90, 320, 3)    0           lambda_1[0][0]                   
____________________________________________________________________________________________________
Conv1 (Convolution2D)            (None, 43, 158, 24)   1824        cropping2d_1[0][0]               
____________________________________________________________________________________________________
Conv2 (Convolution2D)            (None, 20, 77, 36)    21636       Conv1[0][0]                      
____________________________________________________________________________________________________
Conv3 (Convolution2D)            (None, 8, 37, 48)     43248       Conv2[0][0]                      
____________________________________________________________________________________________________
Conv4 (Convolution2D)            (None, 6, 35, 64)     27712       Conv3[0][0]                      
____________________________________________________________________________________________________
Conv5 (Convolution2D)            (None, 4, 33, 64)     36928       Conv4[0][0]                      
____________________________________________________________________________________________________
dropout_1 (Dropout)              (None, 4, 33, 64)     0           Conv5[0][0]                      
____________________________________________________________________________________________________
flatten_1 (Flatten)              (None, 8448)          0           dropout_1[0][0]                  
____________________________________________________________________________________________________
FC1 (Dense)                      (None, 100)           844900      flatten_1[0][0]                  
____________________________________________________________________________________________________
FC2 (Dense)                      (None, 50)            5050        FC1[0][0]                        
____________________________________________________________________________________________________
FC3 (Dense)                      (None, 10)            510         FC2[0][0]                        
____________________________________________________________________________________________________
dense_1 (Dense)                  (None, 1)             11          FC3[0][0]                        
====================================================================================================

#### 2. Attempts to reduce overfitting in the model

The model contains dropout layers in order to reduce overfitting.
At the same time, for the first 3 Conv2D layer, 'subsample' has been set to (2,2), in order to avoid generate too many parameters.

The model was trained and validated on different data sets to ensure that the model was not overfitting (code line 10-16). The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate is set to 0.0001

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road ... 
But, I believe in my dataset, I use too many recovering data, that's why in the final test (in the video), the car will keep driving off the center line and recovering back. In order to get better performance, the dataset shoule be fine tuned.

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The purpose of this proj is similar to this paper https://devblogs.nvidia.com/parallelforall/deep-learning-self-driving-cars/
So I start from the Nvidia CNN, but in order to prevent overfitting one dropout layer has been added.

#### 2. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

![alt text][image2]

I then recorded the vehicle recovering from the left side and right sides of the road back to center so that the vehicle would learn to .... These images show what a recovery looks like starting from ... :

![alt text][image3]
![alt text][image4]
![alt text][image5]

Then I repeated this process on track two in order to get more data points.

To augment the data sat, I also flipped images and angles thinking that this would ... For example, here is an image that has then been flipped:

![alt text][image6]
![alt text][image7]

Etc ....

After the collection process, I had X number of data points. I then preprocessed this data by ...


I finally randomly shuffled the data set and put Y% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was Z as evidenced by ... I used an adam optimizer so that manually training the learning rate wasn't necessary.