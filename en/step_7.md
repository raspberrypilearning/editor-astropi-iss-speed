<h2 class="c-project-heading--task">Calculate feature distance</h2>

With the coordinates of matching features stored, the distance between the coordinates of the matching features can be calculated. This will be the distance on the images though, so it will need to be converted to the equivalent kilometer distance on Earth, and then divided by the time difference between the photos, to calculate the speed.

<h2 class="c-project-heading--explainer">Follow these instructions</h2>

## Step 1

Create a function to calculate the average distance between matching coordinates. Call it `calculate_mean_distance`. It takes two arguments, which will be the two coordinates lists.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 67
line_highlights: 67
---
def calculate_mean_distance(coordinates_1, coordinates_2):
--- /code ---
</div>

The Python `zip` function will take items from two lists and join them together. So the 0th item from the first list and the 0th item from the second list are combined together. Then the 1st items from each of the lists are combined together. The zipped list object can easily be converted back to a single simple list.

## Step 2

Start by creating a variable to store the sum of all the distances between coordinates and call it `all_distances`. Next, you can `zip` the two lists, and then convert the zipped object back to a list.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 67
line_highlights: 68-69
---
def calculate_mean_distance(coordinates_1, coordinates_2):
    all_distances = 0
    merged_coordinates = list(zip(coordinates_1, coordinates_2))
--- /code ---
</div>

To see what has happened here, you can add some `print` calls to see the details of the lists.

## Step 3

Add three `print` calls to see an item in `coordinates_1`, `coordinates_2`, and `merged_coordinates`.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 67
line_highlights: 70-72
---
def calculate_mean_distance(coordinates_1, coordinates_2):
    all_distances = 0
    merged_coordinates = list(zip(coordinates_1, coordinates_2))
    print(coordinates_1[0])
    print(coordinates_2[0])
    print(merged_coordinates[0])
--- /code ---
</div>

## Step 4

Add a call to your function at the bottom of your script.

<div class="c-project-code">
--- code ---
---
language: python
filename: 
line_numbers: true
line_number_start: 81
line_highlights: 
---
average_feature_distance = calculate_mean_distance(coordinates_1, coordinates_2)
--- /code ---
</div>

When you run the code, you should see output that looks like this:
<div class="c-project-output">
<pre>
(2006.4000244140625, 2119.2001953125) (2722.800048828125, 2156.400146484375)
(2006.4000244140625, 2119.2001953125)
(2722.800048828125, 2156.400146484375)
((2006.4000244140625, 2119.2001953125), (2722.800048828125, 2156.400146484375))
</pre>
</div>

Hopefully you can see that the `x` and `y` coordinate from each feature, from each image, have been combined. This will allow for easy iteration over the new list.

## Step 5

Delete the `print` calls and add a `for` loop to iterate over the `merged_coordinates` and calculate the differences between the `x` and `y` coordinates in each image.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 67
line_highlights: 70-72
---
def calculate_mean_distance(coordinates_1, coordinates_2):
    all_distances = 0
    merged_coordinates = list(zip(coordinates_1, coordinates_2))
    for coordinate in merged_coordinates:
        x_difference = coordinate[0][0] - coordinate[1][0]
        y_difference = coordinate[0][1] - coordinate[1][1]
--- /code ---
</div>

Look at the following image:

<div class="c-project-output">
![Triangle with points upper-case A, B, C and lengths lower-case a, b, c.](images/triangle.png)
</div>

The distance between points **A** and **B** is the length of the line **c**. This is called the hypotenuse. Using the `math` package, the hypotenuse (`hypot`) can be calculated.

## Step 6

Calculate the distance between the two points and add them to the `all_distances` variable.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 67
line_highlights: 73-74
---
def calculate_mean_distance(coordinates_1, coordinates_2):
    all_distances = 0
    merged_coordinates = list(zip(coordinates_1, coordinates_2))
    for coordinate in merged_coordinates:
        x_difference = coordinate[0][0] - coordinate[1][0]
        y_difference = coordinate[0][1] - coordinate[1][1]
        distance = math.hypot(x_difference, y_difference)
        all_distances = all_distances + distance
--- /code ---
</div>

## Step 7

Return the average distance between the features by dividing `all_distances` by the number of feature matches, which is the length of the `merged_coordinates` list.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 67
line_highlights: 75
---
def calculate_mean_distance(coordinates_1, coordinates_2):
    all_distances = 0
    merged_coordinates = list(zip(coordinates_1, coordinates_2))
    for coordinate in merged_coordinates:
        x_difference = coordinate[0][0] - coordinate[1][0]
        y_difference = coordinate[0][1] - coordinate[1][1]
        distance = math.hypot(x_difference, y_difference)
        all_distances = all_distances + distance
    return all_distances / len(merged_coordinates)
--- /code ---
</div>

## Step 8

Add a function call at the bottom of your code to calculate the average distance, and then print the result.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: false
line_number_start: 75
line_highlights: 81-83
---
time_difference = get_time_difference(image_1, image_2) # Get time difference between images
image_1_cv, image_2_cv = convert_to_cv(image_1, image_2) # Create OpenCV image objects
keypoints_1, keypoints_2, descriptors_1, descriptors_2 = calculate_features(image_1_cv, image_2_cv, 1000) # Get keypoints and descriptors
matches = calculate_matches(descriptors_1, descriptors_2) # Match descriptors
display_matches(image_1_cv, keypoints_1, image_2_cv, keypoints_2, matches) # Display matches
coordinates_1, coordinates_2 = find_matching_coordinates(keypoints_1, keypoints_2, matches)
average_feature_distance = calculate_mean_distance(coordinates_1, coordinates_2)
print(average_feature_distance)
--- /code ---
</div>

When you run your code, you should get an answer like `857.1072244913112'.

--- save ---

## Now run your code

Run your code and confirm that the average feature distance is printed.
