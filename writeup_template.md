**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/hog-rgb.png
[image2]: ./output_images/hog-hsv.png
[image3]: ./output_images/hog-luv.png
[image4]: ./output_images/hog-hls.png
[image5]: ./output_images/hog-yuv.png
[image6]: ./output_images/hog-YCrCb.png
[image7]: ./output_images/example_car_found.png
[image8]: ./output_images/example_car_found_heatmap.png
[image9]: ./output_images/example_car_found_heatmap_singleboundingbox.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
 

---
Writeup / README

1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.

    You're reading it!

## Histogram of Oriented Gradients (HOG)

1. Explain how (and identify where in your code) you extracted HOG features from the training images.

    The code for this step is contained in the thrid code cell of the IPython notebook.  

    I started by reading in all the `vehicle` and `non-vehicle` images.
    I then explored different color spaces and different `single_img_features` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). I grabbed random images from each of the two classes and displayed them to get a feel for what the `single_img_features` output looks like.
 
 
    Here is an example using different color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:
 
 
    RGB color space 
 
    ![alt text][image1]
 
 
    HSV color space 
 
    ![alt text][image2]
 
 
    LUV color space 
 
    ![alt text][image3]
 
 
    HLS color space 
 
    ![alt text][image4]
 
 
    YUV color space 
 
    ![alt text][image5]
 
 
    YCrCb color space 
 
    ![alt text][image6]

2. Explain how you settled on your final choice of HOG parameters.

    I tried various combinations of parameters. Below is the final choice of HOG paratmers
```
 color_space = 'YCrCb' 
 orient = 9  # HOG orientations 
 pix_per_cell = 8 # HOG pixels per cell 
 cell_per_block = 2 # HOG cells per block 
 hog_channel = 'ALL' # Can be 0, 1, 2, or "ALL" 
 spatial_size = (32, 32) # Spatial binning dimensions 
 hist_bins = 32    # Number of histogram bins 
 spatial_feat = True # Spatial features on or off 
 hist_feat = True # Histogram features on or off 
 hog_feat = True # HOG features on or off
 
```

3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

   The code for this step is contained in the fifth code cell of the IPython notebook.
   I trained a linear SVM using the HOG features and got an accuracy ~99%.

## Sliding Window Search

1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?
  
     I am using the slide_window function which is that of the thrid code cell of the IPython notebook.
     I am using the y_start_stop = [400,656] overlap = 0.5. 

2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

    Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

    ![alt text][image7]
---

### Video Implementation

1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

    Here's a [link to my video result](./project_output.mp4)


2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

    I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

    Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps before thresholding and labeling boxes:


![alt text][image8]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:


![alt text][image9]

---

## Discussion

1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

