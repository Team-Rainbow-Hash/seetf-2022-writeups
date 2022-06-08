## Challenge Name: Huh Where
Author: Mythiology  
Category: OSINT  
Points: 100  
Solves: 52  
<br>
>Bruce Wayne enjoys going for walks at park connectors and taking in the view of the Garden City. He recently stopped by this place which was nearby the trail he was on and took a picture before continuing on his walk.<br><br>
Can you find out where this picture was taken?<br>
Flag format is the name of the place exactly as shown on Google Maps with no capital letters and no spaces. Example would be SEE{123sesamestreet}.

MD5: 87afc4ccbe4c7f1236a7f82d52fab461

[Download Zip File](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/osint/Huh%20Where/files/osint_huh_where.zip "Zip File")

### Approach
Reading the challenge description, we can tell that they are asking for a location near a park connector.

There is an image given to us in the zip file. Opening it up, this is what we get:
![img]( "Image")

From here, there are a number of things that we can observe. Firstly, the image is of the location shows a roundabout around a tree. 

If we look carefully, we can also observe a watermark on the image. The watermark tells us that this image is taken from Google Maps' street view.
![img]( "Image")


With all this information, we can now try to find the location of the image. Putting all the information together, what we can do is to search for park connectors on Google Map to look for an area that has a path in the shape of the circle (due to the roundabout).

First, we can try to search for park connectors and quickly look at the paths in the area. Scrolling through the first few results, we find what we are looking for.
![img]( "Image")

To verify that it is indeed the location that we are looking for, we can go into street view to scout the area. And tada! We have found the location! The area looks the same as inside the challenge image. 
![img]( "Image")

Now we just need to get the location of the place, which is Lor Lada Hitam and put it in the format as required by the challenge. This will give us `SEE{lorladahitam}`.

### Flag
`SEE{lorladahitam}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)