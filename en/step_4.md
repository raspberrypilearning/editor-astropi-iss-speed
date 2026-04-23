<h2 class="c-project-heading--task">Find matching features</h2>

The next step is to find matching features on the two images. There are algorithms to do this in OpenCV.

<h2 class="c-project-heading--explainer">Follow these instructions</h2>

## Step 1

Delete the `print` statement from your code.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 13
line_highlights: 20
---
def get_time_difference(image_1, image_2):
    time_1 = get_time(image_1)
    time_2 = get_time(image_2)
    time_difference = time_2 - time_1
    return time_difference.seconds

--- /code ---
</div>

## Step 2

Install the `opencv-python` package in Thonny.

[[[thonny-install-package]]]

## Step 3

Import the `cv2` package and the in-built `math` package at the top of your script.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 1
line_highlights: 3-4
---
from exif import Image
from datetime import datetime
import cv2
import math

--- /code ---
</div>

## Step 4

Delete your call to `print(get_time_difference('atlas_photo_012.jpg', 'atlas_photo_01.jpg'))` on line 22.

## Step 5

Images need to be converted to OpenCV objects so they can be processed, so add a function that takes the two images as arguments and then returns those objects.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 22
line_highlights: 22-25
---
def convert_to_cv(image_1, image_2):
    image_1_cv = cv2.imread(image_1, 0)
    image_2_cv = cv2.imread(image_2, 0)
    return image_1_cv, image_2_cv

--- /code ---
</div>

The OpenCV objects that have been returned can now be used by other classes and methods in the OpenCV package. For this project, the Oriented FAST and Rotated BRIEF (ORB) algorithm can be used. This algorithm will detect **keypoints** in an image or in several images. If the images are similar, then the same keypoints in each image should be detected, even if some features have moved or changed. ORB can also assign **Descriptors** to the keypoints. These will contain information about the keypoint, such as its position, size, rotation, and brightness. By comparing the descriptors between keypoints, the changes from one image to the other can be calculated.

## Step 6

Write a function to find the keypoints and descriptors for the two images. It will take three arguments: the first two are the OpenCV image objects, and the last is the maximum number of features you want to search for.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 28
line_highlights: 28-33
---
def calculate_features(image_1, image_2, feature_number):
    orb = cv2.ORB_create(nfeatures = feature_number)
    keypoints_1, descriptors_1 = orb.detectAndCompute(image_1_cv, None)
    keypoints_2, descriptors_2 = orb.detectAndCompute(image_2_cv, None)
    return keypoints_1, keypoints_2, descriptors_1, descriptors_2

--- /code ---
</div>

Now you have the keypoints and the descriptors of the keypoints, they need to be matched between the two images. This will tell you whether a keypoint in the first image is the same keypoint in the second image. The simplest way to do this is to use brute force.

### Brute force
<div class="c-project-callout c-project-callout--tip">

A **brute force** algorithm means the computer will try every possible combination. It's like trying to unlock a PIN-protected phone by starting with the PIN `0000`, then moving on to `0001` and keep going until it unlocks or you get to `9999`.

</div>

**Brute force**, in this context, means that you take a descriptor from the first image and try to match it against **all** the descriptors in the second image. A match will either be found or not. Then you take the second descriptor from the first image and repeat the process, and then you keep repeating this process until you have compared every descriptor in the first image to the ones from the second image.

## Step 7

Write a function that takes the two sets of descriptors and tries to find matches by brute force.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 35
line_highlights: 35-39
---
def calculate_matches(descriptors_1, descriptors_2):
    brute_force = cv2.BFMatcher(cv2.NORM_HAMMING, crossCheck=True)
    matches = brute_force.match(descriptors_1, descriptors_2)
    matches = sorted(matches, key=lambda x: x.distance)
    return matches

--- /code ---
</div>

Now that you have your matches, you can run all your functions and have a look at the output.

## Step 8

Assign the two images you want to use, and add function calls to the end of your script to run your functions and print out the matches.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 42
line_highlights:
---
image_1 = 'atlas_photo_012.jpg'
image_2 = 'atlas_photo_013.jpg'

time_difference = get_time_difference(image_1, image_2) # Get time difference between images
image_1_cv, image_2_cv = convert_to_cv(image_1, image_2) # Create OpenCV image objects
keypoints_1, keypoints_2, descriptors_1, descriptors_2 = calculate_features(image_1_cv, image_2_cv, 1000) # Get keypoints and descriptors
matches = calculate_matches(descriptors_1, descriptors_2) # Match descriptors
print(matches)

--- /code ---
</div>

The result should look something like this:

<div class="c-project-output">
<pre>
[< cv2.DMatch 0x11b34cb30>, < cv2.DMatch 0x11b2db8b0>, < cv2.DMatch 0x11b2dbef0>...
</pre>
</div>

This is a list of matches along with the keypoint. It's not very helpful though, so in the next step you can plot the matches on the images and view them.

--- save ---

## Now run your code

Run your code and confirm that you can print a list of feature matches for the two ISS photos.
