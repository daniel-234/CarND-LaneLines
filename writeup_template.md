# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. First, I converted the images to grayscale, then I applied Gaussian Smoothing to the grayed image. 
The following step was to define a low and high threshold and find edges by applying Canny Edge Detection. 
The fourth step was to define four vertices of a region of interest and find a masking region. 
The fifth step was to define the Hough transform parameters and draw Hough lines. 
The last step was to find the weighted image. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by computing the slope for each Hough line (identified by 2 points) and filter for slopes that were not steep enough, to exclude possible horizontal lines. In fact we are only interested in lines that develop in vertical direction, converging towards the horizon from a car's perspective. 
We know that the y coordinates are at the bottom of the image, near the car, and at a position near the horizon that is similar for all images. So we need to find the x positions for the starting and ending point of the lines. 
As using the slope to compute the x values gave me wobbly lines, I computed two polynomials. Each x and y coordinate for both the left and right line were then inserted into a list from which a polynomial was extracted. The polynomial x and y coordinates were reversed to solve the polynomial for the x unknown values. 
Finally, two lines were drawn using the starting and ending points for each line. 

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen if I used an image with some variations on its dimensions or when the lines would only extend for a smaller part of the image and maybe not even started from the image bottom edge, like what happens with the optional challenge video. 

Another shortcoming could be having an image where parts of the lines are canceled or not visible. I experienced it in one of the videos, where I had to slightly adjust the upper vertices of the masking polygon to have a good result. 
In fact, in case any video frames didn't detect a line on either side, there would be empty arrays for some coordinates, which the system wouldn't be able to deal with. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to adjust coordinates in the pipeline to make it work in case lines change in height position. 

For the second case maybe there could be more functions that, in case one of the lines were missing, could deal with the problem by following the line still present and use some calculation to compute the other one, maybe with some stored average information. 
For this solution, however, a lot of more (advanced) checks would be necessary to take into account safety. 
