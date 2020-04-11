# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./test_images_output/original_image.png "Original Image"
[image1]: ./test_images_output/hls_image.png "HLS Color Space"
[image2]: ./test_images_output/color_mask_image.png "Yellow and White Color Mask"
[image3]: ./test_images_output/grey_image.png "Grayscale Image"
[image4]: ./test_images_output/blur_image.png "Gaussian Blur"
[image5]: ./test_images_output/canny_image.png "Canny Edges"
[image6]: ./test_images_output/masked_edge_image.png "ROI"
[image7]: ./test_images_output/hough_lines_image.png "Hough Lines Image"
[image8]: ./test_images_output/hough_lines_extrap_image.png "Hough Lines Extrapolated Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

In this project I created a simple lane finding pipeline to detect lane lines on the road. The pipeline consists of
the following steps:

1. Convert RGB image to HLS color space
2. Apply a yellow and white color mask
3. Convert image to grayscale
4. Apply Gaussian Blur to the grayscale image
5. Apply the Canny Edge Detection to the blurred grayscale image
6. Create a region of interest to remove edges not relevant to the lane lines
7. Apply Hough Transform to get a list of points for lines
    * Divide these points into left and right lanes lanes using slope (+/-)
    * Slopes outside the range of [0.5,1.5] (positive slopes) [-0.5,-1.5] (negative slopes) are not considered valid
    * Calculate the average position for the right and left lane line
8. Extrapolate lines to the top and bottom of ROI
9. Save image

Draw Lane Lines Modification:

* First calculate the slope of the hough line
* Then check if the slope is positive or negative.
    * Positive slope indicates the left lane line
    * Negative slope indicates the right lane line
* Lines with slope not in the range 0.5 to 1.5 (left lane) or -0.5 to -1.5 (right lane) are ignored
    * This will filter out slopes that are very close to 0 (horizontal lines) and
      other slopes that would not make sense for a lane line
* Next average the slope, x, and y points for the left lane and right lane
* Calcualte the intercept for both lanes using the average position and equation of a line (solve for b)
* Find the max and min for y, max is image height, min is the gloabl min of the averaged y points for the left and right lane line
* Find the start and end points in the region of interest by solving for x given the y_min and y_max for both lanes
* Draw final extrapolated lane lines on the original image

Below is a image for each step of the pipeline:

0. Original Image  ![alt text][image0]  
1. HLS Color Space  ![alt text][image1]  
2. Yellow and White Color Mask  ![alt text][image2]  
3. Grayscale Image  ![alt text][image3]  
4. Gaussian Blur  ![alt text][image4]  
5. Canny Edges  ![alt text][image5]  
6. ROI Mask applied to Canny Edges  ![alt text][image6]  
7. Hough Lines ![alt text][image7]  
8. Extrapolated Hough Lines  ![alt text][image8]  




### 2. Identify potential shortcomings with your current pipeline


There are some shortcomings with the current pipeline.

1. Pipeline doesn't work so well on curved lane lines
2. ROI is static and not optimized for each frame 
    * Going up or down steep grades, and curves will cause problems
3. There is no memory of lane line position from previous frames 
    * So if no lanes are detected it will be hard to predict where the lane lines should be if there is no info from previous frame
4. This pipeline is not going to generalize very well.


### 3. Suggest possible improvements to your pipeline

To improve this lane detection pipeline first I would address the region of interest section of the code. Currently this is hardcoded and not optimized for each frame.
An improvement could by dynamically choosing the region of interest for each frame. Another option would be to not implement a region of interest and come up with a 
robust algorithm that can remove edges that are not relevent to the road.

Another improvemnet that would improve this pipeline would be fitting a 2nd order polynomial to the lane lines and keep a history of lane line information from previous frames. This would 
greatly improve the performance for curved lane lines. 



