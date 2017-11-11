# PIPELINE

1. Read the Initial Raw image

2. Reduce the noise of the image by using gaussian blur algorithm with ```kernel_size = 3```

3. Get the Edges using Canny Edge Detection algorithm.
    + In canny edge detection we define the high/low threshold. If a pixel from the very next pixel differs as much as provided high/low threshold we call it a an edge. As long as the difference between high/low is high enough we should get strong edges. The edges in the provided images and video had very well strong colored lane lines. Low threshold was ```canny_low = 50``` and High threshold ```canny_high = 150``` which is a difference=100 gave me a pretty good image.

4. Get Region of Interest.
    + The 2D image only has like 2 lane lines and conceptually speaking that's not even 10% of the whole image if we don't consider the space between the two lane lines. I discarded the part of the image that did not have the lane lines. I used best judgement and cut out the bottom part of the image where the lane lines were in all images/videos.

5. Get the Hough lines from the edges.
    + I used the Hough transformation Algorithm to get the line from the edges that I detected while using canny edge detection. It is an in-built cv2 built function so only the parameters passed in matter.
    + ```rho = 2``` to get the distance resolution in pixels of the Hough grid.
    + ```theta_coeff = 1``` used to calculate radians.
    + ```threshold = 50``` the number of intersections in a given grid cell a particular line needs to have.
    + ```min_line_len = 10```  the minimum length of the line in pixels.
    + ```max_line_gap = 20```  the maximum distance in pixels between broken segments that can constite a contiguous single line.
    + (Draw the Hough lines on the original image. Not a part of solution but this is the place where I have successfully overlapped the lane lines.) 

6. Extrapolate the lines.
    + we have broken line segments after the hough lines transformation as the 1 side (2 sides in real world) of the road in all the images has broken lane lines. We need to connect these broken segments.
    + Use the equation ```y = mx + c``` to get the slopes of all the lines on the image.
    + Since all the lines are quite vertical in nature we can cut out the horizontal ones. I used ```max_vertical_slope=0.5``` as threshold to cut out anything that has lesser `abs slope` than it.
    + Calculated the slopes and line sized using the formula ```length = √(x2-x1)2+(y2-y1)2``` and ```m = y2-y1/x2-x1```.
    + I then segregated the lines in positive and negative slopes. That way I know which one is right lane and which one is left lane.
    + I then sort the segregated lines based on line_sizes and then picked the biggest ones. The number of lines picked was ```extrapolation_n=5``` because I didn't see anything more than 4 in the images worth taking in so I took one extra.
    + Now we need two average slopes one for all the positive slope lines and one for negative slope lines. ```y = mx + c```
    + Once I had the slope then I could calculate the y-intercept ```c``` for the positive and negative using the above average values again  ```y = mx + c```.
    + Now I just need the line and so used image.shape to get the starting and ending x-coordinate and y-coordinate for positive and negative slope using the the above average values.

7. Draw the Extrapolated lines on the original image.


# REFLECTION
1. I learned after tweaking and playing with all the values on of my parameters that getting the vertices for region of interest when you have seen the image is relatively easier. I should have use the hough line transformation and gotten the lines and based off of that I should have figured where the lines were and get that portion of the image instead of the hack I did.
2. All the image analysis only accounts for suitable/ideal conditions. all the images had 1 solid line and no turns and the images were not very fast moving so the detection was made simple with all the helper functions that were listed out line by line.
3. Extrapolating the lines was not easy to do on a computer as it was on paper because I was ending up with NaN's a lot. 
4. This is not a good solution in the ideal world.
5. My lane lines in the challenge go awry for a couple of seconds in the video and I would like to have fixed that.
6. Shadows are not good on videos of roads.
. Drawing lines on the video was tiring and fun.
5. I should have read up on what those parameters on Hough transformation even do.  
6. I would like to have done this from scratch without the helper functions.
7. I should start my next assignment early on.

# POTENTIAL SHORTCOMINGS
1. It would not work in the night/dark or in shadows.
2. It would not work if the lanes are not very prominently visible.
3. The region of interest vertices are a hack and not scalable.
4. It won't work if the driver in front of us is moving lanes and blocks line-of-sight on the lane lines.
5. It would not work in any crowded city at all where the roads are narrow and there are too many edges. 
6. This is not a robust system. We are guessing that the the lines are ahead are lane lines. It's just not scalable.

# POSSIBLE IMPROVEMENTS
1. Redoing the vertices calculation for region of interest.  I should have uses the hough line transformation first and gotten the lines and based off of that I should have figured where the lines started and ended and then get that portion only and start extrapolating on that.
2. We processed each image as it came and didn't look at the previous image to check how much had changed and how to react based off of that change.
3. A validation check to figure out what's a lane and what's not.
4. My lane lines shake a lot they should be a bit more robust.

