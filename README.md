# Wazuh-SIEM-Brute-Force-Attack-Demonstration-Home-Lab

## Description

This is a walkthrough of how I configured a Wazuh Cloud environment named "SIEM & XDR Hands-on Training" with a Windows 10 agent running on VMware. To assess the security of the system, I enabled Remote Desktop Protocol (RDP) on the Windows VM. I then utilized a Kali Linux VM to conduct a brute force attack using Hydra, targeting the RDP login. This setup allowed Wazuh to log and monitor all authentication attempts in real-time. The collected logs provided valuable insights into the system's response to potential unauthorized access. From this project, I learned how to effectively leverage Wazuh for monitoring security incidents and gained a deeper understanding of the vulnerabilities associated with RDP.

## Environments Used

- Wazuh Cloud
- VMware
- Windows 10
- Kali Linux

## Program Walkthrough

The first thing I did was create an account in Wazuh to take advantage of the 14-day free trial for the Wazuh Cloud, allowing me to proceed with this project. Once everything was set up, I created an environment that would contain all the Wazuh components. The only remaining step was to enroll my Wazuh agents to get started.

![Creating_Environment](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(24).png)
![Environment Created](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(22).png)

Once the environment was ready, I logged in and was taken to the Overview page. From there, I needed to enroll agents in order to start monitoring my Windows VM.

![Enroll Agent](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(20).png)

After clicking the "Add Agent" link, I followed the steps to successfully add an agent. In Step 1, I chose the type of agent I wanted to deploy: Linux, Windows, or macOS. Since I wanted to deploy a Windows machine, I selected the Windows MSI 32/64-bit option. Next, I assigned a name to the agent for easier identification, as it would otherwise be identified by the machine's IP address. After that, I was provided with PowerShell commands that I needed to run as an administrator on my Windows machine to enroll the VM in the SIEM.

![Deploy Agent](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(19).png)
![Deploy Agent](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(18).png)
![Deploy Agent](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(17).png)
![Powershell](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(16).png)
![Powershell](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(15).png)

After completing all the steps and successfully starting the Wazuh service on my Windows machine, I navigated to the Endpoint Summary. There, I could see that my Windows 10 machine was deployed and it indicated that the machine was active.

![Windows10 Agent](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(14).png)

After confirming that my Windows machine was deployed and being monitored, I began the brute force attack. My Kali Linux machine had the IP address 192.168.209.128. I opened my terminal and first attempted to ping the host and perform an Nmap scan, but I was unable to do so due to the firewall settings. I had to disable the firewall on my Windows machine and enable Remote Desktop Protocol (RDP) for the attack. Once those changes were made, I successfully conducted an Nmap scan, which revealed that RDP port 3389 was open. I then started the brute force attack using Hydra and the rockyou.txt password list, but I didn't allow the brute force process to complete, as this was a simulation.

![Brute Force](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(13).png)
![Brute Force](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(12).png)
![Brute Force](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(11).png)

After conducting the attack, I navigated to the Threat Hunting section to check the logged data for analysis. I observed that there were 45 failed authentication attempts, so I clicked on that data to examine it further. The logs indicated that the brute force attack was categorized as rule level 5, with a rule ID of 60122. I scrolled to the bottom and selected one of the logs for a more detailed analysis. From the "data.win.eventdata.ipaddress" field, which showed 192.168.209.128 (my Kali Linux IP address), and the "data.win.eventdata.workstationName" field, which was labeled as "kali," I was able to confirm that the brute force attack originated from my Kali machine. The log rule description read, "Logon failure - Unknown user or bad password."

![Log Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(10).png)
![Log Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(9).png)
![Log Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Image%20(8).png)
![Log Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/image%20(7).png)
![Log Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/image%20(6).png)
![Log Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/image%20(5).png)


Now, let’s say we had more data logs of different activities. If I wanted, I could filter the logs by either the IP address of the attacker or the rule ID. In Windows, the rule ID for a brute force attempt is either 60122 or 60204, while in Linux, it is 5551 or 5712. In my case, it is 60122, as I am working with a Windows machine.

![Filter Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/image%20(1)(3).png)
![Filter Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/Images%20(1).png)
![Filter Analysis](https://github.com/ofentse579/Images/blob/main/Wazuh%20Brute-Force%20Attack/image%20(1)(2).png)

## Conclusion

In conclusion, this project provided a comprehensive hands-on experience in configuring and utilizing Wazuh as a security information and event management (SIEM) tool. By simulating a brute force attack on a Windows 10 VM, I was able to observe firsthand how Wazuh captures and logs authentication attempts. The analysis of these logs highlighted the importance of monitoring for unauthorized access, particularly in environments with enabled services like RDP. Overall, this experience deepened my understanding of SIEM tools and their capabilities, as well as the critical security implications of remote access protocols. It underscored the need for robust monitoring and incident response strategies in cybersecurity.
