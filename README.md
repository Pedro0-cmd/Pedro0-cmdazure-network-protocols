<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic


<h2>Actions and Observations</h2>

We'll need to create two virtual machines within the same network. First, create a Windows 10 virtual machine with at least 2 vCPUs and 16 GB of memory. After logging into the machine, log out. Doing this will prompt Azure to create a visible network for our Ubuntu VM, which will be our second virtual machine. When setting up Ubuntu, be sure to select 'Password' as the authentication type.

![image](https://github.com/user-attachments/assets/ade7fb4c-eafe-4f74-93f3-ca685e56e2ed)

![image](https://github.com/user-attachments/assets/04d297ec-3cee-4de6-9a81-2f6c5a7e8dda)

***Note: Log in to your first virtual machine before creating the second one.***

<h2>~Actions</h2>

1-Log in to your Windows 10 virtual machine.

2-Download and install Wireshark.

![image](https://github.com/user-attachments/assets/beeb6c7c-aee5-40a6-8ae0-1b83a7a631cb)

![image](https://github.com/user-attachments/assets/7c443c72-e70f-448b-80bc-8a3c4eee8189)


<h3>Inspecting ICMP Traffic in Wireshark</h3>

1. Open Wireshark and Command Prompt. To open Command Prompt, type "cmd" in the taskbar search bar and press Enter.

![image](https://github.com/user-attachments/assets/97f7a0e7-8aec-473d-9815-2b655a1e7145)

2. In Wireshark, select the ICMP protocol to inspect.

![image](https://github.com/user-attachments/assets/63e8eaf6-763a-4788-96b2-a9598faa1fb6)

3. In Command Prompt, type `ping google.com` and press Enter.

  ![image](https://github.com/user-attachments/assets/ffc0ca4b-3219-4d1d-998a-0a15938b9dee)

4. Check Wireshark to see if it shows ICMP request and reply packets.

![image](https://github.com/user-attachments/assets/b30498c5-0846-452a-9b78-4904701ceb6d)

Wow, incredible! Google is working. However, let’s go above and beyond—what would happen if we tried to ping our second virtual machine and blocked one of the ports?

Remember the second machine we created? Copy its private IP, ping it, and add the -t option.

![image](https://github.com/user-attachments/assets/79502639-3ba8-41d7-b696-cd5350c2ce36)

![image](https://github.com/user-attachments/assets/8a181162-dae1-4561-8d82-e7151754387b)

We can now confirm that our second virtual machine is working.

![image](https://github.com/user-attachments/assets/035b2a7d-115e-4caa-b727-a33a47faaf29)

In Azure, type 'Network Security Groups' and select the Ubuntu virtual machine.

![image](https://github.com/user-attachments/assets/6ef5f1c4-8878-44f7-a1dc-dc8299ab3a71)

Go to 'Inbound security rules' and add a new rule. Remember to select 'ICMPv4' for the port, set the action to 'Deny,' and set the priority to 100. Then click 'Add.'

![image](https://github.com/user-attachments/assets/da75f8cf-9173-49bf-9197-69b763085b20)

One can tell that the Ubuntu virtual machine has blocked the port since it is unreachable. Wireshark shows requests but no responses.

![image](https://github.com/user-attachments/assets/6cc16325-0d49-4975-8ee2-77a474a2fd1f)

Let’s fix it by changing the action from 'Deny' to 'Allow.

![image](https://github.com/user-attachments/assets/6157b6f4-5600-48fd-af17-5969a25f8693)

One can see that requests and replies are working by observing Wireshark. This shows that ICMP traffic can be blocked by modifying the inbound security rules.

![image](https://github.com/user-attachments/assets/8a9f1401-411a-4360-9dbc-84fb4460f59c)

<h3>Inspecting SSH Traffic in Wireshark</h3>

Type `ssh` in Wireshark and enter the username and password for the Ubuntu virtual machine in the Command Prompt, as shown in the example below.

![image](https://github.com/user-attachments/assets/4e010ec3-e679-4e5e-a37f-c4e99e6fc8e0)

![image](https://github.com/user-attachments/assets/036a3d49-934c-44c6-a793-13ef41627032)

Press Enter, and you will see the terminal prompt for your password. Unfortunately, the password won’t be visible because you’re connecting from the Command Prompt.

![image](https://github.com/user-attachments/assets/9ca941e7-1782-4114-a03f-81c92fc3d360)

You're awesome—you made it! Now we are logged into our second virtual machine. Notice how every time you type in the Command Prompt, Wireshark shows encrypted packets. This means we have successfully logged into Ubuntu with WSL.

![image](https://github.com/user-attachments/assets/8c5482ab-ab03-4d7e-8495-be34514a2be7)

![image](https://github.com/user-attachments/assets/6e9e75e4-bd22-403e-901e-237898093a18)

"Don’t forget to type `exit` to log out.

<h3>Inspecting DHCP Traffic in Wireshark</h3>

"Go back to Wireshark and enter `dhcp` in the filter. Then, type `ipconfig /renew` in the Command Prompt. This may temporarily disconnect your virtual machine, but it will help you see that DHCP also has traffic. You can verify this by observing Wireshark."

![image](https://github.com/user-attachments/assets/4f30ec1c-520d-461a-bd48-a893ce21970f)


![image](https://github.com/user-attachments/assets/48cec906-5087-405c-bd33-6574f40f2ca2)

![image](https://github.com/user-attachments/assets/bdb9b86a-4655-4174-be05-c87a51f927d4)


















