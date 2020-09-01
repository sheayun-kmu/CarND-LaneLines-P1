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

The code is run against two sample input videos: solidWhiteRight.mp4 and solidYellowLeft.mp4. The former is a rather short clip of approx. 8 seconds, while the latter ran about 27 seconds. I did not notice any significant gap in terms of difficulties in finding lane lines in the images, though the guide document said something in that direction.

However, when the final (optional) video challenge.mp4 is given as input, first the detection did a really poor job in finding lane lines. Later, a potential bug was found in the code. While the dimension of this video was different from the former two, there was a couple of hard-coded coordinate calculations within functions. (While coding, I decided to put a *temporary* constant to them since I did not come up with an appropriate parameter passing scheme. I forgot afterwards while implementing other functions, saw reasonable results in a limited set of inputs, and the constants remained there - a very common mistake less trained programmers make.)

After a while of testing and debugging, the lane lines were detected in challenge.mp4 as well, but the result was not very satisfactory, with the drawn lines flickered here and there sometimes. Discussions follow in the next sections.

### 6. Identify potential shortcomings with your current pipeline

In real life, lane lines do not come from nowhere. A lane line is a continuous stretch of (sometimes straight, sometimes curvy) line intended to guide the driver. However, in processing the video stream, every frame is treated separately, causing a few glitches in finding the lines. A smooth approximation would be better, which is indeed how we use our senses (mostly vision) in human driving.

### 7. Suggest possible improvements to your pipeline

A very simple filter is expected to smooth out abrupt position changes of determined lines found here and there in the sample videos. This improvement should be necessary when we make driving decisions based on lane detection (possibly along with other sensor data).