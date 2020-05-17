## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, my goal was to create a software pipeline to identify the lane boundaries in a video.


The Methodology
---

The steps I used to create a reliable lane finding pipeline are as follows:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images taken with the same camera that the lane lines will be filmed with.
* Apply distortion correction to raw frames of the video.
* Threshold the image in the saturation color space and the value of the x gradient independently. Overlay these two binary images to create a more dynamic and reliable binary image to work on.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary. Double check for accuracy using the cosine similarity between the current lane detection and previous detection to account for noise.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/original.jpg "Original"
[image2]: ./output_images/undistorted.jpg "Camera Distortion Correction"
[image3]: ./output_images/perspective_transform.jpg "Perspective Transform"
[image4]: ./output_images/gradient-threshold.jpg "Gradient Threshold"
[image5]: ./output_images/histogram.jpg "Histogram"
[image6]: ./output_images/find_pixels.jpg "Sliding Windows"
[image7]: ./output_images/pipeline-output.jpg "Pipeline Output"
[video1]: ./output_videos/project_video_output.mp4 "Final Video"


### The following serves as a more detailed review of my lane finding pipeline.   
---
### Raw image produced by on vehicle camera
![Original Image][image1]

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second code cell of the IPython notebook located in "p2-advanced-lanefinding/P2.ipynb". Specifically I created two functions: get_obj_and_img_points(images), undistort_image(img, objpoints, imgpoints).

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Undistored Image][image2]

### Pipeline (single images)

#### 2. Saturation color and Sobel-X gradients to create a thresholded binary image.  
I used a combination of HLS colorspace and gradient thresholds to generate a binary image. Specifically, I thresholded each frame if it achieved a certain saturation value. Similarly, I thresholded each frame by its Sobel X value to accentuate vertical lines. Here's an example of my output for this step. 

![Saturation and Gradient Thresholded][image4]

#### 3. Perspective transform

The code for my perspective transform includes a function called `warp_perspective()`.  The `warp_perspective()` function takes as inputs an image (`img`). This function returns a 'birds eye view' of the road in front of you based on source (`src`) and destination (`dst`) coorindates. I chose to hardcode the source and destination points based on the typical lane line location.

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 220, 720      | 300, 720      | 
| 515, 450      | 300, 0        |
| 707, 450      | 900, 0        |
| 980, 720      | 900, 720      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![Perspective Transformed][image3]

#### 4. Lane pixel identification and best fit line

I utilized the sliding window method to "scan" for the lane pixels as shown in the `find_lane_pixels()`. This function takes an image(`binary_warped`) which is a gradient/saturation thresholded image that has had its perspective transformed to the 'birds eye view' mentioned above. This `find_lane_pixels()` function starts by centering two different rectangles (left and right) above the starting point of each lane line. The starting point of the left and right lane lines was computed by taking the vertical sum of all the non zero pixels on the gradient and saturation image. This is shown in the histogram below. After this, windows are stacked vertically with their center being decided by the average of all non zero pixels in the last frame. Pixels are identified and added to an array in this way all the way to the top of the image. The function `find_lane_pixels()` returns arrays of left lane and right lane pixel coordinates.

![Histogram][image5]

These pixel coordinates are then fed into the `fit_poly()` funciton which fits a line to the left and right lane pixels independently. `fit_poly()` then evaluates this line of best fit at various y values to allow this line of best fit to be plotted.

![Sliding Windows and Best Fit Line][image6]

#### 5. Radius of curvature and distance from center metrics

Radius of curvature was identified by evaluating the radius of curvature function at the base of the image where the car is currently located. Note that the `fit_poly()` function must be revaluated in meters as oppossed to pixels which it was originally calculated for. This can be found in the `measure_curvature_pixels()` function 

Distance from center was completed by averaging the right and left lane line pixel locations for the bottom 1/3s of the image. These averages acted as the bounds for which the true center of the lane could be evaluated which was then compared to the center of the total image. I assumed the camera was centered on the vehicle. 



#### 6. Returning perspective and composing the output frame

Finally, once the lane lines have been identified and the metrics have been calculated, a lane can then be plotted onto the flat image using the left and right lane lines as bounds. From there the image can be transformed back to the cameras perspective by completing the perspective transform function mentioned in step 3 but switching the source (`src`) and destination (`dst`) coorindates. Once this image is returned to the cars POV the metrics in step 5 are layered on as well as the output seen by the sliding windows output.

![Pipeline Output][image7]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result][video1]

---

### Discussion

#### 1. Filtering out noisy inaccurate lines

The gradient and saturation thresholding is still not perfect and as a result, pixels that are not lane lines are left activated. These pixels lead to innacurate best fine lines being generated. As an attempt to stop these from taking place, I used the cosine similarity between the last best fit line and the current best fit line to weed out lines that are clearly wrong. The cosine similarity is the evaluation of similarity of two vectors in euclidean space.


