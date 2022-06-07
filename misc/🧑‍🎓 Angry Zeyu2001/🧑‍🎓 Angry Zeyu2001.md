## Challenge Name: 🧑‍🎓 Angry Zeyu2001
Author: Infintesky  
Category: Misc  
Points: 100  
Solves: 176  
Tags: Beginner Friendly  
<br>
>After underperforming in the previous CTF, zeyu2001 was really mad at me. He decided to tear my paycheque infront of me. Help me get what ever remnants are left!

MD5: 704f59b279476f16e4fcb17b20d23c06

[Download Zip File](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/misc/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Angry%20Zeyu2001/files/misc_angry_zeyu2001.zip "Zip File")

### Approach
Unzipping the folder, we find many many images. However, on closer inspection (using tools like https://pixspy.com/), we can see that the images all consist of 10*10 pixel and their file names is made up of 2 numbers seperated by a fullstop (e.g `000.000.jpg`). From here, together with the information that the pictures are supposedly "remnants" of the paycheque, we can guess that images are small pieces of a larger image. We can also guess that the number in the image names are the x-coordinates and y-coordinate of the small image's position (more specifically the position of the top-left pixel of the small image) on the larger image.

And now, let's check if we are correct:

First, we can list out all the files in the folder using the below code.
```py
from os import listdir
from os.path import isfile, join
mypath="misc_angry_zeyu2001\distrib\pieces\pieces"
# note that you might have to change the above path to match yours
onlyfiles = [f for f in listdir(mypath) if isfile(join(mypath, f))]
print(onlyfiles)
```
This gives us a list of all the names of the image files in the folder.

We can then use Python Imaging Library to piece all this pixels together on a white background.
```py
from PIL import Image

background = Image.new('RGBA', (600, 300), (255, 255, 255, 255))
# creates the white background of size 600*300 

for i in onlyfiles:
# iterates through the list of images
    img = Image.open(mypath+'\\'+i, 'r')
    # opens the image
    coordinates = (int(i.split(".")[0]),int(i.split(".")[1]))
    # gets the coordinates of the pixel
    background.paste(img, coordinates)
    # pastes the pixel onto the background

background.save('output.png')
# exports the reassembled image as an image
```
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/misc/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Angry%20Zeyu2001/files/output.png "Image")  
And opening `output.png`, we see a reassembled paycheque with amounts to be jealous of lol.  

### Flag
`SEE{boss_aint_too_happy_bout_me_9379c958d872435}`


### Full Solve Script
```py
from os import listdir
from os.path import isfile, join
from PIL import Image

mypath="misc_angry_zeyu2001\distrib\pieces\pieces"
onlyfiles = [f for f in listdir(mypath) if isfile(join(mypath, f))]
print(onlyfiles)

background = Image.new('RGBA', (600, 300), (255, 255, 255, 255))
# creates the white background of size 600*300 

for i in onlyfiles:
# iterates through the list of images
    img = Image.open(mypath+'\\'+i, 'r')
    # opens the image
    coordinates = (int(i.split(".")[0]),int(i.split(".")[1]))
    # gets the coordinates of the pixel
    background.paste(img, coordinates)
    # pastes the pixel onto the background

background.save('output.png')
# exports the reassembled image as an image
```

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)