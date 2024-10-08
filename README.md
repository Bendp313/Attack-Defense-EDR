# Attack-Defense-EDR

## Project Simulation

This lab simulates an attack on an endpoint device, demonstrating how to detect and respond to the threat effectively. This was done using two virtual machines. The victim machine ran Windows 10, a commonly used operating system, equipped with Elastic Security for threat detection and response. The attacking machine ran Kali Linux, allowing me to utilize its built-in offensive security tools.

## Set up
I started by setting up the Windows machine as an endpoint device by adding the Elastic Agent with Elastic Security to it.

![agentconfirmation](https://github.com/user-attachments/assets/c4a63935-1173-4347-afb6-33ad400602aa)

Next I confirmed that Elastic was logging the events on the device. Also Windows Defender was turned off so Elastic wouldn't be interupted.

![ipconfigterm](https://github.com/user-attachments/assets/084d705b-db7e-4760-8c2b-620737524f50)


![ipconfiglog](https://github.com/user-attachments/assets/16aceee6-a03d-4500-a591-cf67466a4c34)

## Creating the attack

The attack I wanted to create was a reverse TCP connection since I had experminted with it some already. This lets the attacking machine get remote access to a target device from the CLI
The payload for this attack can be created using the msfvenom tool that comes with Kali.
![generate payload new ip](https://github.com/user-attachments/assets/8540c3c6-f4e6-4533-a709-1c1597d64460)

To try something new and make this more like a real attack I embeded the payload into a website so that it would automatically download when visted. In a real scenario this webstie could be sent to a user by email and a victim would be more likely to vist a website link than download a file themselves. This was done using [this tool found on Github](https://github.com/Arno0x/EmbedInHTML)

One the payload is embeded in the HTML file it is ran on a server ready to be visted by a victim.

![imbedpayload new ip](https://github.com/user-attachments/assets/6ca98cfa-987c-4ade-9140-9b6f4d387bce)

![server](https://github.com/user-attachments/assets/d5d560c5-d1f7-4079-8cea-9afb49386d51)

Back on the windows machine as soon as you access the websites server, the payload will automatically download.

![htmlinbed](https://github.com/user-attachments/assets/20aeac7e-6741-4b41-9da4-95410b9c86eb)

## Attack detection, prevention and response

### Detection
In the SIEM an alert is created and there are more details about what happened on the device.

![elastic alert](https://github.com/user-attachments/assets/ef5b731d-754a-4993-aa92-59a5bc4922d6)

![alert details](https://github.com/user-attachments/assets/06b6c612-4446-430d-83eb-7bc97fdebfde)

### Prevention
On the windows machine the download is prevented by Elastic security because it is flagged as malware.

![eslastic prevent](https://github.com/user-attachments/assets/44521246-e268-4eaa-beac-fe72a5aa9462)

### Response
If the attack got through the security, you can still respond based off of the events. For example, from this alert we could see that a malicous file was downloaded and could assume new events were not from the user but from an attacker. If the event logs show malicous activity the user could be isolated from the network to contain the threat and end the connection.

![action](https://github.com/user-attachments/assets/49529c9e-ef88-4f52-807c-d5b7c2f97664)

![isolate action](https://github.com/user-attachments/assets/242cd62c-79c6-4a6e-a8ab-68b3c90a8b75)

Then the program could be removed and the device could be restored so that the user can be authorized to connect to the network again.

![isolated](https://github.com/user-attachments/assets/a903caf9-43fc-4836-96ed-0419d031e61b)


