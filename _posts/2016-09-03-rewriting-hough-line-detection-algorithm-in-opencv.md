---
layout: post
title: "Rewriting Hough Line Detection Algorithm in OpenCV"
date: 2016-09-03 01:19:00
thumbnail: /assets/img/migrated/rewriting-hough-line-detection-algorithm-in-opencv/img0.svg
---

_Originally posted on my old blog on 2016-09-03._

The Hough Transform is a popular technique for detecting shapes that can be expressed in a mathematical form -- lines, circles, that sort of thing. One thing that makes it useful is that it still works even when the shape is broken up or a little distorted, which is exactly the kind of noisy edge you get out of a real edge detector.

For lines specifically, the algorithm works by voting. Every line can be described by two parameters, r and θ, in the equation:

![r=x\cos \theta +y\sin \theta](/assets/img/migrated/rewriting-hough-line-detection-algorithm-in-opencv/img0.svg)

So the algorithm builds a 2D array -- the "accumulator" -- with one axis for r and one for θ, at whatever resolution you quantize them to. Then, for every edge pixel (x, y) in the image, it works out which (r, θ) pairs a line through that pixel could have, and increments the accumulator bin for each one. A real line in the image means many edge pixels all "vote" for roughly the same (r, θ), so after processing every edge pixel, the bins with the highest counts -- the local maxima -- correspond to the lines actually present in the image.

Here's that as pseudocode:

```
Initialize h[d,theta]=0
for each edge pixel in E(x,y) in Image:
    for theta = 0 to 180
        h[d,theta] += 1

find values of (d,theta) where H(d,theta) is maximum
```

To try it out, I ran it on a simple test image full of basic shapes:

![shapes.png](/assets/img/migrated/rewriting-hough-line-detection-algorithm-in-opencv/img1.png)

And here's the result, with the detected lines drawn back onto the image:

![Houghlines](/assets/img/migrated/rewriting-hough-line-detection-algorithm-in-opencv/img2.png)

And here's the full implementation, written from scratch with NumPy rather than just calling `cv2.HoughLines` -- building the accumulator, finding the peaks in it, and drawing the resulting lines back onto the image:

```python
# imports
import numpy as np
import cv2
import matplotlib.pyplot as plt


# The Hough Transform is a popular algorithm for detecting any shape that can
# be represented in a parametric mathmatical form in binary images. This
# usually means that images need to be thresholded or filtered prior to running
# the Hough Transform.

# read in shapes image and convert to grayscale
shapes = cv2.imread('images/shapes.png')
cv2.imshow('Original Image', shapes)
cv2.waitKey(0)
cv2.destroyAllWindows()
shapes_grayscale = cv2.cvtColor(shapes, cv2.COLOR_RGB2GRAY)

# blur image (this will help clean up noise for Canny Edge Detection)
# see Chapter 2.0 for Guassian Blur or check OpenCV documentation
shapes_blurred = cv2.GaussianBlur(shapes_grayscale, (5, 5), 1.5)

# find Canny Edges and show resulting image
canny_edges = cv2.Canny(shapes_blurred, 100, 200)
cv2.imshow('Canny Edges', canny_edges)
cv2.waitKey(0)
cv2.destroyAllWindows()
########################################### HOUGH LINES FROM SCRATCH USING NUMPY
# Step 1: The Hough transform needs a binary edges images.  For this particular
# python file, I used the openCV built in Class Canny to create this edge image
# from the original shapes.png file.

# This is the function that will build the Hough Accumulator for the given image
def hough_lines_acc(img, rho_resolution=1, theta_resolution=1):
    ''' A function for creating a Hough Accumulator for lines in an image. '''
    height, width = img.shape # we need heigth and width to calculate the diag
    img_diagonal = np.ceil(np.sqrt(height**2 + width**2)) # a**2 + b**2 = c**2
    rhos = np.arange(-img_diagonal, img_diagonal + 1, rho_resolution)
    thetas = np.deg2rad(np.arange(-90, 90, theta_resolution))

    # create the empty Hough Accumulator with dimensions equal to the size of
    # rhos and thetas
    H = np.zeros((len(rhos), len(thetas)), dtype=np.uint64)
    y_idxs, x_idxs = np.nonzero(img) # find all edge (nonzero) pixel indexes

    for i in range(len(x_idxs)): # cycle through edge points
        x = x_idxs[i]
        y = y_idxs[i]

        for j in range(len(thetas)): # cycle through thetas and calc rho
            rho = int((x * np.cos(thetas[j]) +
                       y * np.sin(thetas[j])) + img_diagonal)
            H[rho, j] += 1

    return H, rhos, thetas


# This is a simple peaks function that just finds the indicies of the number
# of maximum values equal to num_peaks.  You have to be careful here though, if
# there's any noise in the image it will like create a 'pocket' of local maxima
# values.  This function ignores this and in turn has the tendancy to return
# multiple lines along an actual line in the image.
def hough_simple_peaks(H, num_peaks):
    ''' A function that returns the number of indicies = num_peaks of the
        accumulator array H that correspond to local maxima. '''
    indices =  np.argpartition(H.flatten(), -2)[-num_peaks:]
    return np.vstack(np.unravel_index(indices, H.shape)).T


# This more advance Hough peaks funciton has threshold and nhood_size arguments
# threshold will threshold the peak values to be above this value if supplied,
# where as nhood_size will surpress the surrounding pixels centered around
# the local maximum after that value has been assigned as a peak.  This will
# force the algorithm to look eslwhere after it's already selected a point from
# a 'pocket' of local maxima.
def hough_peaks(H, num_peaks, threshold=0, nhood_size=3):
    ''' A function that returns the indicies of the accumulator array H that
        correspond to a local maxima.  If threshold is active all values less
        than this value will be ignored, if neighborhood_size is greater than
        (1, 1) this number of indicies around the maximum will be surpessed. '''
    # loop through number of peaks to identify
    indicies = []
    H1 = np.copy(H)
    for i in range(num_peaks):
        idx = np.argmax(H1) # find argmax in flattened array
        H1_idx = np.unravel_index(idx, H1.shape) # remap to shape of H
        indicies.append(H1_idx)

        # surpess indicies in neighborhood
        idx_y, idx_x = H1_idx # first separate x, y indexes from argmax(H)
        # if idx_x is too close to the edges choose appropriate values
        if (idx_x - (nhood_size/2)) < 0: min_x = 0
        else: min_x = idx_x - (nhood_size/2)
        if ((idx_x + (nhood_size/2) + 1) > H.shape[1]): max_x = H.shape[1]
        else: max_x = idx_x + (nhood_size/2) + 1

        # if idx_y is too close to the edges choose appropriate values
        if (idx_y - (nhood_size/2)) < 0: min_y = 0
        else: min_y = idx_y - (nhood_size/2)
        if ((idx_y + (nhood_size/2) + 1) > H.shape[0]): max_y = H.shape[0]
        else: max_y = idx_y + (nhood_size/2) + 1

        # bound each index by the neighborhood size and set all values to 0
        for x in range(min_x, max_x):
            for y in range(min_y, max_y):
                # remove neighborhoods in H1
                H1[y, x] = 0

                # highlight peaks in original H
                if (x == min_x or x == (max_x - 1)):
                    H[y, x] = 255
                if (y == min_y or y == (max_y - 1)):
                    H[y, x] = 255

    # return the indicies and the original Hough space with selected points
    return indicies, H


# a simple funciton used to plot a Hough Accumulator
def plot_hough_acc(H, plot_title='Hough Accumulator Plot'):
    ''' A function that plot a Hough Space using Matplotlib. '''
    fig = plt.figure(figsize=(10, 10))
    fig.canvas.set_window_title(plot_title)

    plt.imshow(H, cmap='jet')

    plt.xlabel('Theta Direction'), plt.ylabel('Rho Direction')
    plt.tight_layout()
    plt.show()


# drawing the lines from the Hough Accumulatorlines using OpevCV cv2.line
def hough_lines_draw(img, indicies, rhos, thetas):
    ''' A function that takes indicies a rhos table and thetas table and draws
        lines on the input images that correspond to these values. '''
    for i in range(len(indicies)):
        # reverse engineer lines from rhos and thetas
        rho = rhos[indicies[i][0]]
        theta = thetas[indicies[i][1]]
        a = np.cos(theta)
        b = np.sin(theta)
        x0 = a*rho
        y0 = b*rho
        # these are then scaled so that the lines go off the edges of the image
        x1 = int(x0 + 1000*(-b))
        y1 = int(y0 + 1000*(a))
        x2 = int(x0 - 1000*(-b))
        y2 = int(y0 - 1000*(a))

        cv2.line(img, (x1, y1), (x2, y2), (0, 255, 0), 2)


# run hough_lines_accumulator on the shapes canny_edges image
H, rhos, thetas = hough_lines_acc(canny_edges)
indicies, H = hough_peaks(H, 3, nhood_size=11) # find peaks
plot_hough_acc(H) # plot hough space, brighter spots have higher votes
hough_lines_draw(shapes, indicies, rhos, thetas)

# Show image with manual Hough Transform Lines
cv2.imshow('Major Lines: Manual Hough Transform', shapes)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
