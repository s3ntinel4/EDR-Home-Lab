# EDR-home-lab
A lab to demonstrate EDR functions, so as the attack and defense roles.

The main purpose of the lab is to simulate real cyber attacks as well the implementations used to avoid and respond to these attacks. The strcuture is based on Eric's Capuano [blog] post.


[blog]: https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-intro?utm_campaign=post&utm_medium=web

## The Home Lab

The home lab used will consist of: 1 Windows client with LimaCharlie agent, 1 kali machine to attack the Windows and also the EDR solution chosen will be LimaCharlie an cloud solution to work as EDR.

Windows 10 --

![EDR1](https://github.com/user-attachments/assets/d0a0f50d-4d17-4d89-9466-ae2815c14ce4)
![EDR2](https://github.com/user-attachments/assets/b56b977c-e4c7-4851-b5cf-3754b983de45)
![EDR3](https://github.com/user-attachments/assets/25c55a19-ae75-471c-b3a7-0b2befc6364d)

## The malware staging ...

First, we are going to craft a payload using the sliver c2 framework, this payload will point to our kali attacker machine.
![EDR4](https://github.com/user-attachments/assets/3337f1f7-d5ef-4d78-9eba-6f904b7b543b)

Now we download and execute the payload, as we are testing the payload will be executed with administrative privileges.
![image](https://github.com/user-attachments/assets/21187f1e-0391-4d48-b2bf-fe2fccc9d45b)
![image](https://github.com/user-attachments/assets/855a79a5-d378-41b5-99e5-3eb128ade1ed)
![EDR5](https://github.com/user-attachments/assets/12ba9ccf-0623-4f23-942c-bee56b10be2d)
Now our session is active !!!!

## The detection 

We can check informations about the machine as our session is established including the privileges of our session.
![EDR6](https://github.com/user-attachments/assets/b169f8dc-57d6-4b2d-a4a9-993a38b6efa2)

As an security analyst we would see something like that in the EDR:
![EDR7](https://github.com/user-attachments/assets/2ca41e4b-eb0a-4d68-93a3-6f55c1c1b737)
![EDR8](https://github.com/user-attachments/assets/c1f75b41-cd81-44b5-9d8f-cab9dbc9df09)

By checking the file hash in the virustotal integration we won't receive anything as the malware isn't recognize by the platform, but it doesn't mean it's an legitimate app.
![EDR9](https://github.com/user-attachments/assets/73679722-ad95-4a5f-b9da-a1cb5fe6f6de)

Let's emulate an lsass memory dumping as the attacker would want to get some credentials.
![EDR11](https://github.com/user-attachments/assets/31d43be8-0e99-4b62-b27b-92dc7d7d63ba)

The process can now be detected by our EDR:
![EDR10](https://github.com/user-attachments/assets/84957cb0-67a4-4955-af4d-2390e0fc9a97)

With that said, we craft a rule to detect this kind of attack by craft and detection rule and testing to see if it match the case.
![EDR12](https://github.com/user-attachments/assets/d53bee07-1490-472d-80fc-fadbd9a6f08f)
![EDR13](https://github.com/user-attachments/assets/25c7d5e8-84d7-4540-b615-4a444a4d352b)

Now by repeating the process, the EDR now alert the analyst with the settings established in the detection rule.
![EDR14](https://github.com/user-attachments/assets/70eb7330-a45e-4222-b0ae-978dee47a810)


## The response

Now let's enter in a real world scenario, one of the behaviors expected from a ransomware is the deletion of the shadow copies, as an defense mechanism the analyst can use the shadow copies to restore the machine and frustrate the attacker, the usual command use is the "vssadmin delete shadows /all", so we are going to generate an rule to block it.
First we need to detect the attack happening by spawning a shell in our sliver terminal and then executing the command.
![EDR15](https://github.com/user-attachments/assets/f5e47909-e6bd-40bb-b9bd-cf0d6d2104fe)
![EDR17](https://github.com/user-attachments/assets/bdbd28d6-0d8b-42d3-ae19-2b29c3115093)

Now we trace back the attack by analyzing the process and creating a response rule:
![image](https://github.com/user-attachments/assets/a5314de6-f059-4ab2-b6f1-86492dd3594c)

With that said, we try to execute it again by then our session gets killed by the active response rule.
![EDR16](https://github.com/user-attachments/assets/0e722111-6f3a-4f54-8f71-aa0ed7ea853a)

