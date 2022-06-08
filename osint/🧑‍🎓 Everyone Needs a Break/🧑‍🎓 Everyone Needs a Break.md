## Challenge Name: 🧑‍🎓 Everyone Needs a Break
Author: Mythiology  
Category: OSINT  
Points: 100  
Solves: 215  
Tags: Beginner Friendly  
<br>
>Despite the pandemic, Bruce Wayne decided to take a break and visit Singapore. He went to feed some birds at one of Singapore's tourist attractions. He eventually became tired and walked over to the location in the attached image to try out another activity.<br><br>
Where is this location?  
Flag format is SEE{Postal Code}. Note that Singaporean postal codes consist of 6 digits.

MD5: 10e28ca0926deb6c98031e6591db74f3

[Download Zip File](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/osint/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Everyone%20Needs%20a%20Break/files/osint_everyone_needs_a_break.zip "Zip File")

### Approach
For this challenge, we are looking for the place shown in the image.<br><br>

When doing OSINT challenges, it's usually very very helpful to read the challenge description very carefully as it contains all the information you need to solve the challenge.  
In the case, the challenge description tells us that "He went to feed some birds at one of Singapore's tourist attractions". This likely refers to Jurong Bird Park, which is the a tourist destination in Singapore where you can feed birds. Since the source also mentions that Bruce Wayne "walked over to the location in the attached image", we can infer that the location is very close to Jurong Bird Park<br><br>

Unzipping the folder and scutinizing the image, we can see that there are large pools of water. This hints at the location being some sort of a fish or prawn farm where people can fish leisurely as "another activity". 
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/osint/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Everyone%20Needs%20a%20Break/files/challenge.png "Image")  

Opening up Google Maps, we can search for "Jurong Bird Park" and see that there is a fish farm beisde it called "ATC Fishing Village @Jurong Hill". 
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/osint/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Everyone%20Needs%20a%20Break/files/Gogle%20Maps.png "Image")  

To confirm that there is the right location, we can go into street view where we find a very similar image to the challenge file.
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/osint/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Everyone%20Needs%20a%20Break/files/Street%20View.png "Image")  

Now that we have found the location, let's get the postal code for the flag!
![img]( "Image")

### Flag
`SEE{629143}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)