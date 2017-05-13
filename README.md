
**Vehicle Detection Project**

## The steps of this project are the following:

#### Training process

* feature extraction of the training image (Histogram of oriented gradience , Histogram of color , raw pixel values)
* scaling of the feature vector to remove a certain feature from dominating the entire prediction process 
* lableing of the feature vectors of hte car and non car images (1 for car and 0 for noncar)
* training a Linear SVC classifier using these feature vector

#### Detection process

* cropping of the test image into small windows based on scale size (pref various scale size)
* resize and feature extraction from the cropped windows (Note : the extracted feature should match the length of the feature vector used to train our model)
* run the prediction on all the windows taken from this single image and mark those windows where our classifier thinks it has a car image
* generate a heat map from those windows 
* create a bounding box based on the heat map 
* lable the car using a bounding box and return a video 

[//]: # (Image References)
[image1]: ./img/car_notcar.jpg
[image2]: ./img/hog.jpg
[image4]: ./img/pipeline.jpg
[image3]: ./img/searching_window.jpg


## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

At 6th block of my code , i defined a function to find the HOG of the image , along with HOG i also used raw pixel values and histogram of color also as a feature to detect vehicles 

At th 17th block of my code , i started importing the training image into my program . using the function extractFeatures , i started to extract the features (HOG , color hist , bin) and storing them as a single dimentional vectors


Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

i used skimage.feature.hog function to calculate the HOG of a image . my parameters where 

* orient = 9
* pix_per_cell = 8
* cell_per_block = 2

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters in trail and error method . finally i setteld for these parameters becouse these seems to work fine for me

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using HOG , hist of color feature  at 20th block of my code .After a lot of trail methods , i setteled with 100 as the C value for Linear SVM 


### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

At 16th block , i defined a function called find_cars where i implemented the window search methods . when comming to scale , i did trail and error method to find the scaling values and landed on these values . they provide me a good traadeoff between time to process the image and Detecting car percentage 

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

i used 7 different scaling factors to get a decent coverage of the image . this led me to higher time consumption when come to predict the image . here are some test iamges where i marked the bounding box in the image

![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a 


https://www.youtube.com/watch?v=GIzvkgVfaGs

#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:


![alt text][image4]


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

* cars that are appearing on the near end of both right and left side pose a hard time to detect the presence of car
* takes a lot of time for video generation . this is due to the usage of multiple window size
* few false positive detedtions appears in the final output even after the implimentations of filters

