# Windows-User-Account-Forensics

Hello we gonna make today Windows-User-Account-Forensics room.

<h4>Learning Objectives</h4>

🛡️Identify and analyze forensic artefacts related to user and system accounts in Windows.

🛡️Understand the forensic aspects of the user account lifecycle, including creation, modification, and deletion.

🛡️Detect malicious activities through behavioural analysis and threat detection techniques.

🛡️Investigate Group Policy Objects (GPOs) for security insights and potential exploitation.

🛡️Apply forensic analysis techniques in practical scenarios to enhance investigative skill


<h3>1.Windows Account Types</h3>

Task 1:What type of accounts are used by the Windows operating system and various apps?
System and service accounts are special accounts in the Windows operating system and various apps. They automatically perform tasks and are designed to ensure software services and system processes function securely and efficiently.

Task 2:What centrally manages local user accounts and domain accounts?
Unlike local user accounts, domain accounts are centrally managed through a domain controller

<h3>2.Account Lifecycle Artefacts</h3>

Task 1:How many users were found using the DSInternals command?
First what we need to is open powershell as administrator and wrote a command ```ntdsutil.exe "activate instance ntds" "ifm" "create full C:\Exports" quit quit```, then ```$bootKey = Get-BootKey -SystemHivePath 'C:\Exports\registry\SYSTEM'``` To fetch the account details, we then call the Get-ADDBAccount cmdlet, passing the path to the NTDS.dit and the boot key: ```Get-ADDBAccount -All -DBPath 'C:\Exports\Active Directory\NTDS.dit' -BootKey $bootKey``` 

And the answer will be a: 5, you can search it that text what will be dsiplayed

Task 2:What is the value of the "bootKey" variable?
its take me while when i figured out, how can get that, that's the answer: 36c8d26ec0df8b23ce63bcefa6e2d821
```Get-BootKey -Online```

Task 3:What is the SID of the domain user, m.ascot?
By the previouse command you can see on that plain text, the SID of that user.

![image](https://github.com/user-attachments/assets/6673f3be-5efb-42d7-883d-f3aa44bf0ca9)

<h3>3.Account Authentication Artefacts</h3>

Task 1:What is the user name used for the NTLM authentication?
Navigate to File > Open in Wireshark and select the pre-captured file under C:\Captures\ntlm.pcapng provided for this exercise.

![image](https://github.com/user-attachments/assets/1df27cca-e392-499d-94e8-8e82c10cd6fc)

And there you can find a username: admin

Task2:What was the Server Challenge sent to the client during the Challenge stage of the NTLM handshake?
You can also check on the below screanshot to check where i found that information.
![image](https://github.com/user-attachments/assets/4c055ec2-84b2-4fea-a9c7-41dfe15cec10)


Task 3:What is the Dns Name of the other result from the the DsGetDomainControllerInfo response?
Check below, where you can find that dns name:

![image](https://github.com/user-attachments/assets/e4c36198-502c-4430-81b5-d51639fecb81)


<h3>4.Group Policy Artefacts</h3>

Task 1:What is the name of the user specified as the apply target for this Policy?
First we need to do is open group polcy managment, then go to tryhackme.com>Group Policy Object>Default Policy Object. 

![image](https://github.com/user-attachments/assets/a1716a4b-50ee-4b9b-acd6-00fb8caf6d64)

Task 2:Under Computer Configuration > Policies > Administrative Templates > Windows Components > Windows Defender Antivirus > Real-time Protection, what is the setting that was enabled?
Navigato to that path, searach in windows run then wrote there gpedit.esc, below there answer:

![image](https://github.com/user-attachments/assets/af62f8c1-c084-45a1-a200-3a3dd64bebae)

Task 3:There is an updated malicious startup PowerShell script. What is the filename of this script? (Without file extension)
We need to navigate to Group Police Managment > Default Domain Policy > Settings 

![image](https://github.com/user-attachments/assets/4f575c99-11f2-4136-9f75-f8550d38408a)


And that's it :)
