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

My pipeline consisted of 5 steps:
1. I converted the images to grayscale
2. I added Gaussian blur to smooth out some of the noise in the image,
3. I ran a Canny filter to detect strong edges in the image
4. I then tried to discard as many edges as possible that are not in the road way using a region centered in front of the car
5. I used hough lines to try and connect up the strong canny edges and discard smaller edges

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by expanding the function to
1. check the gradient of each hough line. All those leaning one way were left lines, and all those leaning the other way were right lines. Note I did add a check to eliminate lines where the gradient were extreme (close to horizontal) as they were likely irrelevant to my task.
2. iterate over all the left lines to get an average gradient for them, as well as compute the average position of each line (x and y), 
3. repeat step 2 for the right lines
4. once i have the average position and gradient, i now know y, m, and x for each line (left and right lines), i then calculate c for each line as in the formula y=mx+c (the equation of a line). Once i know this, i can draw any points on that line(s) i want.
5. Now i chose two x,y points for the left line and connect those, and two for the right line and connect those

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when too many shadows fall on the road, causing large amounts of erroneous canny edges and hough lines.

Another shortcoming could be if the lane markings dissappear, or the dashed lines are too far apart.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to eliminate more noise from the hough line detector. As part of draw image, once i have calculated the average gradient of the lines, I could run another pass to see which detected lines gradients are far from the average gradient.

Another potential improvement could be to test how far each detected hough line is from the other and use spacial filtering so that left lines that appear of the right are discarded, and right from the left. This would help stablize the tracking line. Also, i could add memory so that line position is remembered for some period of time if tracking is temporarily lost. Another imporovement would be to look at color or shade of the image under a detected line to see if it is a valid line, or if it could just be a shadow and ignored.
