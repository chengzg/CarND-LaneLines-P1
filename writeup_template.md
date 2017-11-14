**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 4 steps.

    1. Filter the image with red_threshold and gree_threshold, as we only consider the White/Yellow lines
    2. Gaussian Blur and Canny Edge Detection
    3. Define the region of interest to filter out not interested region
    4. Use hough line algorithm to get the lines which has several sub steps
        4.1 Get the detected the hough lines
        4.2 Separate the lines into left lines and right lines
        4.3 Compute the average line for both left lines and right lines
        4.4 Smooth the average line
        4.5 Project the left/right boundary line points on the left/right average line to extend the line
        4.6 draw the project left/right lane onto the image


In order to draw a single line on the left and right lanes, I modified the hough_lines() function mentioned in step 4 above by:

    4.1 Get the detected the hough lines
    4.2 Separate the lines into left lines and right lines using the middle line of the region of interested 
    4.3 Compute the average line for both left lines and right lines. Classify the points for each points into two cluster and Computer the average point for each cluster named lowPoint and upPoint. Use the two points to form the average line. If any of the average points is happen to be empty, use the point from last frame
    4.4 Smooth the average line use the average value computed with the current and last 5 frames
    4.5 Project the left/right boundary line points(defined by the region of interested) on the smoothed left/right average line to extend the line
    4.5 draw the project left/right lane onto the image


### 2. Identify potential shortcomings with your current pipeline


The potential problems could be:

    1. the region of interested calculation is not smart enough, currently it is equivalent to hard coded values
    2. The color filtering assumes lane color would be whilte or yellow(both threshold values are above 180) 
    3. the way to compute the average left/right lane can not filter out the deteced noisy lines which have slope between 30 and 150 degree. And the 30 and 150 degree are hard codes values
    


### 3. Suggest possible improvements to your pipeline

Possible improvements would be to ...

    1. start from 30 and 150 degree, based on the calculated slope value for each frame, we can adjust these two value to get closer to the acutally lane slope. So that we can filter out more detected noisy lines 
    2. Similarly, based on the left/right average line found for each frame, we can refine the region of interested to get closer to the actual lanes so that we furthur reduce the deteced noise lines
