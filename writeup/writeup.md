# **Finding Lane Lines on the Road** 

## Python implementation of vision-based lane line detection code

[//]: # (Image References)

[image1]: ../test_images_output/solidWhiteRight.jpg "solidWhiteRight"
[image2]: ../test_images_output/solidWhiteCurve.jpg "solidWhiteCurve"
[image3]: ../test_images_output/solidYellowLeft.jpg "solidYellowLeft"
[image4]: ../test_images_output/solidYellowCurve.jpg "solidYellowCurve"
[image5]: ../test_images_output/solidYellowCurve2.jpg "solidYellowCurve2"
[image6]: ../test_images_output/whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"
[image7]: ../test_images_output/capture.jpg "capture.jpg"

### Reflection

### 1. The Pipeline

The pipeline for detecting lane lines consists of 5 steps. They are:

1. Convert the given image to grayscale.
2. Apply Gaussian Blur.
3. Detect Canny edges.
4. Mask out regions outside of the ROI.
5. Detect Hough lines.

Helper functions are used in each of these steps. Parameters for steps 2, 3, 4 and 5 are the ones derived in previous quizzes. Run the code on Jupyter notebook and verified the result from each step in the pipeline by displaying the intermediate results using `matplotlib`.

### 2. Approximating Lane Lines

Now that we have line segments that are possibly edges of lane lines on the road, we want to collect those that are *likely* to be the actual lane line segments. The function `select_lines()` do this job by checking two aspects for each of the line segments:

1. Whether the segment's gradient belongs within the range given by the two parameters `min_angle` and `max_angle`. They are defined in degrees from the X-axis (in the direction of increasing X values), and converted to radians within the function, and
2. Whether the segement's location is the left (or right) half of the original image (or the ROI). This is checked by comparing the X coordinates of two endpoints of the segment with the left and right bounds of the intended region.

After selecting the candidates for left and right lane lines, we aggregate them to form a single (extended) line by computing the weighted average of (1) the slope (gradient) and (2) the (conceptual) y-intercept of the candidate segments. This is done by the function `aggregate_lines()`. Given a set of line segments, this function calculates the above mentioned weighted average to apply to the single line's slope and position. The results are given by its two endpoints, defined by (`x1`, `y1`) and (`x2`, `y2`).

### 3. Annotate the Image with Detected Lane Lines

First, a blank image with the same dimension is initialized. The (detected) left lane line is denoted by a thick red line, while the right one is given by blue. Next, superimpose this image onto the original image (taken by the camera) for (manual) verification purposes. Again, helper functions `draw_lines()` and `weighted_img()` are used to this end.

### 4. Tests on Still Images

The code is run against all 6 test images provided in the directory `test_images`.

* solidWhiteRight.jpg

![solidWhiteRight.jpg][image1]

* solidWhiteCurve.jpg

![solidWhiteCurve.jpg][image2]

* solidYellowLeft.jpg

![solidYellowLeft.jpg][image3]

* solidYellowCurve.jpg

![solidYellowCurve.jpg][image4]

* solidYellowCurve2.jpg

![solidYellowCurve2.jpg][image5]

* whiteCarLaneSwitch.jpg

![whiteCarLaneSwitch.jpg][image6]

Finally, an image has been extracted from (the first frame of) challenge.mp4 for testing. To get a reasonable result from this image, a set of adjustments had to be done, especially in setting the ROI within the given image. The perspective of this camera seems to be different from that for the other images.

* capture.jpg

![capture.jpg][image7]

### 5. Tests on Sample Videos


### 6. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
