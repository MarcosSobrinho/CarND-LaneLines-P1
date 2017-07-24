# **Finding Lane Lines on the Road** 

## Writeup Template
---
**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Description of the pipeline

My pipeline consists of the following steps:

* Grayscaling the original image
* Gaussian blurring with a kernelsize of 5 times 5
* Canny edge detection with 70 as low treshold, and 175 as high treshold
* Hough transformation to detect the lines 
* Computing the slope and assigning the points of a line to either right or left side
* Calculating the average point and average slope for the right and left segment and thus finding the two average lines 
* Computing the intersection of the average lines with the bottom and (roughly) the middle of the picture
* Connecting the intersection points of the segments

##### Slopegroup function
After extracting the lines with the Hough Transformation the `slopeGroups()` function is used. Inside the function the slope `m = (y2-y1)/(x2-x1)` is calculated for every line. Then two cases are considered:
* If `0.4 < m < 0.8` is true, the slope `m` and the corresponding points `(x1, y1)`  and `(x2, y2)` are assigned to the right lane 
* If `-0.8 < m < -0.4` is true, the slope and the corresponding points `(x1, y1)`  and `(x2, y2)` are assigned to the left lane

After computing the average slopes `m_left`, `m_right` and average points `leftPoint`, `rightPoint` for the two groups, these values are used to calculate the starting and end point of each line. They are located at y = 540 and and y = 320. The start end end of the left and right side are returned by the function.

The image below shows the returned points. Due to visibility the bottom boints are shifted upwards along the line a little bit.

![][./test_images_output/new_solidWhiteCurve.jpg]

### 2. Potential shortcomings with the current pipeline
One shortcoming with the current pipeline is that it only computes detects straight lines correctly. In curves the algorithm will fail due to two reasons: The segmentation to left and right side already implies, that the lane lines are straight and not curved. Also the Hough transformation detects straight lines, so it cannot be used either for the detection of curved lines. 

### 3. Possible improvements to the pipeline
Up to the Canny filter the steps can remain the same. Maybe some adjustments concerning the region of interest have to be done. Then segments have to be found in a different way than with Hough. A possibility would be that the edge pixels have to be connected to each other to belong to a segment. Afterwards a polynomial fit can be applied to characterize the segments. Through the curvature of the polynoms the curvature of the street can be estimated and the segments can be devided to left and right lane. 

