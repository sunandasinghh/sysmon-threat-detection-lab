SOLVING A SYSMON LAB FROM BTLO USING SPLUNK.


1.What is the file that gave access to the attacker?  
# we search for event od 1 seeing what new processes are being created and then filter it by stats count by comaand to filter for commandline sourcetype.
this shows us which process was being created. we see that updater.hta is suspicious to be sure we will see if it has any ther child processes and look for data exfiltration.


![img](https://github.com/sunandasinghh/sysmon-threat-detection-lab/blob/2ebc34e2541ff0faa8d37841be7aac92adb964ca/spl-queries/access.file.png)


2.What is the powershell cmdlet used to download the malware file and what is the port?
#to find this we will see event id 1 for process creation and search for commandline "powershell" because we know that the comaand comes from there.
we see that invole webrequest is downloading malicious files and then again we will use the previous command and see what invoke webrequest uses 6969 port.

![img](https://github.com/sunandasinghh/sysmon-threat-detection-lab/blob/9ca4eca2c50ae157f49fb1fee17d0044a9f8df1a/spl-queries/cmdlet.png)


3.What is the name of the environment variable set by the attacker?
#in this we will search for commandline "exe" or "set"  we see that the attaker set environment to supply.exe

![img](https://github.com/sunandasinghh/sysmon-threat-detection-lab/blob/9ca4eca2c50ae157f49fb1fee17d0044a9f8df1a/spl-queries/environment-variable.png)


4.What is the process used as a LOLBIN to execute malicious commands? 
# Common examples of LOLBINs include PowerShell, cmd.exe, ftp.exe, and wscript.exe, which are integral to the operating system.
In this case it could be powershell.exe or ftp.exe, because all malicious activity was started with powershell command it also downloads malicious file from internet like supply.exe, but in some instances of supply.exe the parent process is ftp.exe.
the correct answer according to the lab is ftp.exe

5.Malware executed multiple same commands at a time, what is the first command executed?
#ipconfig

6.Looking at the dependency events around the malware, can you able to figure out the language, the malware is written 
# using stats count by command for eventdata.targetfilename and see that the processes are executed in python language.

![img](https://github.com/sunandasinghh/sysmon-threat-detection-lab/blob/9ca4eca2c50ae157f49fb1fee17d0044a9f8df1a/spl-queries/language.png)

7.Malware then downloads a new file, find out the full url of the file download
#by searching for eventdata.commandline we get that invoke webrequest is downloading a file using supply.exe environment that was set earlier

![img](https://github.com/sunandasinghh/sysmon-threat-detection-lab/blob/9ca4eca2c50ae157f49fb1fee17d0044a9f8df1a/spl-queries/fileurl.png)


8.What is the port the attacker attempts to get reverse shell?
#Reverse shell mean when attacker establishes a backdoor connection from the infected system to the attacker's Command and Control (C2) server, enabling the attacker to execute commands on the infected machine and potentially exfiltrate sensitive information.
using the stats count by command data we see that its tryig to create bsckdoor connection to 9898

#[img](https://github.com/sunandasinghh/sysmon-threat-detection-lab/blob/df02996c6ed2cba49591ddd2c0cd9518aa6bf5df/spl-queries/reverseshell.png)
