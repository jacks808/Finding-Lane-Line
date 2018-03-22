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

My pipeline include 6 steps. 
1. Use grayscale to process image, from RGB to gray scale
2. Use Gaussian smoothing to smoothing image. After this step, lot of tiny edge were filtered
3. Use Canny edge detection to find lines.
4. Use fillPoly to cut the image, And find where I need
5. Use hough transformer to find the line needed
6. Merge image of line to origin image

In order to draw a single line on the left and right, I modified the draw_lines() function by calculate the slope of each line.
split line into two types, which slope bigger than 0 or not. Then add all left line together to count a avg line, and do the same to right lines.

![](https://ws2.sinaimg.cn/large/006tNc79ly1fpktysuht2j30d5074js9.jpg)

the I get a avg line to represent each left and right line. But, sometimes ths avg line are shorter than real lane line, For fix this, 
I calculate the intercept By use: `b = y - slope * x` where slope is known, x\y is a point at this line. Then I plot new line from bottom of this image.
![](https://ws4.sinaimg.cn/large/006tNc79ly1fpku0ja5cjj30ji07e75k.jpg)


After image process, when I try to handle `challenge.mp4` video. I found there is a lot of noisy line 
e.g. Under the wall beside the rode, some line on the top of tree..., lot of them are horizontal to the X axis, 
then I drop the line which slope smaller than `0.5`, after doing this, the lane line looks better.
![](https://ws2.sinaimg.cn/large/006tNc79ly1fpku129v01j30d2070q3s.jpg)

Because of time is not to much, So I decided to commit my code. 
But I still have something want to do to improve accuracy:
* use region cut twice, first time to get the lane region, second time to cut result after candy edge detection.
* Drop line which is too short 


### 2. Identify potential shortcomings with your current pipeline

* The pipeline don't know the different of line, different lint means different single, solid line, dotted line, yellow line etc. 
* If there is some object in front of my car, the lane line detection pipeline will puzzled.
* This pipeline is based on 2D image, but, car is running in a 3D world.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use different hough line detection function to find different type of lines. such as color, length, etc.
Another potential improvement could be to use dual camera to record 3D video, and build a 3D lane line detection system.

