## Challenge Name: 🧑‍🎓 Close Enough
Author: TheMythologist  
Category: Misc  
Points: 100  
Solves: 218  
Tags: Beginner Friendly  
<br>
>My prof mentioned something about not using primes that are close to each other in RSA, but it's close enough, isn't it?<br><br>
Ciphertext is 4881495507745813082308282986718149515999022572229780274224400469722585868147852608187509420010185039618775981404400401792885121498931245511345550975906095728230775307758109150488484338848321930294974674504775451613333664851564381516108124030753196722125755223318280818682830523620259537479611172718588812979116127220273108594966911232629219195957347063537672749158765130948724281974252007489981278474243333628204092770981850816536671234821284093955702677837464584916991535090769911997642606614464990834915992346639919961494157328623213393722370119570740146804362651976343633725091450303521253550650219753876236656017<br><br>
For beginners:
>- https://ctf101.org/cryptography/what-is-rsa/

[Download Zip File](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/crypto/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Close%20Enough/files/crypto_close_enough.zip "Zip File")

MD5: 35601e92f6bc17e36bc042ba30f3ebc4

### Approach
Firstly, to do this challenge, you must understand RSA. If you don't understand it yet, it will be helpful to read up on it [here](https://ctf101.org/cryptography/what-is-rsa/). Lol the link is just the article linked in the challenge description so if you already read that, don't bother.

Unzipping the zip file given, we get 2 files, [encrypt.py](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/crypto/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Close%20Enough/files/encrypt.py) and [key](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/crypto/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Close%20Enough/files/key).

Opening up the file first, we can gather that the e value is 65537 and the n and e value are stored in the key file which is in the PEM format. It can be decoded using the following command: `openssl rsa -pubin -in key -text`.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/crypto/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Close%20Enough/files/OpenSSL.png "Image")

This gets us the modulus, n, in hexadecimal, and the public exponent, e, in decimal. We can decode the modulus into decimal using Python as shown below.
```py
>>>n="4a:4b:fc:4e:b9:e6:fc:4b:28:58:b2:42:f0:9b:60:d3:86:87:dc:79:70:ea:ef:7a:4d:96:ee:c2:03:07:f5:ad:36:05:7d:85:20:6e:26:8a:aa:43:9f:c9:30:ee:ac:b3:95:5e:15:a7:c1:c9:82:46:8f:47:c1:45:04:c8:71:b7:b3:05:d2:e4:ea:12:1a:61:c7:a6:53:2f:f1:ca:d3:2e:56:3e:fe:10:a9:10:59:cb:48:f0:dc:9e:5e:c7:23:b6:bf:a2:9a:ca:a4:28:1e:89:93:a9:84:48:88:86:6f:31:51:0a:a2:bd:6a:99:bc:6c:de:57:d1:1f:8d:e6:80:68:f9:73:b6:ed:e7:b9:1c:9f:71:c3:23:aa:8c:eb:79:4d:b4:7f:c4:8d:49:af:70:0f:ab:ca:aa:d0:20:be:3b:f7:55:aa:6f:95:15:47:45:f3:ef:95:60:ab:d3:d5:44:f1:6d:6a:cd:d5:af:93:45:d6:9f:28:a1:77:ed:ec:b0:28:0a:05:7a:62:af:29:40:4f:55:96:66:4e:9d:dd:1c:aa:6e:1e:db:89:3a:5e:56:ac:e8:bc:41:59:a5:0a:e6:d9:3f:18:9b:a4:cc:46:46:c8:98:ba:3d:39:b1:0e:c9:5b:cf:dd:3c:94:39:4b:5a:96:00:e1:d6:26:aa:1d:69:27"
>>> n=n.replace(":","")
>>> n=int(n,16)
>>> print(n)
9379104451666902807254251547664494589376537004464676565187690588653871658978822987097064298936295147221139510534805502109113119601614394205797875059439905610480321353589582133110727481084808437441842912190040256221115163284631623589000119654843098091251164625806009940056025960835406998838387521455069967004404011645684521669329210152867128697650117219793408414423485717757224152576433432244378386973038733036305783601847652110678653741642215483011184789551861027169721217226927325340419066252945574407810391883801428118671134092909741227928016626842719456736068380990227433485001024796590524675348060787126908578087
```

Now, we have the values of n and we need to find p and q. For this challenge specifically, note that the challenge description hints at the facotrs of n, p and q, being close primes. Thus, we can use this amazing math thingamajig called [Fermat's Factorisation Method](https://en.wikipedia.org/wiki/Fermat%27s_factorization_method) to find the factors of n. 

The script to factor n is as follows:
```py
from math import ceil
import decimal
from decimal import Decimal

n = 9379104451666902807254251547664494589376537004464676565187690588653871658978822987097064298936295147221139510534805502109113119601614394205797875059439905610480321353589582133110727481084808437441842912190040256221115163284631623589000119654843098091251164625806009940056025960835406998838387521455069967004404011645684521669329210152867128697650117219793408414423485717757224152576433432244378386973038733036305783601847652110678653741642215483011184789551861027169721217226927325340419066252945574407810391883801428118671134092909741227928016626842719456736068380990227433485001024796590524675348060787126908578087

def FermatFactors(n):
    decimal.getcontext().prec = 9999
    a = ceil(Decimal(n).sqrt())

    while(True):
        b1 = a * a - n
        b = int(Decimal(b1).sqrt())
        if(b * b == b1):
            break
        else:
            a += 1
    return [a-b, a + b]

p,q= FermatFactors(n)
```

Note: Since we are working with large numbers in Python, we need to use the Decimal library to carry out operations such as square rooting or we will get `OverflowError: int too large to convert to float`.

Now, with p and q, we can now find φ(n), which is equal to (p-q)(q-1). 
```py
totientn = (p-1)*(q-1)
```

Now that we have φ(n), we can find the private key d. Since d is the modular multiplicative inverse of e, `d * e = 1 mod λ(n)`. Thus, we can use the following script to find d.
```py
d = pow(e, -1, totientn)
```
<br>
  
With c, d and n, we can now decode the ciphertext using the equation `m = c^d mod n`. 
```py
m = pow(c, d, n)
```

This will get us the message in numbers as `4795228321404673807027900098606938857110427833246205320646678453411675086257803663158619984121121520277654130236086973480471806106920308261355428359854123093667046781`. 

All that we have to do now is to convert this decimal number into hex before converting it into a string. This can be done as follows:
```py
mhex = hex(m)[2:]
flag = bytearray.fromhex(hex(m)[2:]).decode()
```

This gives us the flag to be `SEE{i_love_really_secure_algorithms_b5c0b187fe309af0f4d35982fd961d7e}`. Yay!

### Flag
`SEE{i_love_really_secure_algorithms_b5c0b187fe309af0f4d35982fd961d7e}`

### Full Solve Script
```py
from math import ceil
import decimal
from decimal import Decimal

c = 4881495507745813082308282986718149515999022572229780274224400469722585868147852608187509420010185039618775981404400401792885121498931245511345550975906095728230775307758109150488484338848321930294974674504775451613333664851564381516108124030753196722125755223318280818682830523620259537479611172718588812979116127220273108594966911232629219195957347063537672749158765130948724281974252007489981278474243333628204092770981850816536671234821284093955702677837464584916991535090769911997642606614464990834915992346639919961494157328623213393722370119570740146804362651976343633725091450303521253550650219753876236656017
n = 9379104451666902807254251547664494589376537004464676565187690588653871658978822987097064298936295147221139510534805502109113119601614394205797875059439905610480321353589582133110727481084808437441842912190040256221115163284631623589000119654843098091251164625806009940056025960835406998838387521455069967004404011645684521669329210152867128697650117219793408414423485717757224152576433432244378386973038733036305783601847652110678653741642215483011184789551861027169721217226927325340419066252945574407810391883801428118671134092909741227928016626842719456736068380990227433485001024796590524675348060787126908578087
e = 65537

def FermatFactors(n):
    decimal.getcontext().prec = 9999
    a = ceil(Decimal(n).sqrt())

    while(True):
        b1 = a * a - n
        b = int(Decimal(b1).sqrt())
        if(b * b == b1):
            break
        else:
            a += 1
    return [a-b, a + b]


p,q= FermatFactors(n)

totientn = (p-1)*(q-1)
d = pow(e, -1, totientn)
print(d)
m = pow(c, d, n)
mhex = hex(m)[2:]
flag = bytearray.fromhex(hex(m)[2:]).decode()
print(flag)
```

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)