# **Finding Lane Lines on the Road** 

## Writeup

### This README serves as the project writeup for P1

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./images/grayscale.jpg "Grayscale"
[image2]: ./images/gaussian-blur.jpg "blur"
[image3]: ./images/canny-edge.jpg "Canny Edge"
[image4]: ./images/masked-image.jpg "Masked-image"
[image5]: ./images/solidWhiteCurve.jpg 
[image6]: ./images/SolidWhiteRight.jpg 
[image7]: ./images/SolidYellowCurve.jpg 
[image8]: ./images/SolidYellowCurve2.jpg 
[image9]: ./images/SolidYellowLeft.jpg 
[image10]: ./images/whiteCarLeanSwitch.jpg 
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My process to identify lanelines worked as follows:
- convert the image to gray scale for eventual use with canny edge detection
![image1]
- apply gausian smoothing on the grayed out image to reduce noise and prevent sporadic edges from being found
![image2]
- apply canny edge detection to find strong gradients indicative of 'edges' or important features of the image
![image3]
- identify the piece of the image where lane lines typically lie in our camera and mask the surrounding area.
![image4]
- compute hough line detection to sort the image for lines based on intersections a user specificied intersection of points. User also inputs min line length and space between points to optimize line detection. The results from the 6 test images are shown below: 
![image5] ![image6] 
![image7] ![image8] 
![image9] ![image10] 


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
