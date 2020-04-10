# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./test_images_output/original_image.jpg "Original Image"
[image1]: ./test_images_output/hls_image.jpg "HLS Color Space"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

In this project I created a simple lane finding pipeline to detect lane markings on the road. The pipeline consists of
the following steps:

1. Convert RGB image to HLS color space
2. Apply a yellow and white color mask
3. Convert image to grayscale
4. Apply Gaussian Blur to the grayscale image
5. Apply the Canny Edge Detection to the blurred grayscale image
6. Create a region of interest to remove edges outside ROI
7. Apply Hough Transform to get a list of points for lines
8. Divide these points into left and right lanes lanes using slope (+/-)
9. Slopes outside the range of [0.5,1.5] (positive slopes) [-0.5,-1.5] (negative slopes) are not considered valid
10. Calculate the average position for the right and left lane line
11. Extrapolate lines to the top and bottom of ROI
12. Save image

Below is a image for each step of the pipeline:

0. Original Image ![alt text][image0]
1. HLS Color Space ![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
