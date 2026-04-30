<h2 class="c-project-heading--task">Use Exif data to find a time difference</h2>

To collect Exif data from a photograph, you need a Python library called `exif`.

<h2 class="c-project-heading--explainer">Follow these instructions</h2>

### Exif data
<div class="c-project-callout c-project-callout--tip">

An **exchangeable image file format (Exif)** is a standard that sets formats for image and sound files, and various tags that can be stored within the file. These tags can include the time the photo was taken, the location it was taken at, and the type of camera that was used.

</div>

## Step 1

Open the Thonny Python IDE, and click on **Tools** > **Manage packages**, then search for and install the `exif` library.

[[[thonny-install-package]]]

## Step 2

In the main code editor, add the following two lines of code.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 
line_highlights: 1-2
---
from exif import Image
from datetime import datetime
--- /code ---
</div>

## Step 3

Save your file as `iss_speed.py`.

## Step 4

You will need some images taken from the Astro Pi unit on the ISS. You can download the photos by clicking on [this link](https://rpf.io/examplephoto){:target='_blank'}. Once the photos have been downloaded, you can right-click on the folder in your **Downloads** and unzip the folder. **Then move the photos to the same place that you have saved your python script.**

## Step 5

Beneath your `import` lines, create a function to find the time that a photo was taken. It will take one argument, which will be the photo's file name.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 
line_highlights: 5
---
from exif import Image
from datetime import datetime

def get_time(image):
--- /code ---
</div>

## Step 6

The image needs to be opened and then converted to an `Image` object, which is part of the `exif` package.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 
line_highlights: 6-7
---
from exif import Image
from datetime import datetime

def get_time(image):
    with open(image, 'rb') as image_file:
        img = Image(image_file)
--- /code ---
</div>

## Step 7

You can have a look at all the Exif data that is saved in the image file.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 
line_highlights: 8-9
---
from exif import Image
from datetime import datetime

def get_time(image):
    with open(image, 'rb') as image_file:
        img = Image(image_file)
        for data in img.list_all():
            print(data)
--- /code ---
</div>

## Step 8

You can test out your function using one of the image names that you have downloaded. This needs to be a string.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 
line_highlights: 12
---
from exif import Image
from datetime import datetime

def get_time(image):
    with open(image, 'rb') as image_file:
        img = Image(image_file)
        for data in img.list_all():
            print(data)

get_time('atlas_photo_012.jpg')
--- /code ---
</div>

## Step 9

Run your code, and you should see some output that looks like this:

<div class="c-project-output">
<pre>
make
model
software
datetime
_exif_ifd_pointer
_gps_ifd_pointer
exposure_time
photographic_sensitivity
datetime_original
gps_latitude_ref
gps_latitude
gps_longitude_ref
gps_longitude
</pre>
</div>

## Step 10

The data that is needed for this project is `datetime_original`. This can be saved as a string, and then it needs to be converted to a `datetime` object so that calculations can be performed on it.

<div class="c-project-code">
--- code ---
---
language: python
filename: iss_speed.py
line_numbers: true
line_number_start: 
line_highlights: 8-10, 13
---
from exif import Image
from datetime import datetime

def get_time(image):
    with open(image, 'rb') as image_file:
        img = Image(image_file)
        time_str = img.get("datetime_original")
        time = datetime.strptime(time_str, '%Y:%m:%d %H:%M:%S')
        return time

print(get_time('atlas_photo_012.jpg'))
--- /code ---
</div>

When you run this code, you should see output that looks like this:

<div class="c-project-output">
<pre>
2023-05-08 15:31:57
</pre>
</div>

--- save ---

## Now run your code

Run your code and confirm that you can print the `datetime_original` value from one of the ISS photos.
