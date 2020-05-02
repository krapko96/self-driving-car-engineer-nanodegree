# **Finding Lane Lines on the Road** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./images/grayscale.jpg "Grayscale"
[image2]: ./images/gaussian-blur.jpg "Grayscale"
[image3]: ./images/canny-edge.jpg "Grayscale"
[image4]: ./images/masked-image.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My process to identify lanelines worked as follows:
- convert the image to gray scale for eventual use with canny edge detection
![image1]
- apply gausian smoothing on the grayed out image to reduce noise and prevent sporadic edges from being found
- apply canny edge detection to find strong gradients indicative of 'edges' or important features of the image
- identify the piece of the image where lane lines typically lie in our camera and mask the surrounding area.
- compute hough line detection to sort the image for lines.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
