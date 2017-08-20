# CarND-Vehicle-Detection-P5
## Eva Liu


---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.


### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first code cell of the IPython notebook.  

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/data_img.JPG)

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

I implemented this step in the cell called: Histogram of Oriented Gradients (HOG). Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=8` and `cells_per_block=2`:


![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/hog.JPG)

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and got the best fit as shown above.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using Linear SVC model.

Here is the data analysis:

* Car samples:  8792
* Notcar samples:  8968
* Using: 8 orientations 8 pixels per cell and 2 cells per block
* Feature vector length: 2432
* 4.69 Seconds to train SVC...
* Test Accuracy of SVC =  0.9862

### Sliding Window Search

#### 1. Describe how you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to search random window positions at random scales all over the image and came up with this.


#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Sliding_win1.JPG)
![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Sliding_win2.JPG)
---

### Video Implementation

#### 1. Provide a link to your final video output. 

After the first sliding window search, I used find_car function to draw multiple windowns with different sizes in one image. And there is overlapping for these boxes.

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Multi_win1.JPG)
![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Multi_win2.JPG)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

##### Here are six frames and their corresponding heatmaps:

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Heat_map1.JPG)

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Heat_map2.JPG)

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Heat_map3.JPG)

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Heat-map.JPG)

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Heat_map5.JPG)

![alt text](https://github.com/evaliu1/CarND-Vehicle-Detection-P5/blob/master/Images/Heat_map6.JPG)



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

At the beginning I only called three times for the find_car function to add windows. But for the heat map part, it was not so accurate. So I added more function call to add windows. Then the heat map worked better.

