## Challenge Name: 🧑‍🎓 Bonjour
Author: AtlanticBase  
Category: OSINT  
Points: 100  
Solves: 54  
<br>
>Welcome to SEETF! Get used to the environment.<br><br>
Guide: https://github.com/Social-Engineering-Experts/ETH-Guide<br><br>
Challenge: nc awesome.chall.seetf.sg 40005

### Approach
This was a really interesting challenge cause I have never ever encountered a smart contract challenge in any other CTF before so basically this is a first for me.

Firstly, we can just follow the [guide](https://github.com/Social-Engineering-Experts/ETH-Guide) given to us to set up a Metamask account. 
 
Once you have followed the guide, you should have already completed the first step in the challenge. With your token, you can now go for the second step, deploying a smart contract.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/Contact%20Address.png "Image")

With the contract address, we can now deploy it using Remix IDE. First, we can compile our own smart contract using the source code they gave us.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/Source%20Code.png "Image")

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Bonjour {

  string public welcomeMessage;

  constructor() {
    welcomeMessage = "Bonjour";
  }

  function setWelcomeMessage(string memory _welcomeMessage) public {
    welcomeMessage = _welcomeMessage;
  }

  function isSolved() public view returns (bool) {
    return keccak256(abi.encodePacked("Welcome to SEETF")) == keccak256(abi.encodePacked(welcomeMessage));
  }
}
```

Pasting the code in, we can compile it here.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/Compile.png "Image")

Next, under the "DEPLOY & RUN TRANSACTIONS" section, we can select the environment to be "Injected Web3", the account to be the one we created ealier, and the contract under "At Adress" to be the address we were given.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/Deployment.png "Image")

Clicking on "At Address", we can deploy the contract from the address, which will give us this prompt.
![](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/Prompt.png "Image")

And now, we have finished deploying the contract.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/Deployed.png "Image")

After clicking confirm, we will have successfully deployed the contract to our account. However, this isn't the end of the challenge yet. Remember the source code of the contract? Or more specifically the last line?
```
function isSolved() public view returns (bool) {
    return keccak256(abi.encodePacked("Welcome to SEETF")) == keccak256(abi.encodePacked(welcomeMessage));
  }
}
```
Luckily, there is a function for us to change the welcome message to "Welcome to SEETF" to make isSolved() return true. 
```
function setWelcomeMessage(string memory _welcomeMessage) public {
    welcomeMessage = _welcomeMessage;
  }
```

We can use the Remix IDE to interact with the contract as shown in the image below (type the new message in and click on the orange button to call the function to set the welcome message). Metamask will open up another prompt to ask for your permission so you can just click confirm from there.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/welcome%20message.png "Image")

This makes isSolved() return true (you can view the value by clicking on the button), meaning that we have solved the challenge.  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/solved.png "Image")

Now we can just go the netcat server and get the flag. Hooray!  
![img](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups/blob/main/smart%20contracts/%F0%9F%A7%91%E2%80%8D%F0%9F%8E%93%20Bonjour/files/flag.png "Image")


### Flag
`SEE{W3lc0mE_t0_SEETF_a71cda2f322e7834169418a9d1a036a0}`

---
[Back to home](https://github.com/Team-Rainbow-Hash/seetf-2022-writeups)