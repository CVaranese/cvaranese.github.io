---
layout: post
title: "Prediction of Steering Angles"
date: 2018-06-05
excerpt: "Using Keras to predict steering angles from images of the road"
tag:
- Python
- Neural Network
- Numpy
- Keras
- Tesla
project: true
comments: false
---

## Overview
DeepTesla from MIT offers 10 videos of a Tesla Model S being driven on the highway, each with steering angle time stamped with each frame. The videos run at 30 FPS, which was sufficient for DeepTesla for being able to predict the steering angle from looking at a video stream using a Convolutional Neural Network (CNN). Five of the datasets are with the Tesla autopilot driving and the latter half is with a human driver. DeepTesla was able to get good results with their CNN, but we wanted to see if we could improve the error by adding in recurrence in the form of a 3D CNN.

## 2D CNN with Saliency Maps
Our initial test 2D CNN performed much better than we expected, reaching an mse loss of around 1 after 2 epochs (which varied sometimes during training). The saliency maps show various things are picked up to determine the steering angle such as the lane lines, guard rails, and surprisingly enough-- the reflection of the vents!

{% capture images %}
	/assets/img/image5.png
{% endcapture %}
{% include gallery images=images caption="Figure 1. Predicted: -3.5201 , Actual -2.0" %}

3D CNN model
The 3D CNN model resulted at best a mse loss of 13.4 steering angle degrees after three epochs. The most interesting thing about the 3D CNN model was the discovery that the final dense layer had to be the same shape as the number of sequences. For example if the 3D CNN had an input shape of (8, 105, 320, 3), the final dense layer had to be 8 to get Keras to run. The intended architecture was to have a single dense neuron predicting the steering angle. 

		
						
Figure 2. Loss of Conv3D shows overfitting	 Figure 3.  Summary of the Conv3D model

Another particularly interesting problem was the possibility of negative dimensions due to short sequence lengths. For example each Conv3D layer reduced the depth, length, and width by 2--but the MaxPooling3D resulted in a division by half! This required longer sequences, which resulted in GPU resources being exhausted. The solution then became using less layers, which caused the model to quickly over fit. Lastly, an LSTM layer was added after the Conv3D to make use of the temporal information. Unfortunately, the LSTM in Keras could not handle a 5D shape from the Conv3D. This suggests for situations like this, learning Tensorflow would be useful.

Nvidia Jetson TX2 on RC car
Personal video and steering angles were collected using a mechanical engineering senior project RC car. The goal was to take what was learned from the MIT deepTesla dataset and apply it to a real world control problem. Videos were obtained, but time only allowed for a study in saliency maps.

Figure 4.  RC car was used to log video and steering angles

Comments on Results
The project got off to a slow start because of all of the unforeseen difficulties. Initially, separating all of the data from videos into images took some time to figure out. After that, we learned that working with 19GB of images was not an easy task, so we had to preprocess our images by compressing/cropping before moving them around. Then, even when we preprocessed them, our Nvidia Jetson TX2 and the school servers could not load the entire dataset at once, so we took some time looking into methods of only partially loading the dataset, but we instead settled on just processing the images even more--compressing them to an npz file. The 3D CNN eventually required a generator to take random sequences of videos. The team had better results with 2D Convolution, despite the the inclusion of temporal information. The team would suggest looking into Transfer Learning as further research suggested better results with using a pre-trained model such as ResNet50. 



References
[1] “DeepTesla - End-to-End Steering Model.” DeepTesla, selfdrivingcars.mit.edu/deeptesla/.
[2]  Du, Shuyang, et al. “Self-Driving Car Steering Angle Prediction Based on Image Recognition.” Stanford, pp. 1–9. 

