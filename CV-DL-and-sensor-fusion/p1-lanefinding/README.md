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
[image5]: ./images/Output.JPG "Output"



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
![image5] 


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what happens when there is a dashed line very close to the camera. Due to perspective, this line appears to have a slope that is much closer to parallel than is actually correct focing the lane line to project into the middile of the road. This is evidenced by the lane line behavior under /images/solidYellowLeft (1).mp4


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have an addaptive search mask based on the lines that are present. For example, in my P1 notebook, if the left or right lane arrays had less than x# of inputs than the mask should be enlarged and hough lines should be recalculated.
