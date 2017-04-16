# **Finding Lane Lines on the Road** 

## Writeup Template


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
- Make a pipeline that finds lane lines on the road
- Reflect on your work in a written report


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
The pipeline I have created takes as the image or a frame from a video input and processes it as follows.

For Instance , the input image in the pipeline is as embedded below.

![input_image] (https://github.com/rohanmaan/udacity-sdcnd-P1/blob/master/test_images/solidYellowCurve2.jpg)

The pipeline I created basically consisted of 7 steps:

1. I converted the images to grayscale using the open cv method :
```python
cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
```
![grayscale_image] (https://github.com/rohanmaan/udacity-sdcnd-P1/blob/master/output_images/Lane_Lines_Gray.jpeg)

2. Next, this grayscale image was smoothened using a gaussian filter.For this the following lines of code was used:
```python
return cv2.GaussianBlur(img, (kernel_size, kernel_size), 0)
```

![gauss_image] (https://github.com/rohanmaan/udacity-sdcnd-P1/blob/master/output_images/Lane_Lines_Gaussian_Blurred.jpeg)

3. Then the canny edge operator is used to detect edges in the smoothened image using :
```python
cv2.Canny(img, low_threshold, high_threshold)
```

![canny_edge] (https://github.com/rohanmaan/udacity-sdcnd-P1/blob/master/output_images/Lane_Lines_Canny_Edge.jpeg)

4. Now we mask a region of interest(ROI) which typically is the image section of shape of a trapezium arround the lane line pixels.The code for this is written in the function **region_of_interest()** of cell no 3 of the iPython Notebook **P1**.

5. Now all the lines in the ROI are stored in an array using the open cv method :
```python
cv2.HoughLinesP(img, rho, theta, threshold, np.array([]), minLineLength=min_line_len, maxLineGap=max_line_gap)
```

6. All the lines stored are then filtered using the slope filter technique in which horizontal lines and lines with slopes more than half are declared as outliers and eliminated. 

![region_masking] (https://github.com/rohanmaan/udacity-sdcnd-P1/blob/master/output_images/Lane_Lines_Region_masking.jpeg)

7. Now the mean of the accepted slopes is taken and the lane lines are drawn on the original image. This functionality can be found in the function **draw_lines()** of cell no 3 of the iPython Notebook **P1**. The result of this pipeline is  shown in the below image.

![final_img] (https://github.com/rohanmaan/udacity-sdcnd-P1/blob/master/output_images/Lane_Lines_detected.jpeg)

For the solid white right input video the output can be found [here](https://www.youtube.com/watch?v=vM7e6WpV9aQ).

For the solid yellow left input video the output can be found [here](https://www.youtube.com/watch?v=aLPrjcId7l0).



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lightening conditions on the road are not optimal.The algorithm will not be able to identify the lane line pixels under poor lightening conditions.

Also if we consider different weather conditions like snow , this algorithm will not be able to differentiate between lane line pixels and snow pixels.

If the lanes are too much curved then , the output of the hough transform would not be suitable for our pipeline.The mean slope line plotted will not be similar to the curve itself.



### 3. Suggest possible improvements to your pipeline

A possible improvement to this pipeline would be using other color formats like HSV and HLS instead of RGB. Thresholding these different channels might help in detecting lane pixels in different light conditions.

Also averaging the final slopes from previous frames in the video and then using it to plot lines in next frame , would help in avoiding the jittery motion of the drawn lane lines.


