#**Finding Lane Lines on the Road** 

---

### Reflection

###1. Pipeline Description.

My pipeline applies 5 modifications to each image.  First the image is converted to grayscale, then a gaussian blur is applied to reduce any "noise" in the lines of the image.  Canny edge detection is then applied to detect the edges in the image, with the intent of highlighting the road lines.  A region of interest is then selected, and is based on the dimensions of the image.  A Hough transform is then used to identify paramters for the road lines.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to first separate the slopes into positive and negative lists.  From these lists I averaged the slopes, then found the points with the highest and lowest y values for each average slope.  I knew that I wanted the highest y value to be at the bottom of the image because that is where the lane lines originate.  I also knew that I wanted the lowest y value of the lane lines to be bound by the region of interest.  With these y values, the average slope, and a coordinate from the Hough transform, I used the get_x_from_slope() function to determine the x value for the highest and lowest y point.  The left and right lane lines were drawn by using the average slope to calculate the high and low points that are bound by the region of interest.

#### Steps
[Step 1][/writeup_images/grayscale.jpg]

<img src="/writeup_images/grayscale.jpg" alt="Grayscale" />

In Step 1, the image is converted to grayscale in order to eliminate excess noise in the image that are not lane lines.

[Step 2][./writeup_images/gaussian.jpg]

In Step 2, a gaussian blur is applied to the grayscale image, in order to further eliminate noise in the more defined lines in the image.

[Step 3][./writeup_images/canny.jpg]

In Step 3, canny edge detection is applied to the image to highlight the edges in the image and further identify lane lines.

[Step 4a][./writeup_images/region0.jpg]

In Step 4a, the region of interest is shown.  This region is based on the height of the image, and assumes that the center of the lane is in the center of the image (in the x direction).

[Step 4b][./writeup_images/region.jpg]

In Step 4b, the region of interest is shown when applied to the image showing canny edge detection.

[Step 5][./writeup_images/hough.jpg]

In Step 5, a Hough transform is applied on the edge detected image.  Within the Hough function, is also the draw_lines() function which two lane lines each with a slope that has been averaged from various lines.

[Output][./writeup_images/overlay.jpg]

Here is the final output, which overlays the calculated average lane lines with the original image.



###2. Identify potential shortcomings with your current pipeline


One potential shortcoming of my pipeline would be if the horizon is not roughly near the middle y value of the image.  My pipeline is hardcoded to define the region of interest as starting ~60% from the top of the image, all the way to the bottom of the image.  If the horizon is located higher up on the image (lower y coordinates), then my pipeline would be ignoring a large portion of the lane line data.  My pipeline also assumes that the center of the lane is in the center of the image (x coordinate).  In the challenge video, the center of the lane is not in the center of the image.

Another shortcoming I noticed is the amount of jitter in the averaged drawn lane lines.  This is most likely due to only selecting only a singular point when considering lowest y value in my hough_lines() function.  To reduce or minize the jitter, it might be best to take an average of the lowest points within a specific threshold value.  Because these lines are drawn through extrapolation, averaging the starting  and ending points may help in reducing the jitter when viewing the video.


###3. Suggest possible improvements to your pipeline

To extend the shortcomings of the region of interest, when running my pipeline on the extra challenge video, it appears that there are other lines detected in the video that are influencing the lane line average drawing.  In this video it appears that the middle of the lane is not in the center of the image.  One improvement to my pipeline that might be beneficial, would be to develop a function that would roughly determine the location of the center of the lane.  Once the center of the lane is found, the region of interest could be adjusted (shifted right or left) in order to include more relevant data points.

Another improvement, which I mentioned previously, would be to average the high and low points of the each line in order to minized jitter when drawing the lane lines.