## Challenge Name: 🤪 Welcome
Author: zeyu2001  
Category: Misc  
Points: 392  
Solves: 27  
Tags: Insanity Check  
<br>
>Welcome to SEETF! Submit the teaser video flag here.<br><br>
https://www.youtube.com/watch?v=0GVC30jiwJs<br><br>
Download: https://drive.google.com/file/d/1Dlfp6vC1pto7vW5WUKqvX-tbIt6tfGfg/view

MD5: 704f59b279476f16e4fcb17b20d23c06

### Approach
Watching the [video](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/misc/%F0%9F%A4%AA%20Welcome/files/Trailer_With_Challenge.mp4), besides finding it really cool, we can find see that there are white dots appearing on the top right hand of the video. Taking a closer look, we see that the noise starts at about 0:25. Looking at the way the white dots are concentrated in one corner, one guess that we can take is that this white dots might mean something when put together. Something like a QR code maybe?

So now, how do we extract the white dots? Firstly, we can extract the relevant frames of the video that contain the white dots using Python and OpenCV.

By checking out the properties of the video, we realise that the video is video is 60 frames per second. Since the white dots only start at 0:25, it means the first 25*60=1500 frames are useless to us. 

Furthermore, since the white dots only appear on the top right of the video, we can choose to only look at the top right part of the relevant frames, i.e. the top 201\*201 pixels. 

Since the video's width is 1920 and height is 1080, to look at the top right 201\*201 pixels, we can set the region of interest to be in width, 1719 to 1920 and in height, 0 to 201. We can put this into code now!

```py 
import cv2 as cv

cap = cv.VideoCapture('Trailer_With_Challenge.mp4')
cap.set(cv.CAP_PROP_POS_FRAMES, 1500)
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print("Can't receive frame (stream end?). Exiting...")
        break
    regionOfInterest = frame[0:201,1719:1920]
    cv.imshow('frame', regionOfInterest)
    if cv.waitKey(1) == ord('q'):
        break

cap.release()
cv.destroyAllWindows()
```

Next, let's extract a random frame to have a closer look at the white dot. 
```py
import cv2 as cv

cap = cv.VideoCapture('Trailer_With_Challenge.mp4')
cap.set(cv.CAP_PROP_POS_FRAMES, 1500)
frameExtracted = False
count = 1500
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print("Can't receive frame (stream end?). Exiting...")
        break
    regionOfInterest = frame[0:201, 1719:1920]
    if frameExtracted == False and count == 1800:
        cv.imwrite('frame.jpg', regionOfInterest)
        frameExtracted = True
    count += 1
cap.release()
cv.destroyAllWindows()
```
This will give us:  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/misc/%F0%9F%A4%AA%20Welcome/files/frame.jpg "Image")

Putting the extracted image into a [pixspy](https://pixspy.com/), an online image inspection tool, we can see that the white dot is actually composed of 3*3 pixels in a square.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/misc/%F0%9F%A4%AA%20Welcome/files/pixspy.png "Image")

We can turn this into 1 pixel by compressing the width and height in a 3:1 ratio each, thus getting an image of size 67\*67. This can be achieved using the following code:
```py
import cv2 as cv
resized = cv.resize(regionOfInterest, (67, 67),
                         interpolation=cv.INTER_AREA)
```

Now, all we have to do is to extract the white dots from each frame and paste them onto a black background.

For extracting the white dot, we can just iterate through each pixel in each frame and check if its quite bright and white. Our criteria for that can be that it's RGB value are each 200 or higher. After having identified the pixel, we can save its coordinates in a list.
```py
import cv2 as cv

whiteDotCoordinates = []

# Iterating through frames...

    for widthIterator in range(0, 67):
        for heightIterator in range(0, 67):
            if resized[heightIterator][widthIterator][0] >= 200 and resized[heightIterator][widthIterator][1] >= 200 and resized[heightIterator][widthIterator][2] >= 200:
                whiteDotCoordinates.append([widthIterator, heightIterator])
```

Now, we can just generate a black background and turn the pixels at these coordinates into white dots.
```py
import numpy as np

black = np.zeros([67,67,3],dtype=np.uint8)
# creates a black image of size 67*67

for i in whiteDotCoordinates:
    black[i[1]][i[0]] = [255, 255, 255]
    # gets a set of coordinates and turns the pixel there white

cv.imwrite("QR.jpg", black)
```

Putting all of this together, we can get a full solve script for the challenge.
```py
import numpy as np
import cv2 as cv

cap = cv.VideoCapture('Trailer_With_Challenge.mp4')
cap.set(cv.CAP_PROP_POS_FRAMES, 1500)
frameExtracted = False
whiteDotCoordinates = []
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print("Can't receive frame (stream end?). Exiting...")
        break
    regionOfInterest = frame[0:201, 1719:1920]
    resized = cv.resize(regionOfInterest, (67, 67),
                        interpolation=cv.INTER_AREA)
    for widthIterator in range(0, 67):
        for heightIterator in range(0, 67):
            if resized[heightIterator][widthIterator][0] >= 200 and resized[heightIterator][widthIterator][1] >= 200 and resized[heightIterator][widthIterator][2] >= 200:
                whiteDotCoordinates.append([widthIterator, heightIterator])

black = np.zeros([67, 67, 3], dtype=np.uint8)

for i in whiteDotCoordinates:
    black[i[1]][i[0]] = [255, 255, 255]

cv.imwrite("QR.jpg", black)

cap.release()
cv.destroyAllWindows()
```

Running it, we will get the QR code exported into the same folder as the script itself.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/misc/%F0%9F%A4%AA%20Welcome/files/QR.jpg "Image")

Scanning the QR code using [a random online QR code reader](https://4qrcode.com/scan-qr-code.php), we get the flag to be `SEE{W3lc0m3_t0_SEETF_95c42d3be1cb93cce8241235529ad96f8e0e1c12}`. Hooray!  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/misc/%F0%9F%A4%AA%20Welcome/files/QR%20Reader.png "Image")

### Flag
`SEE{W3lc0m3_t0_SEETF_95c42d3be1cb93cce8241235529ad96f8e0e1c12}
`


### Full Solve Script
```py
import numpy as np
import cv2 as cv

cap = cv.VideoCapture('Trailer_With_Challenge.mp4')
cap.set(cv.CAP_PROP_POS_FRAMES, 1500)
frameExtracted = False
whiteDotCoordinates = []
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print("Can't receive frame (stream end?). Exiting...")
        break
    regionOfInterest = frame[0:201, 1719:1920]
    resized = cv.resize(regionOfInterest, (67, 67),
                        interpolation=cv.INTER_AREA)
    for widthIterator in range(0, 67):
        for heightIterator in range(0, 67):
            if resized[heightIterator][widthIterator][0] >= 200 and resized[heightIterator][widthIterator][1] >= 200 and resized[heightIterator][widthIterator][2] >= 200:
                whiteDotCoordinates.append([widthIterator, heightIterator])

black = np.zeros([67, 67, 3], dtype=np.uint8)

for i in whiteDotCoordinates:
    black[i[1]][i[0]] = [255, 255, 255]

cv.imwrite("QR.jpg", black)

cap.release()
cv.destroyAllWindows()
```

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)