# EDR-Home-Lab-Attack-and-Defense

[Project:](https://github.com/jerrterrell/EDR-Attack-and-Defense#project)

This lab is dedicated to simulating a real cyber attack and endpoint detection and response. Utilizing Eric Capuano's guide online, I will be using virtual machines to simulate the threat & victim machines. The attack machine will utilize 'Sliver' as a C2 framework to attack a Windows endpoint machine, which will be running 'LimaCharlie' as an EDR solution.

Eric Capuano's Guide: https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-intro?utm_campaign=post&utm_medium=web

[Setup](https://github.com/jerrterrell/EDR-Attack-and-Defense#setup)

The first step to the lab is setting up both machines. The attack machine will run on Ubuntu Server and the endpoint will be running Windows 11. In order for this lab to work smoothly, Microsoft Defender should be turned disabled (along with other security settings). Sliver will be installed on the Ubuntu machine as the primary attack tool, and LimaCharlie will be setup on the Windows machine as an EDR solution. LimaCharlie will have a sensor linked to the windows machine, and will be importing sysmon logs.


Windows 11 Machine - 
![real-time](https://github.com/user-attachments/assets/486ad5cc-91fd-4e49-98df-8e64a15d0e39)

![image](https://github.com/user-attachments/assets/d63cc84f-7f91-490f-a314-6b77aff1412b)

![image](https://github.com/user-attachments/assets/a366f75a-b972-47f1-86e8-01349ce335d8)

![image](https://github.com/user-attachments/assets/06a77e82-3e62-4a4b-92b6-9f8af1081cf2)

Ubuntu Server Machine - 
![image](https://github.com/user-attachments/assets/ed642679-d164-4606-a219-891251ea80a2)

[The Attacks and the Defense](https://github.com/jerrterrell/EDR-Attack-and-Defense#the-attacks-and-the-defense)

The first step is to generate our payload on Sliver, and implant the malware into the Windows host machine. Then we can create a command and control session after the malware is executed on the endpoint.


![image](https://github.com/user-attachments/assets/5f6384c5-7c72-4ce2-865b-07d6fa1c036f)

![image](https://github.com/user-attachments/assets/8d0b8be5-aa52-4de4-bea1-a77757c1aca8)

![image](https://github.com/user-attachments/assets/2bf41d30-1c4e-433b-8a8f-5dc50704f747)

![image](https://github.com/user-attachments/assets/ad7dfe55-27f2-44fd-8045-2e2fbe42317e)

Now that we have a live session between the two machines, the attack machine can begin looking around, checking privileges, getting host information, and checking what type of security the host has.

![image](https://github.com/user-attachments/assets/a52cb103-a2ad-408a-8325-d084189a1954)

![image](https://github.com/user-attachments/assets/82cdb8c6-0fe4-4af0-b784-b88505654a93)

![image](https://github.com/user-attachments/assets/8af00c6e-0dc7-4b77-99a2-69bbff39b663)


![image](https://github.com/user-attachments/assets/09e0c202-3308-4205-9c1a-e68c978c4ed0)

On the host machine we can look inside our LimaCharlie SIEM and see telemetry from the attacker. We can identify the payload thats running and see the IP its connected to.

![image](https://github.com/user-attachments/assets/f40527ff-2ffa-4143-87fd-677d20c4753a)

![image](https://github.com/user-attachments/assets/d0b02c5c-4031-48d2-a5a9-df7e7bbda3cb)

We can also use LimaCharlie to scan the hash of the payload through VirusTotal; however, it will be clean since we just created the payload ourselves.

![image](https://github.com/user-attachments/assets/a16ad2f3-885e-4c28-b891-90f8e515feb6)

![image](https://github.com/user-attachments/assets/741c54aa-c990-4c8d-bcf7-d3af33343611)

![image](https://github.com/user-attachments/assets/d13fb160-1d6f-4b6a-85d3-ca6f12e7dbf7)

Now on the attack machine we can simulate an attack to steal credentials by dumping the LSASS memory. In LimaCharlie we can check the sensors, observe the telemetry, and write rules to detect the sensitive process.

![image](https://github.com/user-attachments/assets/cbb1e192-d3eb-4341-beef-895bf588bece)

![image](https://github.com/user-attachments/assets/257a416e-c192-4afa-bb95-fd2145d16a3c)

![image](https://github.com/user-attachments/assets/3f10dcbc-ddf0-4326-b1d5-10cd535d38a3)

Now instead of simply detection, we can practice using LimaCharlie to write a rule that will detect and block the attacks coming from the Sliver server. On the Ubuntu machine, we can simulate parts of a ransomware attack by attempting to delete the volume shadow copies. In LimaCharlie, we can view the telemetry and then write a rule that will block the attack entirely. After we create the rule in our SIEM, the Ubuntu machine will be unsuccessful when trying the same attack again.

![image](https://github.com/user-attachments/assets/edf590c9-888e-4755-8c36-5a31c553d8f7)

![image](https://github.com/user-attachments/assets/ed018e51-a736-4b1a-8013-04d9ed83d2fb)

![image](https://github.com/user-attachments/assets/533bbbd5-ca42-4222-9505-b8a975e65738)

![image](https://github.com/user-attachments/assets/db0bd127-fec5-48dd-9316-50b954cb3ed3)

Due to the D&R rule, the system shell will hang and fail to return anything from the whoami command, because the parent process was terminated.
