

**Advanced Lane Finding Project**


---
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

[image1]: ./examples/undistort_output.png "Undistorted"

[image2]:  ./output_images/undistort_output.png "road"
[image3]: ./output_images/threshould.png "Binary Example"
[image4]: ./output_images/warped_straight_lines.png "Warp Example"
[image44]: ./output_images/histogram.png "histogram Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./output_video/advance_lane1.gif "Color 2"




## Overview 

---
 In the first project [Canny Lane Detection](https://github.com/ms802x/Lane-Detection-Project-1---canny) we had several issues in lanes detection. Issues such as : 
1. Lanes cannot be detected when the lighting conditions change. 
2.  when the lane is curvy the detection fails

In this project an improvments to the lane detection algorathem have been done to overcome these issues. The improvments are as follow: 

1. Removing image distoration 
2. Combining multiple thresholding methods rather than one. 
3. transform the prespective of the road image so the lines look parllel. 
4. Detect lane lines using histogram rather than the slope

by applying these improvments to the detection algorathem we get the following result:

![alt text][video1]



## Camera Calibration
---
 
Cameras use lenses to take pictures. The edge of the lense of the camera cause the light to bend which introduce  distortion to the image that need camera callibartion to get eleminated. The calibration is done by using a set of chessboard images with different angles. Then, by creating a list for object points and image points and we try to map these points to a chessboard to get their corners. Lastly, we use the object points and the corners we got to remove the distortion from any road image. 

![alt text][image1]

## Pipeline 

**Distortion Correction**

The first step is to undistort the image so we can get more accurate cordinates for the lane lines.
![alt text][image2]

**Threshoulding**

I have used gradient and HLS color threshoulding to get the the following binary image: 

![alt text][image3]

The astrix in the image are for the location where the "eye-bird view" will be done. The code is shown clearly in the jupyter notebook under "Use color transforms, gradients, etc., to create a thresholded binary image."

**Perspective Transform**

Now to apply the prespective transform I have warped the points in the previous binary image to its endpoints : 
 

```python
    src = np.float32( [[200, 720],[1100, 720],[595, 450], [685, 450]])
    
    dst = np.float32([[300, 720],[980, 720],[300, 0],[980, 0]])
```
this has resulted in the following transformed image:


![alt text][image4]

**Lane Detection**

Now, after we have changed the prespective all left is to detect the lanes. In detecting the lanes I have used the histogram. The histogram of the prepective transformed image is as follows:
![alt text][image44]

The histogram gives a valueable inforamtion about where most of the white pixels are accumlated which indicate a possible lane position. 

**Polynomial Fitting and Radius of Curvature**

Using the histogram data I have used sliding windows that slide by a value delta x and delta y. Using the generated widnows I have fitted a plynominal at the midpoint of each sliding window after determining which midpoint belongs to the left or right lane .  The radius of curvature is calculated after converting the pixels points to real world point by aplying a conversion fourmla :

1. ym_per_pix = 30/720 meters per pixel in y dimension
2. xm_per_pix = 3.7/700 meters per pixel in x dimension

Then by using the converted values the left and right curve and the radius has been calculated. 

![alt text][image5]

---

## video

 The video result is [here](output_video/project_video.mp4)



---

### Discussion

This algorathem suffer from several issues : 

1. if an object/viechle become infront of the car the detection will fail. 
2. if the road was highly shady the detection will fail also. 
3. if lane lines were diffcult to distinguis or faded the detection may fail also. 

Possible Improvments: 

1. Adding sensors to the detection not only depending on the camera 
2. Using deep learning algorathems to help in lane detection 
"# Project2-Advanced-Lane-Lines-Detection" 
