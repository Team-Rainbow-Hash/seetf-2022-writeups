## Challenge Name: 🧑‍🎓 Sniffed Traffic
Author: Enyei  
Category: Forensics  
Points: 100  
Solves: 187  
Tags: Beginner Friendly  
<br>
>We inspected our logs and found someone downloading a file from a machine within the same network.<br><br>
Can you help find out what the contents of the file are?<br><br>
For beginners:
> - https://www.javatpoint.com/wireshark

MD5: 71cd3bdbecece8d7919b586959f2d3b7

[Download Zip File](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/forensics_sniffed_traffic.zip "Zip File")


### Approach
Since we are given a PCAP file, let's open it up in Wireshark and see what we can find. Firstly, we can try to find a file transferred because the question states "someone downloading a file from a machine within the same network". 

We can do this by going to the top bar and clicking on the "File" tab before clicking on the "Export Objects" button as shown in the image below.
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Wireshark%20Export%201.png "Image")

Looking through the options given to us, we can see that a number of objects were transferred over HTTP. This includes a zip file, which will be of interest to us.
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Wireshark%20Export%202.png "Image")

We can then select the zip file and save it on our computer for messing around. When we try to unzip the zip file, it will prompt us for a password. Perhaps there was a password sent and recorded in the packet capture in addition to the file?

As seen from the image above when exporting the zip, the zip transfer was recorded as packet 3844. From there, we can locate the transfer in Wireshark before following the TCP stream where the back and forth conversation between the 2 IP addresses will be shown in full.
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Wireshark%20Password%201.png "Image")
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Wireshark%20Password%202.png "Image")

Hmm, there doesn't seem to be the password here. However, we can still check nearby TCP streams by changing this number here (refer to image below).
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Wireshark%20Password%203.png "Image")

And voila! In the next TCP stream, we find ourselves the password, which is `49949ec89a41ed9bdd18c4ce74f37ae4`.
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Wireshark%20Password%204.png "Image")

Now we can put this password into the zip file to get a file called [Stuff]( "File"). During the CTF itself, at this point, I honestly thought that I was done with the challenge and `Stuff` would just contain the flag file, but wow, how woefully wrong was I. Opening stuff in note, instead of getting the flag, I was greeted with a bunch of gibberish. 

Now, we can try putting the file through binwalk to see what sort of file it is or what it might contain.
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Stuff%20Binwalk.png "Image")

We can extract the zip from the stuff file using the command `binwalk -e stuff`
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/Stuff%20Binwalk%202.png "Image")

And no, the fun doesn't end here lol. Turns out the extracted zip file is password protected too! Yay! This time, we can try bruteforcing the zip file password using the [Fcrackzip Tool](https://www.geeksforgeeks.org/fcrackzip-tool-crack-a-zip-file-password-in-kali-linux/). We can also make use of the rockyou.txt wordlist found in /usr/share/wordlists. The command for doing so is `fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt 3E8.zip`.
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/forensics/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Sniffed%20Traffic/files/fcrackzip.png "Image")

Now that we know the password is `john`, we can unzip the file. This gives us `flag.txt` which contains the flag, `SEE{w1r35haRk_d0dod0_4c87be4cd5e37eb1e9a676e110fe59e3}`.


### Flag
`SEE{w1r35haRk_d0dod0_4c87be4cd5e37eb1e9a676e110fe59e3}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)