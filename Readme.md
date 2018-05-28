[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/fold_left.svg?style=social&label=Follow%20%40CHEF-KOCH)](https://twitter.com/CKsTechNews)
[![Say Thanks!](https://img.shields.io/badge/Say%20Thanks-!-1EAEDB.svg)](https://saythanks.io/to/CHEF-KOCH)
[![Discord](https://discordapp.com/api/guilds/418256415874875402/widget.png)](https://discord.me/CHEF-KOCH)


# What is a VPN kill switch and why is it required?

Kill switch is a technique that helps you prevent unprotected access to the Internet, where your traffic doesn't go through the VPN.

While most leading VPN providers are reliable, there can be _unexpected network drops_ at anytime. Many VPN providers provide 99.99% uptime but there can always be that 0.01% of downtime. Connection drops generally happen because of reasons such as poor Internet connection, an unstable protocol or your firewall settings. There might be any reason behind the drop but it will leak your real IP which can be a really bad thing.


When you turn on the kill switch while using VPN and a connection drop occurs, it will drop the Internet connection as well. This way, you will be unable to send unprotected data on the Internet. Several VPN providers integrating their own solutions and options for this within their own VPN Client software, but it's not needed and the original OpenVPN software (GUI) is more than enough, so this tutorial was basically designed to address the 'issue' that the [OpenVPN GUI doesn't include an option for this](https://forums.openvpn.net/viewtopic.php?t=19193). 



In general there only three ways to prevent a leakage:

* Configure Window own Firewall to disable every outgoing connections on all Private networks, then make the OpenVPN Adapter Public.
* Fiddle with the Windows routing table directly. Requires some strong networking awareness.
* Create a virtual router using VM software like VMWare, VirtualBox, QEMU, Parallels, to install a router-friendly OS like PFSense or Linux + OpenVPN config in a new guest virtual machine. Route your host machine through the guest.
* Some VPN provider directly providing kill switch scripts.



## Is this tutorial secure, or do I need to monitor something manually or even with an external program?

You don't need any additional software or monitor something manually. This is how it works:

* The Windows own firewall will instantly close all your connections when your VPN disconnects, while other programs only disabling your network interface(s).
* Layering your OS with software that does the same thing will not add any extra security, it will likely make it less secure because it's maybe vulnerable, you need to trust the download source and monitor it if it's up2date.



## What about inbound connections?

Inbound rules are only in case your device as a server, and has other remote devices connecting to it. In such a case you will have to create or enable rules to allow connections to come through to your server (RDP etc).



# How do I automatically connect to my VPN Server with OpenVPN

* Go to the configuration directory where you keep the configuration files for OVPN. Usually, they can be found under `C:\Program Files\OpenVPN\config`.
* Create a file called password.txt (or whatever you like to call it). On the first line, write your `username` and on the second line your `password`. The username and password is your authentication to your VPN server.
* Edit the configuration file (.openvpn) that you are use, you can do this with every text editor such as Windows own Notepad or Notepad++.
* Find the command **auth-user-pass** and change it to **auth-user-pass password.txt**. If your configuration doesn't have any of these entries create one yourself.
* Then save the configuration file.



## Requirements

* Windows 7 up to 10 (Home Editions doesn't include secpol.msc)
* A VPN provider
* OpenVPN CLient Software (it also works with other client software, just replace the path to the executable which establish the VPN connection)
* **No** third-party firewall application! 
* Network adapters need to be set from Private to Public
* (_optional_) WFC or Windows 10 Firewall Control



## Some warnings

* Using a hardware firewall is usually the best way to go for locking down your network, however as compliment for the Windows own firewall you could use WFC ([binisoft.org](https://www.binisoft.org/wfc) now Malwarebytes) or Windows 10 Firewall Control ([sphinx-soft.com](http://sphinx-soft.com/Vista/index.html)). Some router firmware simply won't allow an easy OpenVPN integration or simply doesn't support it at all.
* If you ever get a **firewall popup** to add program, make sure to **uncheck Private networks** and only have **Public** networks checked before clicking Allow access. If you fail to monitor this, the kill switch will be pointless.
* Never allow any program to automatically add firewall exceptions. You should only do this manually or whenever you get prompted by Windows Firewall. This isn't a setup and forget solution.
* Existing firewall rules that are assigned the Private/Domain network spaces will be able to still connect, usually it's just local network related stuff. It would be good if you reviewed all rules and adjust them accordingly to your needs.



## What's the difference between VPN Client integrated kill switch and a Windows Firewall kill switch? 

Simple, the client disables your main network interface, while the firewall simply blocks all traffic without disabling any network interface. Private Internet Access for example has an option within their Client Software, which also doesn't 'prevent' the leakage is simply blocks the interface so no communication can pass the interface.



## Problems with third-party integration?

The main problem with any third party application that disables your network adapter is when the VPN connection is terminated, there is a very small window where your IP address can be leaked. Let's not forget to mention that if the client cannot disable the adapter, perhaps due to: security suite, permissions, or when a malfunctioning operating system interferes. A firewall, especially Windows Firewall will have minimum chances of failure if configured correctly; it is arguably the best firewall for Windows in my opinion.



## Switching your main Network Adapter from 'Public' to'Private'

Press **WinKey+R** to bring up the runbox type in: `control.exe /name Microsoft.NetworkAndSharingCenter`.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/control.exe.png">
</p>


Now Press **WinKey+R** to bring up the runbox type in: `secpol.msc`.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/Network%20List%20Manager%20Policies.png">
</p>


Select **Network List Manager Policies** from the left pane and double-click on **WLAN-205946** (this name is different for you). In the right pane right-click on the network name and select the **Network Location** tab from the context menu you can also double-click on it to bring the menu in front of you.


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/Public%20to%20Private.png">
</p>


Change the **Location type** from **Public** to **Private**. You also need to change **User permissions** to **User cannot change location** and then hit **Apply**.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/Network%20And%20Sharing%20Center.png">
</p>


This should be your end-result.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/NetworkAndSharingCenter.png">
</p>

Keep in mind that the network adapter names are different for your network.



# Backup your current Firewall Rules


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Backup%20Current%20Firewall%20Policy/wf.msc.png">
</p>


Press **WinKey+R** to bring up the runbox and enter **wf.msc**.


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Backup%20Current%20Firewall%20Policy/Firewall%20Export%20settings.png">
</p>

* Click **Action** from the top menu bar and press **Select Export Policy...**.

* Save this file to for example your Desktop or any other location you want, the file which will be exported is a Microsoft own format (.wfw), it contains all the firewall rules into one file.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Backup%20Current%20Firewall%20Policy/Restore%20Default%20Policy.png">
</p>


* If you mess up your firewall rules, you can always it later with the **Import Policy...** option which requires the file you have backed up previously. 

Importing .wfw rules (entire profile) will override all existent rules and set everything back to default.



# Creating Outbound Firewall Rules

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/New%20Rule.png">
</p>

* Select **Outbound Rules** on the left pane.
* Then click **New Rule...** button on the right pane.
* Under **Rule Type** select **Program**.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/Allow%20OpenVPN.png">
</p>

* Under **Program** set your path to your OpenVPN installation or the VPN Client which manage your VPN Client side connection, we're using OpenVPN in our example so the path under Windows x64 will be `C:\Program Files\OpenVPN\bin\openvpn.exe`.


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/Allow%20OpenVPN%20connection.png">
</p>


Now we're allowing the connection press in the next dialog **Allow the connection**.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/Allow%20OpenVPN.png">
</p>

Under **Profile** select everything, **Domain**, **Private** and **Public** ensure it's checked (by default everything is selected). In the next dialog you can name your new rule to whatever you want, for example `Our secure VPN Kill Switch`.



# Block all connections for Private/Domain locations

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Block%20all%20Connections%20for%20Private%20%26%20Domain%20connections/Windows%20Firewall%20with%20Advanced%20Security%20on%20Local%20Computer.png">
</p>

Select **Windows Firewall with Advanced Security on Local Computer on the left pane**.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Block%20all%20Connections%20for%20Private%20%26%20Domain%20connections/Domain%20and%20Private%20Profile.png">
</p>


And click **Windows Firewall Properties** on the middle pane.


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Block%20all%20Connections%20for%20Private%20%26%20Domain%20connections/Public%20Profile.png">
</p>

Under the **Domain Profile** and **Private Profile** change **Outbound connections** to **Block**.



# Giving Internet permission to specific applications manually

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Allow%20internet%20permission%20for%20applications%20manually/Allow%20applications%20manually.png">
</p>
 
* Select **Outbound Rules** on the left pane.
* Click **New Rule...** on the very right pane.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Allow%20internet%20permission%20for%20applications%20manually/Profile.png">
</p>

Repeat the same like for the OpenVPN rules you've created earlier, but this time at the end select the **Public** network space since we're want prevent any holes over `Domain` or `Private` locations.


# Notice for WFC users

Some firewall GUI applications such as WFC have special options to avoid that Windows or other applications tampering with your existent rules.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/WFC/WFC%20Secure%20Rules.png">
</p>

**Uncheck** the following 3 options _temporarily_ (if enabled) during the import/export process. **Secure Boot**, **Secure Rules** & **Secure Profile**. The option Secure Boot is now in general not necessary anymore since the integrated Windows Firewall now blocks everything until you're connected to your VPN provider.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/WFC/WFC%20rules.png">
</p>

Uncheck under the WFC **Rules** Tab the **Domain** and **Private** network locations, otherwise you might allow an application accidentally to use these locations which then bypasses the kill switch VPN connection.
