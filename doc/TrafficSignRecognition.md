# Traffic Sign Recognition
### 2024, Spring - Szabo Lorand

## Table of Contents:
1. Introduction
2. Theoretical Aspects
3. Methods of Implementation
4. Algorithm
5. Implementation
6. Evaluation & Result
7. Sources

## 1. Introduction
___Traffic Sign Recognition___ (TSR) is a technology developed to recognize traffic signs and displays them on the driver's dashboard. It is part of the _advanced driver-assistance systems_ (ADAS) that uses cameras and sensors to provide for the driver additional information in order to increase road safety and enables various levels of _autonomous driving_. Other technologies part of ADAS are, for instance, adaptive cruise control, lane departure detector, parking sensors and blind spot monitor.

This is a university project that demonstrates some methods to implement a traffic sign recognition system, focusing mostly on classical _image processing_ methods, rather than on _artifficial intelligence_ and _computer vision_.

## 2. Theoretical Aspects

Traffic signs were standardized in 1968 by the _Vienna Convention on Road Signs and Signals_ treaty. This means that this convention enables the consistent use of traffic signs and the implementation of traffic sign recognition.

They are highly visible signs next to or on the roads, typically featuring a border of a specific color (e.g. red with white or yellow background, or blue background and white marking), and an easily interpretable drawing or text in the center of it. The shape of the signs are designed to be easily recognizable (triangle, circle), some important signs having unique shapes, for instance the Give Way or Stop Ahead signs.

Therefore, an ideal approach for traffic sign recognition involves a shape detection algorithm. This can be achieved by modifying the original image into a binary one using the _Canny edge detection_ algorithm (because of the signs' standardized shapes) or using _color based segmentation_ (because of the signs' standardized colors).

## 3. Methods of Implementation

1. Using Convolutional Neural Networks

The most precise and accurate solutions for the traffic sign recognition are implemented using convolutional neural networks (CNNs). These models can classify road signs with over 95% accuracy, being approximately 20% more accurate than classical image processing algorithms.

These methods involve training an AI model using neural networks with convolutional layers. In principle, convolutional layers excel in image recognition and segmentation because the weights of the network depend only on a certain pixel's neighbours. Consequently, it will recognize patterns within a given window's size, resulting in fewer nodes (pixel) at the subsequent layer. Therefore, at the next convolutional layer, it will detect features that are larger in the original image.

2. Using Image Processing algorithms

The following example demonstrates Traffic Sign Segmentation based on _canny edge detection_ and _color based segmentation_.

The steps of the image processing pipeline is the following:

![alt text](image_processing_pipeline.png)

- firstly, the image has to be preprocessed, namely denoised, resized and its contrast is enhanced
- after that, the image is processed using _canny edge detection_ with _shape detection_ and _color based segmentation_ with _shape detection_, separately
- both paths result in outputs that represent the contour of the sign
- the last steps are combining the outputs and computing the bounding box for the merged output

An example for tracing the above steps:

![alt text](image_processing_pipeline2.png)

To achieve traffic sign recognition, the detection (segmentation) phase has to be followed by an algorithm that classifies the output of the above implementation.

## 4. Algorithm

## 5. Implementation

## 6. Evaluation & Result

## 7. Sources
[1] Algorithm using Convolution Neural Nets: https://www.analyticsvidhya.com/blog/2021/12/traffic-signs-recognition-using-cnn-and-keras-in-python/

[2] Comparison of ML solutions: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10223536/#:~:text=Traffic%20sign%20recognition%20can%20be%20divided%20into%20machine%20learning%20and,NN)%2C%20and%20decision%20trees.

[3] Traditional Traffic Sign Segmentation pipeline: https://jq0112358.medium.com/traffic-sign-segmentation-with-classical-image-processing-methods-canny-edge-detection-color-8ff1096535db

[4] Canny edge detection: https://docs.opencv.org/4.x/da/d22/tutorial_py_canny.html

[5]