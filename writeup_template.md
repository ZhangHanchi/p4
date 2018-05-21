## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/undist2.jpg "Undistorted"
[image2]: ./output_images/test2.jpg "Road Transformed"
[image3]: ./output_images/thresholded_binary-test2.jpg "Binary Example"
[image4]: ./output_images/warped-test2.jpg "Warp Example"
[image5]: ./output_images/lane_lines.jpg "Fit Visual"
[image6]: ./output_images/example_output.jpg "Output"
[image7]: ./output_images/result_test2.jpg "Distored"
[video1]: ./project_result.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

At first，I set object points as replicated array of coordinates and collected image points as corners from chessboard images. Based on these, I then compute the camera calibration matrix and distortion coefficients with cv2.calibrateCamera()  function. With the help from these calculated parameters,  the distorted test images can be corrected using cv2.undistort. Following is test2.jpg and undistorted undist2.jpg.

![alt text][image7]
![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

At second, I used both color transforms and gradients to acquire a binary threshold image for detecting lanes.  On the one hand, pixels,  which have over threshold gradient values in both x and y directions, have been set to white; while other in black. On the other hand,  pixels with specific s values(in HLS model) and v values( in HSV model) have been collected. Combining these two results, the final binary threshold image can be created. Following is an example thresholded_binary_test2.jpg.


![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

At third,  the binary image is masked and warped to perform birds-eye view with clear lanes. The mask was designed to filter lines besides two lanes out, but it doesn’t work well. Because the warp function will also extract limited area of the image. By setting source points and destination points, binary image will be transformed to birds-eye view, which is convenient to calculated the radius of lanes. Following is an example warped_test2.jpg.


![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

At fourth, the lanes pixels will be picked up and be performed as polynomials. For first frame,  sliding windows have been used to search the whole area; for the other frames, the search area are only based on previous detected lanes. I use the lane pixels in two frames to smooth the changes of lanes and improve the stability. Following is an example lane_lines.jpg.

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

At fifth, the position of vehicle and  polynomials coefficients are calculated with np.polyfit().


#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

At last,  the area between two detected lanes are printed to green using cv2.fillPoly. Following is an example result_test2.jpg.

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_result.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
