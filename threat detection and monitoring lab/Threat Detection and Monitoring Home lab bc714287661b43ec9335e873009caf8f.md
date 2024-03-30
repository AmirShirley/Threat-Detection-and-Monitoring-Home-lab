# Threat Detection and Monitoring Home lab

---

# **Introduction**

Before cybersecurity professionals implement their solutions, they will test them in safe environments and learn how components work and interact with each other. Specifically, cybersecurity analyst create testing environments to better analyze, monitor, and detect threats before applying them to their networks.

# **Steps in this lab include…**

- Configuring pfSense firewall for Network Segmentation & Security
- Configuring Security Onion as an all-in-one IDS, Security Monitoring, and Log Management solution
- Configuring Kali Linux as an attack machine
- Configuring a Windows Server as a Domain Controller
- Configuring Windows desktops
- Configuring Splunk
- Ubuntu/CentOS/Metasploitable/DVWA/Vulnhub machines: All these are potential Linux machines that can be added to the network for exploitation, detection, or monitoring purposes.

I suggest installing all the ISO files first to save time:

Here are all the links for the ISO files used in this Lab:

---

[https://www.pfsense.org/download/?source=post_page-----47bd61428375--------------------------------](https://www.pfsense.org/download/?source=post_page-----47bd61428375--------------------------------)

[https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md?source=post_page-----47bd61428375--------------------------------](https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md?source=post_page-----47bd61428375--------------------------------)

[https://ubuntu.com/download/desktop?source=post_page-----47bd61428375--------------------------------](https://ubuntu.com/download/desktop?source=post_page-----47bd61428375--------------------------------)

[https://www.kali.org/get-kali/?source=post_page-----47bd61428375--------------------------------#kali-platforms](https://www.kali.org/get-kali/?source=post_page-----47bd61428375--------------------------------#kali-platforms)

[https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019?source=post_page-----47bd61428375--------------------------------](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019?source=post_page-----47bd61428375--------------------------------)

[https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise?source=post_page-----47bd61428375--------------------------------](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise?source=post_page-----47bd61428375--------------------------------)

[https://ubuntu.com/download/server?source=post_page-----47bd61428375--------------------------------](https://ubuntu.com/download/server?source=post_page-----47bd61428375--------------------------------)

**HOMELAB NETWORK DESIGN & TOPOLOGY:**

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled.png)

For this lab, I'll be using VMware Workstation 17 Pro as my hypervisor.

# **Configuring Pfsense**

Click “Create a New Virtual Machine” on VMware Workstation Home screen.

Click typical install.

Click “Browse” and navigate to the folder where your pfsense file is located.

Click Next.

Rename your Virtual Machine. I named mine “PfSense”

20GB disk size is sufficient for this VM.

Ensure that the “Split virtual disk into multiple files” option is selected.

Click Next.

Click “Customize Hardware”.

Increase the memory to 2GB.

Add 5 network adapters and correspond them with a VMnet interface as shown below.

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%201.png)

Then click Finish.

The pfsense machine will power on and start with this screen. Accept all the defaults. pfsense will configure and reboot.

You should end up with a screen like this:

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%202.png)

Select Option 1

Should VLANS be set up now [y:n]?: n

Enter em0, em1, em2, em3, em4 & em5 respectively for each consecutive question

Do you want to proceed [y:n]?: y

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%203.png)

Enter option 2

Start with the LAN interface (2)

The IP address **192.168.1.1** is going to be used to access the pfsense WebGUI via the Kali Machine

Use the configuration below for the Lan interface.

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%204.png)

Configure the rest of the settings just like this with the corresponding IP addresses from the Netowrk Topology Diagram.

Leave the OPT3 interface without an IP. This is going to be our span port that Security Onion will be monitoring.

# **Configuring Security Onion**

Click typical install.

Select the ISO file location.

Click next.

Choose Linux, CentOS 7 64-Bit and click Next.

Choose the name of the virtual machine and click next.

Specify disk size (minimum of 200GB)

Click “customize hardware”

Change memory to 16 GB

Add to network adapters and assign them Vmnet 4 and Vmnet 5

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%205.png)

Click finish

Power the virtual machine and click Enter when prompted:

After the machine powers on and ask “Do you wish to continue?”, type yes

Set a username & password:

After security onion reboots, enter the user name and password

Select “Yes”

Click Enter

Select the EVAL option

Type “AGREE”

Select “Standard”

Set a hostname, and a short description

Click the spacebar to select ens33 as the management interface

Set the addressing to DHCP:

Select “YES”

Select “OK”

Select “Direct”

Select “ens35” as the Monitor Interface

Select “Automatic” for the OS patch schedule”

Accept the default home network ip

Accept all the defaults

Enter an email address and password for the admin account

Select “IP”

Select “Yes” for the NTP server & accept the defaults

Take note of your final settings before proceeding! If possible take a screenshot.

Note the IP address for web access. This will be used to access the web interface.

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%206.png)

Select “Yes”

Accessing security onion will be done via a web interface. For this lab I will use an Ubuntu desktop.

Here are the configuration settings:

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%207.png)

Head back to your Security Onion instance and run the following command.

```bash
 sudo so-allow
```

Enter your password

Type a and wait for the process to complete

Type the IP Address from the Ubuntu Desktop machine you created

This will create a firewall rule and security onion that will allow you to access the interface from Ubuntu desktop.

Enter the IP address for the security onion and view alerts

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%208.png)

# **Configuring Kali Linux**

This Kali Linux box will be used as an attack machine to attack the victim network.

Create a Kali Linux VM and add it to Vmnet2

Give it the following hardware configuration

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%209.png)

# **Configuring pfsense Interfaces and Rules**

Navigate to the web browser and search for 192.168.1.1 In the Kali Linux VM.

Select “Advanced…” at this screen:

Accept the risk and continue:

Sign in to pfsense using default credentials “**admin**” & “**pfsense**“

You’ll be greeted with a “Wizard/pfSense Setup/” page.

Click Next till you get to Step 2 of 9.

Add 8.8.8.8 as the Primary DNS Server

Add 4.4.4.4 as the Secondary DNS Server

Click Next.

At Step 3 of 9, Choose your Timezone

Click Next.

At Step 4 of 9, untick the last two options

At Step 5 of 9, Click Next

At Step 6 of 9, Set a new Admin Password

Click Next.

At Step 7 of 9, Click Reload

Finish

At this point, pfsense Wizard is complete and changes can now be made to the Interfaces.

Click on Interfaces.

Select LAN

For “Description“, Change LAN to Kali as this is the Kali interface

Scroll all the way down and Click Save

Then do this for the rest of the Interfaces as shown below

For OPT3, Be sure to go into the edit section and select Enable Interface

Back at Interfaces Assignment select Bridges

Click Add

Select VictimNetwork as the Member Interface

Then select Display Advanced

Under Advanced Configuration for Span Port, select “SPANPORT”

Scroll all the way down and Click Save

Click Firewall >> Rules

Select the Add button with the arrow pointed downward

~ Under “edit Firewall Rule” for Protocol select ANY

~ Scroll all the way down and SAVE

# **Configuring Windows Server as the Domain Controller**

I recently did this in a previous lab which is linked here:

[https://medium.com/@ashirley8888/active-domain-directory-lab-with-1-000-users-7f494014e60c?source=post_page-----47bd61428375--------------------------------](https://medium.com/@ashirley8888/active-domain-directory-lab-with-1-000-users-7f494014e60c?source=post_page-----47bd61428375--------------------------------)

I will run through it again but in Vmware

Select the ISO file path and configure the virtual machine with the following settings

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2010.png)

Click Next

Click Install Now

Select the **Windows Server 2019 standard Evaluation (Desktop Experience)**

Accept the License Terms

Click **Next**

Select the **Custom Install**

Click **New**

Click **Apply**

Click **OK**

Click **Next**

When that is complete, create a password

I'm going to rename the domain controller so it is easier to identify

Search for “pc name” in the settings search

Select Rename PC and rename the PC

Select Restart Now

After the reboot, on the Server Manager Dashboard, Click Manage >> Add Roles and Features

Keep clicking to the prompts until you get to the confirmation menu then click install

Click on the flag with yellow caution triangle

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2011.png)

Select “promote this server to a domain controller”

Select “add new forest”

I have named mine “amir.local”

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2012.png)

Specify a domain name

Click next

Set a password

Click next until you get the prerequisites check menu

Click install

Wait for the reboot

After the system reboots log back in

Select Manage >> Add Roles & Features again on the Server Manager

Click next until it prompts for server roles

Select Active Directory Certificate Services, Then add features

Click next until you get to the confirmation menu

Check “Restart the destination server automatically if required“

Select Yes

Select Install

After the Installation, Click Close

Click on the flag with the yellow caution triangle

Click Configure active directory certificate services on the destination server

Click Next on Credentials

On the Role Services menu, check Certification Authority

Click Next till you get to the Validity period menu and change it to 99 years

Click Next till you get to the Confirmation menu

Then select Configure

Manually restart the server so all the changes can take effect

Back at the server manager select tools > Active directory users and computers

Select your domain and create a new user

Set a password that never expires and select finish

Turn off the firewall for all networks

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2013.png)

Head to Control Panel > Network and Internet > Network Connections

Enter the following settings:

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2014.png)

This will give the domain controller the address to use PFsense as the default gateway

# **Configure Windows 10 and add a user to the Domain**

Use the following settings for the Windows 10 VM

Be sure to remove the floppy drive

Continue all the way up, accepting the defaults until you have a screen that says “I don't have access”

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2015.png)

For Cortana choose not now

Rename and restart the PC

Navigate to network adapter settings

Right click on the Ethernet0 and select properties

Add the IP Address(192.168.2.21) & Use 192.168.2.1 as the default gateway

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2016.png)

Search “domain” and select Access work or school

Enter your domain (name.local)

Join the user to the domain

Enter the Username: Administrator and the password of your DC

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2017.png)

If you head back to the domain controller you can see that we can now view the computer. Go to manage, then go to active directory users and computers. You can see that “PC1” is now added.

# **Installing Splunk on a Ubuntu Server**

Set up splunk using the following settings then start the virtual machine

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2018.png)

Remove the CD/DVD drive with the file named autoinst.iso, as well as the Floppy drive with the file

During the installation, you’ll be prompted to remove the CD(ISO) remove it and then reboot the VM.

After the VM has rebooted your sign on screen should look like this:

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2019.png)

Enter these following commands to install a GUI on the Ubuntu server

```bash
sudo apt update
```

```bash
sudo apt install tasksel
```

```bash
sudo tasksel install ubuntu-gnome-desktop
```

Reboot the vm with the reboot command

```bash
reboot
```

After it reboots you should have your GUI

Navigate to Splunk.com

Create a free splunk account under the 60 day free trial option

Open the terminal and navigate to the downloads directory

Untar the file

Navigate to the ~/splunk/bin directory

Use the command ./splunk start to start the splunk instance.

Navigate to [http://splunk:8000](http://splunk:8000/)

Log in with the username and the password you configured in the Setup wizard

# **Installing Universal Forwarder on Windows Server**

Add the Vmnet6 network adapters to the Splunk adapter

Go to Settings >> Forwarding and Receiving >> New Receiving Port

Name the index “wineventlog” and save

On Windows Server, Download the Universal Forwarder

Accept the License Agreement & click Next

Create a preferred username and password

Enter the IP Address of your Splunk server and the default ports as prompted (8089 & 9997)

Install

Navigate back to your Splunk Instance >> Settings >> Add Data

Select “Forward”

Select the Domain Controller (Windows Server) >> Enter a Server Class Name e.g “Domain Controller” >> Next

![Untitled](Threat%20Detection%20and%20Monitoring%20Home%20lab%20bc714287661b43ec9335e873009caf8f/Untitled%2020.png)

Select Local Events Logs and choose your desired event logs >> Next

Select “wineventlog” as the index >> Review >> Submit

The intention behind creating this lab is for me to have an environment to play around in while I study for my Comptia CySA+ certification. My next lab will definitely be a cloud based enterprise threat detection lab. This lab took quite a while. I hope this lab is useful. Enjoy!