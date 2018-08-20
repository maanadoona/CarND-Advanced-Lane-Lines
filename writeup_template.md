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

[image1]: ./output_images/calibration3_distorted.png "Undistorted"
[image1-1]: ./output_images/calibration3_distorted_warped.png "Warped"
[image1-2]: ./output_images/calibration3_corner.png "Corners"
[image2]: ./output_images/lane_distorted.png "Lane undistortion"
[image3]: ./output_images/lane_warped.png "Lane Warped"
[image4]: ./output_images/lane_combination.png "Combination for image"
[image5]: ./output_images/lane_combination_warped.png "Combination for warped"
[image6]: ./output_images/lane_gray.png "Gray"
[image7]: ./output_images/lane_rgb.png "RGB"
[image8]: ./output_images/lane_hls.png "HLS"
[image9]: ./output_images/lane_color_combi.png "HLS"
[image10]: ./output_images/lane_color_combi_warped.png "HLS"
[image11]: ./output_images/lane_window.png "Sliding"
[image12]: ./output_images/lane_window_prior.png "Sliding Prior"
[image13]: ./output_images/lane_curvature.png "Curvature"
[video1]: ./output_images/project_video_output.mp4 "project video output"
[video2]: ./output_images/challenge_video_output.mp4 "challenge video output"
[video3]: ./output_images/harder_challenge_video_output.mp4 "harder challenge video output"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./advanced_lane_find.ipynb". 


First, I opened the image files of chessboard in 'camera_cal' folder.
I convert the image to gray scale to find corner easily. I used 'cv2.findChessboardCorners' function to find chessboard corner. and I drawed circles by using 'cv2.drawChessboardCorners' function. 
![alt text][image1-2]

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 
![alt text][image1]

Then, I used cv2.getPerspectiveTransform and cv2.warpPerspective to get warped images like below.
![alt text][image1-1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

First of all, I'd like to know which transform is better than others for all test images.
So, I tested a lot and show them in jupyter notebook file.
These are undistorted and warped.
![alt text][image2] ![alt text][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I tested gradients of 'Sobel', 'Magnitude of Gradient' and 'Direction of the Gradients' for image and warped one. I think 'Sobel' is better than others.
![alt text][image4]
![alt text][image5]

I tested Gray, RBG and HLS which of each component.
From this images, Gray and R of RGB has good quality. but these are not good at bright or dark cases. I think S of HLS is the best in all around case.
![alt text][image6]
![alt text][image7]
![alt text][image8]

So, I combined Sobel and S of HLS and finally I got this result.
![alt text][image9]
![alt text][image10]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

When I got the warped image, I thought that the source and destination positions were important. I tried trapezoid to cover left and right lines at front. and I thought it is better to make trapezoid symmetrically.
I got the points of them like this.

src = np.float32([[580, 450],[700, 450],
                  [180, 720], [1100, 720]])
dst = np.float32([[180, 0], [1100, 0], 
                 [180, 720], [1100, 720]])

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 580, 450      | 180, 0        | 
| 700, 450      | 1100, 0      |
| 180, 720     | 180, 720      |
| 1100, 720      | 1100, 720       |


#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I got the left and right lane from histogram. while counting pixels through y-axis, the most stuffs in left and right side should be line.
![alt text][image11]
![alt text][image12]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I studied and tried to implement the curvature but it's hard thing to me. So, I copied them from jeremy's. link : https://github.com/jeremy-shannon/CarND-Advanced-Lane-Lines


#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

![alt text][image13]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

1. Project
![alt text][video1]

2. Challenge
![alt text][video2]

3. Harder Challenge
![alt text][video3]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I did by myself before curvature and I think they are also hard things to me. In fact, I think it's not enough to understand that class contents about curvature and displaying cover-range.
Anyway, I'd like to see whether my strategy works or not in video mode.
Thank you.

