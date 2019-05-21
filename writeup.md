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

[image1]: ./output_images/1.png "Undistorted"
[image2]: ./output_images/2.png "Thresholded Binary Image"
[image3]: ./output_images/3.png "Perspective Transform"
[image4]: ./output_images/4.png "Lane Pixels Detection"
[image5]: ./output_images/5.png "Curvature Determination"
[image6]: ./output_images/6.png "Warp Back"
[image7]: ./output_images/7.png "Output"
[video1]: ./test_output_videos/project_video_result.mp4 "Result Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook name P2. 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function. 

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

I applied the distortion correction to the raw image using the `cv2.undistort()` function by passing the camera calibration and distortion coefficients obtained from the previous step and obtained this result: 

![alt text][image1]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used the color space HSL extracted the s channel and r channel from the same. I also utilized sobel and gradient thresholds to generate a binary image.  Here's an example of my output for this step.

![alt text][image2]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective()`. The function takes as inputs an image (`image`), as well as source (`src`) and destination (`dst`) points.

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image3]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I used sliding windows to detect pixels for the right and left lane, then fit those pixels with a polynomial. Sanity checks were also performed. I drew the detected windows on top of the perspective transformed image and it looked something like this:

![alt text][image4]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I performed curvature calculation and utilized the real world values to obtain the world space center and position of the car. I fitted a 2nd degree curve on the obtained values and the following was the result upto this step:

![alt text][image5]

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in my code and here is an example of my result on a test image:

![alt text][image6]


Finally I overlayed the drift from the center and the radii of curvature detials onto the lane detected image and got the final pipeline output on the test image as:

![alt text][image7]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a link to the final output video:

![alt text][video1]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

By adding a variety of color spaces, threshold and gradient detection, I was able to obtain a good output on the image and the video. Though as it can be observed from the video that during the shadow period, the video underperforms to a certain extent even though it is able to recover quite fast from the same. The challenge video was difficult too but overall a lot of lessons learnt!
