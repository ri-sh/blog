---
layout: post
title: "digit recognition in opencv and python"
date: 2015-05-09 11:33:00
thumbnail: /assets/img/migrated/digit-recognition-in-opencv-and-python/img0.png
---

_Originally posted on my old blog on 2015-05-09._

I got motivated to write a blog post on using HOG features and a multiclass Linear SVM which i recently learnt and just wanted to try some cool hands on these algorithms then i saw my bad handwriting because of which i suffered a lot in recent exams and wanted to apply something to recognize my handwriting . In the current blog post i am covering only how to recognize handwritten digits in subsequent post i will cover how to rcognize handwritten characters . 

Before we begin, I will succinctly enumerate the steps that are needed to detect handwritten digits -

  1. Create a database of handwritten digits.
  2. For each handwritten digit in the database, extract HOG features and train a Linear SVM.
  3. Use the classifier trained in step 2 to predict digits.



###  MNIST database of handwritten digits

The first step is to create a database of handwritten digits. We are not going to create a new database but we will use the popular **MNIST database of handwritten digits.** The MNIST database is a set of 70000 samples of handwritten digits where each sample consists of a grayscale image of size 28×28. There are a total of 70,000 samples. We will use sklearn.datasets package to download the MNIST database from [mldata.org](http://hanzratech.in/2015/02/24/mldata.org). This package makes it convenient to work with toy datasbases, you can check out the documentation of sklearn.datasets [here](http://scikit-learn.org/stable/datasets/).

The size of of MNIST database is about 55.4 MB. Once the database is downloaded, it will be cached locally in your hard drive. On my Linux system, by default it is cached in ~/scikit_learn_data/mldata/mnist-original.mat . Alternatively, you can also set the directory where the database will be downloaded.

  
  
[![One sample for each handwritten digit in MNSIT database](/assets/img/migrated/digit-recognition-in-opencv-and-python/img0.png)](http://hanzratech.in/figures/mnist-dataset.png)Figure 1: One sample for each handwritten digit in MNSIT database [[PNG](http://hanzratech.in/figures/mnist-dataset.png)]  


There are approximate 7000 samples for each digit. I actually calculated the number of samples for each digit using collections.Counter class. The actual samples for each digit was -

Digits| Number of samples  
---|---  
0| 6903  
1| 7877  
2| 6990  
3| 7141  
4| 6824  
5| 6313  
6| 6876  
7| 7293  
8| 6825  
9| 6958  
  
We will write 2 python scripts – one for training the classifier and the second for test the classifier.

###  Training a Classifier

Here, we will implement the following steps –

  1. Calculate the HOG features for each sample in the database.
  2. Train a multi-class linear SVM with the HOG features of each sample along with the corresponding label.
  3. Save the classifier in a file



The first step is to import the required modules –
    
    
    1 # Import the modules
    2 from sklearn.externals import joblib
    3 from sklearn import datasets
    4 from skimage.feature import hog
    5 from sklearn.svm import LinearSVC
    6 import numpy as np

We will use the `sklearn.externals.joblib` package to save the classifier in a file so that we can use the classifier again without performing training each time. Calculating HOG features for 70000 images is a costly operation, so we will save the classifier in a file and load it whenever we want to use it. As discussed above `sklearn.datasets` package will be used to download the MNIST database for handwritten digits. We will use`skimage.feature.hog` class to calculate the HOG features and `sklearn.svm.LinearSVC` class to perform prediction after training the classifier. We will store our HOG features and labels in numpy arrays. The next step is to download the dataset using the `sklearn.datasets.fetch_mldata` function. For the first time, it will take some time as 55.4 MB will be downloaded.
    
    
    1 dataset = datasets.fetch_mldata("MNIST Original")

Once, the dataset is downloaded we will save the images of the digits in a numpy array `features` and the corresponding labels i.e. the digit in another numpy array `labels` as shown below –
    
    
    1 features = np.array(dataset.data, 'int16') 
    2 labels = np.array(dataset.target, 'int')

Next, we calculate the HOG features for each image in the database and save them in another numpy array named hog_feature.
    
    
    17 list_hog_fd = []
    18 for feature in features:
    19     fd = hog(feature.reshape((28, 28)), orientations=9, pixels_per_cell=(14, 14), cells_per_block=(1, 1), visualise=False)
    20     list_hog_fd.append(fd)
    21 hog_features = np.array(list_hog_fd, 'float64')

In **line 17** we initialize an empty list `list_hog_fd`, where we append the HOG features for each sample in the database. So, in the for loop in **line 18** , we calculate the HOG features and append them to the list `list_hog_fd`. Finally, we create an numpy array `hog_features` containing the HOG features which will be used to train the classifier. This step will take some time, so be patient while this piece of code finishes.

To calculate the HOG features, we set the number of cells in each block equal to one and each individual cell is of size 14×14. Since our image is of size 28×28, we will have four blocks/cells of size 14×14 each. Also, we set the size of orientation vector equal to 9. So our HOG feature vector for each sample will be of size 4×9 = 36. We are not interesting in visualizing the HOG feature image, so we will set the visualise parameter to false.

If you don’t know about Histogram of Oriented Gaussians (HOG), don’t be disappointed because it is pretty easy to understand. You can check out the below 16 minute [YouTube video](http://www.youtube.com/watch?v=0Zib1YEE4LU) by Dr. Mubarak Shah from UCF CRCV. Alternatively, you can check out the documentation of the skimage’s hog function from the [official page](http://scikit-image.org/docs/dev/auto_examples/plot_hog.html). They do discuss tersely about how HOG works.

The next step is to create a Linear SVM object. Since there are 10 digits, we need a multi-class classifier. The Linear SVM that comes with sklearn can perform multi-class classification.
    
    
    26 clf = LinearSVC()

We preform the training using the fit member function of the `clf` object.
    
    
    29 clf.fit(hog_features, labels)

The `fit` function required 2 arguments –one an array of the HOG features of the handwritten digit that we calculated earlier and a corresponding array of labels. Each label value is from the set — `[0, 1, 2, 3,…, 8, 9]`. When the training finishes, we will save the classifier in a file named `digits_cls.pkl` as shown in the code below -
    
    
    32 joblib.dump(clf, "digits_cls.pkl", compress=3)

The compress parameter in the `joblib.dump` function is used to set how much compression is done and I am quoting this from the documentation –

> compress: integer for 0 to 9, optional
> 
> Optional compression level for the data. 0 is no compression. Higher means more compression, but also slower read and write times. Using a value of 3 is often a good compromise.

Up till this point, we have successfully completed the first task of preparing our classifier.

###  Testing the Classifier

Now, we will write another python script to test the classifier. The code for the second script is pretty easy and here is the code for the same –
    
    
     1 # Import the modules
     2 import cv2
     3 from sklearn.externals import joblib
     4 from skimage.feature import hog
     5 import numpy as np
     6 
     7 # Load the classifier
     8 clf = joblib.load("digits_cls.pkl")
     9 
    10 # Read the input image 
    11 im = cv2.imread("/home/droy/Desktop/photo8.jpg")
    12 
    13 # Convert to grayscale and apply Gaussian filtering
    14 im_gray = cv2.cvtColor(im, cv2.COLOR_BGR2GRAY)
    15 im_gray = cv2.GaussianBlur(im_gray, (5, 5), 0)
    16 
    17 # Threshold the image
    18 ret, im_th = cv2.threshold(im_gray, 90, 255, cv2.THRESH_BINARY_INV)
    19 
    20 # Find contours in the image
    21 ctrs, hier = cv2.findContours(im_th.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    22 
    23 # Get rectangles contains each contour
    24 rects = [cv2.boundingRect(ctr) for ctr in ctrs]
    25 
    26 # For each rectangular region, calculate HOG features and predict
    27 # the digit using Linear SVM.
    28 for rect in rects:
    29     # Draw the rectangles
    30     cv2.rectangle(im, (rect[0], rect[1]), (rect[0] + rect[2], rect[1] + rect[3]), (0, 255, 0), 3) 
    31     # Make the rectangular region around the digit
    32     leng = int(rect[3] * 1.6)
    33     pt1 = int(rect[1] + rect[3] // 2 - leng // 2)
    34     pt2 = int(rect[0] + rect[2] // 2 - leng // 2)
    35     roi = im_th[pt1:pt1+leng, pt2:pt2+leng]
    36     # Resize the image
    37     roi = cv2.resize(roi, (28, 28), interpolation=cv2.INTER_AREA)
    38     roi = cv2.dilate(roi, (3, 3))
    39     # Calculate the HOG features
    40     roi_hog_fd = hog(roi, orientations=9, pixels_per_cell=(14, 14), cells_per_block=(1, 1), visualise=False)
    41     nbr = clf.predict(np.array([roi_hog_fd], 'float64'))
    42     cv2.putText(im, str(int(nbr[0])), (rect[0], rect[1]),cv2.FONT_HERSHEY_DUPLEX, 2, (0, 255, 255), 3)
    43 
    44 cv2.imshow("Resulting Image with Rectangular ROIs", im)
    45 cv2.waitKey()

From **line 2-5** we load the required modules. In **line 8** , we load the classifier from the file **digits_cls.pkl __which we had saved in the previous script. In __line 11** , we load the test image and in **line 14** we convert it to a grayscale image using `cv2.cvtColor` function. We then apply a Gaussian filter in **line 15** to the grayscale image to remove noisy pixels. In **line 18** , we convert the grayscale image into a binary image using a threshold value of 90. All the pixel locations with grayscale values greater than 90 are set to 0 in the binary image and all the pixel locations with grayscale values less than 90 are set to 255 in the binary image. In **line 21** , we calculate the contours in the image and then in **line 24** we calculate the bounding box for each contour. From **line 28-35** for each bounding box, we generate a bounding square around each contour. Then in **line 37** , we then resize each bounding square to a size of 28×28 and dilate it in **line 38**. In **line 40** , we calculate the HOG features for each bounding square. Remember here that the HOG feature vector for each bounding square should be of the same size for which the classifier was trained, else you will get an error. In **line 41** , we predict the digit using our classifier. We also draw the bounding box and the predicted digit on the input image. Finally, in **line 44** we display the image.

I tested the classifier on this image -  
  


[![](/assets/img/migrated/digit-recognition-in-opencv-and-python/img1.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZMr5GJdneBC0fassse6yRiw-n6tBbSEN1js3pg-k82ZbrFX93-5AlHCoLeh8T_9l2Qehj1atYomWiicHPDITVXVQueYSPKszszQ-Lb2aAOFkYHGAZWnpQAPEqN6D3g9RDwPxRDOfSL4U/s1600/p.jpg)  
  
Figure 2: Input Image [JPG]  


The resulting image, after running the second script was -

[![](/assets/img/migrated/digit-recognition-in-opencv-and-python/img2.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg4-HOPSuIkcC1rMh5sxHIob3fnx5u-DDjKxwy7c7r40JWEpPB5DxafMSHQuJs6A_-Sf-ujf-8695RNoHArHvaEHfHk3Fe1lrPF3qboUmzRbZZnNGpxqjhpK2cUzwe_X8LynqY2Psqhwnk/s1600/pe.jpg)  
  
Figure 3: Resultant Image [PNG]  


So, the results are pretty good.

Here is another result -

[![](/assets/img/migrated/digit-recognition-in-opencv-and-python/img3.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjpJprGSMPEuXO8xhjo2nvXH26eF_5Bs4KU2xXW2r8NldApO9xSQzm8tMrJmQRTL_YDVLTWUXCduLcIIDN4538TiNr6Z3-M3tlZdSEAxa6YMitzIg7bj7DV_0X3Xl9k0NnrvGl247glpmo/s1600/pe.jpg)  
  
Figure 4: All the digits have been correctly recognized. [PNG]  


_(Above)_ In the image on the left hand side, we display the thresholded image with a square around each digit. Each of this square region is then resized to a 28×28 image. After resizing, we calculate the HOG features of this square region and then using these HOG features we predict the digit. In the image on the right hand side, we display the predicted digit for each handwritten sample bounded in the rectangular box.

###  Assumption during testing

There are a few assumptions, we have assumed in the testing images –

  1. The digits should be sufficiently apart from each other. Otherwise if the digits are too close, they will interfere in the square region around each digit. In this case, we will need to create a new square image and then we need to copy the contour in that square image.

  2. For the images which we used in testing, fixed thresholding worked pretty well. In most real world images, fixed thresholding does not produce good results. In this case, we need to use adaptive thresholding.

  3. In the pre-processing step, we only did Gaussian blurring. In most situations, on the binary image we will need to open and close the image to remove small noise pixels and fill small holes


-
