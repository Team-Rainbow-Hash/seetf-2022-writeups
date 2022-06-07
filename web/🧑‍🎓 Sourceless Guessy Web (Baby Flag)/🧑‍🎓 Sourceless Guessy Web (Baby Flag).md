## Challenge Name: 🧑‍🎓 Sourceless Guessy Web (Baby Flag)
Author: zeyu2001  
Category: Web  
Points: 100  
Solves: 435  
Tags: Beginner Friendly  
<br>
>WHY SO SERIOUS? WHY NEED SOURCE?  <br>  
Contrary to the title of this challenge, you do not need to guess. As usual, do not bruteforce or scan our infrastructure, it is not allowed. <br><br>
<b>Note</b>: There are two flags for this challenge.
<br><br>http://sourcelessguessyweb.chall.seetf.sg:1337<br><br>
For beginners:<br>
> - https://portswigger.net/web-security/file-path-traversal



### Approach
Looking at the article given to us, we can see that this challenge is about file path traversal.

The website given to us has a url of http://sourcelessguessyweb.chall.seetf.sg:1337/?page=whysoserious. The page parameter takes input from us and returns the contents of the specified file. In this case, the application reads from the following file path: `/var/www/images/whysoserious`.

We can then try to retrieve an important file from the server's filesystem. Examples of such would be `/etc/passwd`. We can access that from `/var/www/images/whysoserious` by travelling through up the file directory with `..` and then going to `/etc/passwd`.

Thus, we can get a payload of `../../../etc/passwd`, giving us the file we are looking for. Putting the paylaod into the url, we get http://sourcelessguessyweb.chall.seetf.sg:1337/?page=../../../etc/passwd. 

With the payload, we can then retrieve such a page.
![img]( "Image")  

Highlighting all the text, we can see some invisible text, and the flag is in there (underlined in the image).
![img]( "Image")  

To get the flag, we can go into Inspect Element to find it.
![img]( "Image")


### Flag
`SEE{2nd_fl4g_n33ds_RCE_g00d_luck_h4x0r}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)