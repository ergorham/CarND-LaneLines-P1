# **Finding Lane Lines on the Road** 

## Project 1 Writeup

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./doc/solidWhiteCurve_bad.jpg "Classic Hough Lines Averaged"
[image2]: ./test_images_output/solidWhiteRight.jpg
"Smoothed, extrapolated lines"

---

### Reflection

### 1. Description of my Pipeline.

The first iteration of my pipeline exclusively used the helper functions to process the images. I later added my own helper functions to help reuse certain functions. I ended up unpacking the draw_lines() function from the hough_lines() function. In all, my pipeline consists of the following n steps:
* Convert to grayscale
* Apply Gaussian blur
* Apply Canny edge detection
* Extract Hough lines from region of interest
* Segregate lines according to left/right of ROI
* Convert Hough lines to slope/intercept and average these, respectively
* Extrapolate averaged left and right lines to ROI limits
* Draw lines on image.

I had attempted using the classic, as opposed to the probabilistic, version of the OpenCV Hough lines transform in the hopes of easily segregating the lines by phase angle. This proved too tedious in terms of filtering unwanted edges and would only give good results in few cases.

![alt text][image1]

In the end, I used a simple binning approach, comparing the x-values of the Hough lines to the midpoint of my ROI to determine left or right. I opted for a simple inter-frame smoothing method by keeping the slope of the last frame and using it as the mean with which to apply MSE for rejecting "noisy" lines. The results were acceptable after some tuning.

![alt text][image2]


### 2. Limitations of the current pipeline


The pipeline does not perform well on lanes with a high curvature, as it attempts to fit a straight line to the left and right lane.

Based on the stock video, the pipeline performs reasonably well with high contrast, as in the day time, but it probably would not perform as well in situations of low contrast where the line lanes are not perfectly illuminated.


## 3. Future improvements to the pipeline

Currently, the pipeline uses the previous slope as a mean with which to smooth the slope averaging. A possible improvement would be to have a moving average to get better smoothing results.

Another potential improvement could be to use piece-wise smoothing to take into account lanes with high curvature.
