
## Overview

Advanced Lane Detection project includes computer vision algorithms to detect lane lines on the road. The code is written in Python and images or a video could be used as an input source. As a part of the Udacity Self-Driving Car Nanodegree Program, some of the code is leveraged from the lecture notes.

#### Dependencies
- Python 3.5
- Numpy
- OpenCV-Python
- Matplotlib
- Pickle

## Pipeline

#### 1. Camera Calibration
As a very first beginning step, the camera distortion values are extracted by analyzing several photos of a chessboard taken by the camera. 

![Calibration](output_images/cal_sample_chess.png?raw=true "Calibration sample 1")

#### 2. Undistorted Image
Images are then undistorted by the function `cv2.undistort` to reduce defects in upcoming analyzing functions that may lead unexpected results that are far from the real case. Otherwise, straight lines in real world may appear as curvatures on image. 

#### 3. Cropping Area
In order to better performance, all image analyzing functions for lane detection are applied on only a portion of the overall canvas. To distinct the lanes from the other parts of the image, also a polygonal area is cropped so that images consist only the lanes line.    

#### 4. White and Yellow Color Filter
The cropped image that has white and yellow lanes are filtered according to color channels. For detecting white, a simple filtering is used by checking all RGB channels of the image. However, for filtering yellow, the image is converted into HSL space so that from the hue value interval of yellow, it become easier to filter it. After detection of lane lines separately (which are separate binary images), they are combined into one binary image that shows both lanes only.

#### 5. Perspective Transform
Now then the polygonal area of the lanes and it's filtered version that shows only lane lines are both wrapped to be shown as a top-down view images. That images are great way to understand if the filtering and cropping is done correctly. 

#### 6. Identification of Lanes
Since there are white pixels that show lane lines, there are also some noise around to be filtered. Here the histogram is taken along all the columns in the lower half of the top-down image of the road. The peaks on histogram gives clue about the possible position of lanes. Then, starting from these points, sliding window technique is applied through the top of the image. 

#### 7. Curve Fit
The result of sliding window method consists detected points of lanes and a curve is fitted on them. Now there is a polynomial curve that represents the lanes instead of bunch of pixels. Since the camera is fixed at the center of the car, curve starting point's coordinate according to the center of image gives the car's position relative to the lanes. By using these curves, we can also predict about the radius of the curvature of the road.

#### 8. Display All
At the end, by resizing some of the processed images and combining them, a final image is created.



## Discussion

#### Project Content
This project is a great way of understanding a typical process of lane detection on road for self driving cars. It helped a lot to improve Python skills and learn about new libraries and related functions. It requires so much debugging and problem solving skills with various challenges on image processing. That makes this project an ideal way of gaining knowledge on self driving cars.

#### Improvement Suggestions
There are lots of way of improving the code. One of the improvement needed would be tools for deciding on filter values and parameters. Sliders for setting values of filtering functions and seeing the effect immediately on the image would make calibration process so easier and precise. The main disadvantage of the existing lane detection filter is that detection of yellow color may not be ideal for other cases to detect the lanes. Filters using sobel or gradients would end with better solutions for varying environments and conditions (like the challenge videos)
