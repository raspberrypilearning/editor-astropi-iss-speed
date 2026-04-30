<h2 class="c-project-heading--task">Find matching coordinates</h2>

Now that the features that are the same on each image have been matched, the coordinates of those features need to be fetched.

<h2 class="c-project-heading--explainer">Follow these instructions</h2>

## Step 1

Create a new function that takes the two sets of keypoints and the list of matches as arguments.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 54
line_highlights: 54
---
def find_matching_coordinates(keypoints_1, keypoints_2, matches):
--- /code ---
</div>

## Step 2

Create two empty lists to store the coordinates of each matching feature in each of the images.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 54
line_highlights: 55-56
---
def find_matching_coordinates(keypoints_1, keypoints_2, matches):
    coordinates_1 = []
    coordinates_2 = []
--- /code ---
</div>

The list of matches contains many OpenCV `match` objects. You can iterate through the list to find the coordinates of each match on each image.

## Step 3

Add a `for` loop to fetch the coordinates (`x1`, `y1`, `x2`, `y2`) of each match.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 54
line_highlights: 57-61
---
def find_matching_coordinates(keypoints_1, keypoints_2, matches):
    coordinates_1 = []
    coordinates_2 = []
    for match in matches:
        image_1_idx = match.queryIdx
        image_2_idx = match.trainIdx
        (x1,y1) = keypoints_1[image_1_idx].pt
        (x2,y2) = keypoints_2[image_2_idx].pt
--- /code ---
</div>

## Step 4

Next, those coordinates can be added to the two coordinates lists, and the two lists can be returned.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 54
line_highlights: 62-64
---
def find_matching_coordinates(keypoints_1, keypoints_2, matches):
    coordinates_1 = []
    coordinates_2 = []
    for match in matches:
        image_1_idx = match.queryIdx
        image_2_idx = match.trainIdx
        (x1,y1) = keypoints_1[image_1_idx].pt
        (x2,y2) = keypoints_2[image_2_idx].pt
        coordinates_1.append((x1,y1))
        coordinates_2.append((x2,y2))
    return coordinates_1, coordinates_2
--- /code ---
</div>

## Step 5

Add a function call to the bottom of your script to store the outputs of the function. Add a line to print the first pair of coordinates from each list, and the run your program.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 67
line_highlights: 72-73
---
time_difference = get_time_difference(image_1, image_2) # Get time difference between images
image_1_cv, image_2_cv = convert_to_cv(image_1, image_2) # Create OpenCV image objects
keypoints_1, keypoints_2, descriptors_1, descriptors_2 = calculate_features(image_1_cv, image_2_cv, 1000) # Get keypoints and descriptors
matches = calculate_matches(descriptors_1, descriptors_2) # Match descriptors
display_matches(image_1_cv, keypoints_1, image_2_cv, keypoints_2, matches) # Display matches
coordinates_1, coordinates_2 = find_matching_coordinates(keypoints_1, keypoints_2, matches)
print(coordinates_1[0], coordinates_2[0])

--- /code ---
</div>

Your result should look something like this:
<div class="c-project-output">
<pre>
(2006.4000244140625, 2119.2001953125) (2722.800048828125, 2156.400146484375)
</pre>
</div>

--- save ---

## Now run your code

Run your code and confirm that the first pair of matching coordinates is printed.
