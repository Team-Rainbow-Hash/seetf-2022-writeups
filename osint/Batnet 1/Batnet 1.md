## Challenge Name: Batnet 1
Author: Mythiology  
Category: OSINT  
Points: 100  
Solves: 109  
<br>
>"With so many people starting to use the Internet, Master Wayne has also joined in the fun. He has been fixated on using all kinds of online accounts not just for leisure, but also for monitoring and surveillance as part of his new artificial intelligence project. For Master Wayne's own safety and as my position as his butler, I need your help to find all of his accounts that use the username 1mn0tb4tm4m." ~ Alfred<br><br>
Note: There are 4 flag fragments to find. There are also limited attempts to this challenge so do not try to bruteforce the flag.<br><br>
Note: There are red herrings created by other participants. Please ignore everything created after the CTF started.<br><br>
Validator service: nc fun.chall.seetf.sg 60001

### Approach
For this challenge, we are looking for the accounts called 1mn0tb4tm4m. First off, we can check some of the major social media platforms for this account name. <br><br>

Firstly, we can try Instagram, one of the most popular photo and video sharing social networking service, which can definitely be used for leisure purposes. Immediately, we find an account with a matching name.
![img]( "Image")

Going into the account, we find some posts and also the first part of the flag, `SEE{br0th3r`, in the user description.
![img]( "Image")

Looking at the post of a discord message from the all mighty zeyu2001, we can infer that Batman uses Discord and is in the SEETF server, where the message was posted. 
![img]( "Image")

We can now try to find Batman in the Discord server. Discord has a function that sends a message for users joining the server, which happened to be enabled in the SEETF server. Interestingly, the message is also marked as from the relevant user, which means that we can search for message from 1mn0tb4tm4m. From there, Discord helps to complete the search request into `from: 1mn0tb4tm4m#0166`. 
![img]( "Image")
![img]( "Image")

From here, we can look at Batman's Discord account, which has an about me. 
![img]( "Image")

From experience with working with Base64, we can recognise that the "=" signs at the back are likely paddings for Base64 and the about me is Base64 encoded. We can decode the about me, finding ourselves another part of the flag. For this, I used an online decoder at https://www.base64decode.org/ but anthing else should work too.
![img]( "Image")
This gets us the 4th part of the flag, `_ab0ut_3very0ne}`.

Now we are just left with the 2nd and 3rd parts of the flag! For this, we can just try randomly try some popular platforms and search within them for 1mn0tb4tm4m. From there, we can try TikTok, thus getting us the following
![img]( "Image")

From here, we can get the 2nd part of the flag to be `_3y3_help_m3`.

Lastlty, trying GitHub, we find an account named 1mn0tb4tm4m. Initially, the flag isn't very obvious, and we only get a message telling the flag is somehwere here.
![img]( "Image")
![img]( "Image")

However, poking around his repositories a bit, we find that his commit message's description contains the third part of the flag, `_f1nd_0ut`.

Lastly, we can piece together all of the parts of the flag and get the full flag! `SEE{br0th3r_3y3_help_m3_f1nd_0ut_ab0ut_3very0ne}`


### Flag
`SEE{br0th3r_3y3_help_m3_f1nd_0ut_ab0ut_3very0ne}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)