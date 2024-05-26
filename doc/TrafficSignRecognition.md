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

This project will implement the second approach from above, namely the traditional image processing algorithm. The idea is recognizing the sign by its form - achieved by the _canny edge detection_ algorithm, respectively by _color based segmentation_, both results applied to a _shape detection_ algorithm. The 2 outputs from the 2 branches are integrated and a bounding box is calculated the contains the traffic sign.

On the other hand, the type of the sign has to be determined by the shape of its border and by the color of it, respectively if this was not enough, its content. For this a _shape classification_ and a _color detection_ algorithm would be ideal.

## 5. Implementation

Details of each step of the pipeline:

 1. __Image Preprocessing__ - filters have to be added to enhance some properties of the image and clean it from noise.

    1.1. __Noise Filtering__
    
    Median Filtering is applied to the original image so that it removes the unexpected and peak pixel values by choosing the median of the pixel's 8-neighbourhood.

    The choice is made by sorting the neighbourhood elements and choosing the value in the middle, illustrated below:

    ![alt text](median_filtering.png)

    1.2. __Contrast Enhancement__

    Contrast enhancement algorithms usually stratch the histogram of the image and modify the cumulative distribution function, increasing the spectrum of the colors.

    However, the images have 3 color channels (BGR), therefore, they have to be converted to grayscale images before applying the contrast enhancement algorithm. The grayscale image's histogram will be modified and all 3 color channels will be modified relative to it.

    For the BGR to grayscale conversion the NTSC formula is employed that represent's the people's relative perception of brightness of the 3 color channels: 0.299 * Red + 0.587 * Green + 0.114 * Blue.

    After that, the histogram is stretched to cover the whole spectrum (from 0 to 255) and all the colors are interpolated from the old spectrum to the new one.

    After the application this is how the 3 color's distribution will look like compared to the original:

    ![alt text](distribution.png)

    This method scales every color, thus, the resulting image will be darker than expected. Therefore, it's not used in this implementation.

3. __Color Based Segmentation__

This is an approach of detecting features by creating 4 different images based on certain colors that are found on the traffic signs, more specifically, red, blue, yellow and black. By applying the correct threshold for the values of the colors, the algorithm will result in an image that displays only the pixels that were in the given range.

Before color segmentation:

![alt text](noisy_image.png)

After color segmentation and extraction the red component (image):

![alt text](red.png)

3. __Shape Detection__

To apply shape detection algorithms simpler it is recommended to extract the contours of the color-segmented images. In this case, given as input the output of the color segmentation algorithm, all the white (object) pixels that have a black (background) pixel in their neighbourhood will be contour pixels (white) and all the others black.

The result will look like this:

![alt text](contour.png)

4. __Shape Classification__

The next step of the pipeline is contour classification using the __Douglas-Peucker algorithm__ to get the number of nodes the polygon of the respective shape has and classifying it into the following categories: triangle (3 nodes), rectangle (4), stop sign (6 or 8) or complex shape.

When this part return Triangle class, it is checked whether the center of gravity of the shape is located below the middle of the object's height, meaning that it is a _Normal triangle_, or above it, meaning it is _Upside down_.

In case this algorithm returns complex shape, __Circle Hough Transform algorithm__ is called to decide if the the shape is a circle or not.

Therefore, the classes that can be obtained from this classification are: Triangle (Upside Down), Triangle (Normal), Square, Rectangle, Stop, Circle, Complex shape or Unknown.

 Bounding Box Detection

This action is performed in the same functions as the shape classification and calculated based on the contour's maximum and minimum coordinates. It is crucial for localizing each traffic sign.

5. __Traffic Sign Classification__

Based on the color and the shape of the connected components, the traffic sign is derived using a nested if statement. The classes detectable are:
- concrete traffic signs: Give way, Stop, Priority Road
- group of signs: Warning/Danger sign, Prohibitory sign, Information sign and Mandatory sign.

This is how the output of this segment looks like:

![alt text](classification.png)

6. __Filtering the Shapes__

At the end of the pipeline a cleaning has to be made in order to remove the relatively small shapes, because in most of the cases they are noise. On the other hand, bounding boxes completely contained in another one are redundant and only the largest one is kept.

## 6. Evaluation & Result

This is an example of how the pipeline performs on an image of ideal traffic signs:

![alt text](example-0.png)

According to this result, there are 2 miss-classifications and 2 missing classifications from 25 traffic signs. However, this does not represent the reality because it is a high-contrast image with low noise and white background.

And this is how it performs on real-life images:

![alt text](example-1.png)

![alt text](example-2.png)

As can be seen on these real-life examples, it does classify about 3/4 of the traffic signs that are in the first plain, however, not the ones that are in the background, because they are filtered out due to their size.

On the other hand, this method classifies the flags or any other rectangular, triangular or round shape that has strong red, blue or yellow colors as traffic signs, for instance, the flag in the second example.

### Conclusions

As can be seen on the examples, working based on colors and contrasts of images is not always a good idea because many other objects will be detected as traffic sign that are not one, because of their color.

Another factor that influences the precision and accuracy of the pipeline is the contact between 2 objects of similar colors that are detected, becuase they will be detected as one complex shape and will not be classified correctly. This could be solved using morphological operations, like _erosion_.

A better method of implementation would be an edge-based detection instead of color-based detection because colors would not influence it. However, computationally it would be heavier.

This implementation's slowest part is denoising the image that is not necessary if Canny edge detection is not used.

Considering all these, an option for improvement would be the Preprocessing part, so that the contrast of the image would help identifying the colors found on traffic signs.

## 7. Sources
[0] Github link to the project: https://github.com/pizzahunter2000/TrafficSignRecognition

[1] Algorithm using Convolution Neural Nets: https://www.analyticsvidhya.com/blog/2021/12/traffic-signs-recognition-using-cnn-and-keras-in-python/

[2] Comparison of ML solutions: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10223536/#:~:text=Traffic%20sign%20recognition%20can%20be%20divided%20into%20machine%20learning%20and,NN%2C%20and%20decision%20trees.

[3] Traditional Traffic Sign Segmentation pipeline: https://jq0112358.medium.com/traffic-sign-segmentation-with-classical-image-processing-methods-canny-edge-detection-color-8ff1096535db

[4] Canny edge detection: https://docs.opencv.org/4.x/da/d22/tutorial_py_canny.html

[5] Doubles-Peucker algorithm: https://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm