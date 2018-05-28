[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/fold_left.svg?style=social&label=Follow%20%40CHEF-KOCH)](https://twitter.com/CKsTechNews)
[![Say Thanks!](https://img.shields.io/badge/Say%20Thanks-!-1EAEDB.svg)](https://saythanks.io/to/CHEF-KOCH)
[![Discord](https://discordapp.com/api/guilds/418256415874875402/widget.png)](https://discord.me/CHEF-KOCH)


# Why is a VPN kill switch required?

A kill switch ensures that no traffic or IP leakage can happen which might exposes you. With the Windows integrated firewall you can eliminate accidental leakage. 


## Is this tutorial secure, or do I need to monitor something manually or with a external program?


* The Windows own firewall will instantly close all your connections when your VPN disconnects, while other programs only disabling your network interface(s).
* Layering your OS with software that does the same thing will not add any extra security, it will likely make it less secure because it's maybe vulnerable, you need to trust the download source and monitor it if it's up2date.


## What about inbound connections?

* Inbound rules are only in case your device as a server, and has other remote devices connecting to it. In such a case you will have to create or enable rules to allow connections to come through to your server (RDP etc).



## Requirements

* Windows 7 up to 10 (Home Editions doesn't include secpol.msc)
* A VPN provider
* OpenVPN CLient SOftware (also works with others, just replace the path to the executable which etablish the connection)
* **No** third-party firewall application! 
* Network adapters need to be set from Private to Public
* (_optional_) WFC or Windows 10 Firewall Control


## Some warnings

* Using a software or hardware firewall is usually the best way to go for locking down your network, however as compliment for the Windows own firewall you could use WFC (binisoft.org) or Windows 10 Firewall Control (sphinx-soft.com)
* If you ever get a **firewall popup** to add program, make sure to **uncheck Private networks** and only have **Public** networks checked before clicking Allow access. If you fail to monitor this, the kill switch will be pointless.
* Never allow any program to automatically add firewall exceptions. You should only do this manually or whenever you get prompted by Windows Firewall. This isn't a setup and forget solution.
* Existing firewall rules that are assigned the Private/Domain network spaces will be able to still connect, usually it's just local network related stuff. It would be good if you reviewed all rules and adjust them accordingly to your needs.



## What's the difference between VPN Client integrated kill switch and a Windows Firewall kill switch? 

Simple, the client disables your main network interface, while the firewall simply blocks all traffic without disabling any network interface. Private Internet Access for example has an option within their Client Software, which also doesn't 'prevent' the leakage is simply blocks the interface so no communication can pass the interface.


## Problems with third-party integration?

The main problem with any third party application that disables your network adapter is when the VPN connection is terminated, there is a very small window where your IP address can be leaked. Let's not forget to mention that if the client cannot disable the adapter, perhaps due to: security suite, permissions, or when a malfunctioning operating system interferes. A firewall, especially Windows Firewall will have minimum chances of failure if configured correctly; it is arguably the best firewall for Windows in my opinion.


## Switching your main Network Adapter from 'Public' to'Private'


* Press **WinKey+R** to bring up the runbox type in: `control.exe /name Microsoft.NetworkAndSharingCenter`

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/control.exe.png">
</p>


* Press **WinKey+R** to bring up the runbox type in: `secpol.msc`

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/secpol.msc.png">
</p>


* Select Network List Manager Policies from the left pane and double click on Network (name may differ) from the right pane. Right click and select the **Network Location** tab

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/NetworkAndSharingCenter.png">
</p>


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/Network%20List%20Manager%20Policies.png">
</p>


* Change the **Location type** to **Private**

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/NetworkAndSharingCenter.png">
</p>



* Change **User permissions** to **User cannot change location** and click **Apply**.

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Network%20Adapter%20preparation/Public%20to%20Private.png">
</p>



### Open Windows Firewall with 'Advanced Security' Settings

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Backup%20Current%20Firewall%20Policy/wf.png">
</p>


* Press WinKey+R to bring up the runbox and enter **wf.msc**


# Backup Current Firewall Policy


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Backup%20Current%20Firewall%20Policy/Firewall%20Export%20settings.png">
</p>

* Click Action from the top menu bar > **Select Export Policy...**.

* Save to Desktop or other location, the file is a Microsoft own format (.wfw) it contains all the firewall rules.

* If you mess up your firewall rules some how, you can always Import Policy... to restore

* Click Action from the top menu bar > Select **Restore Default Policy**. This will revert your firewall rules to the default ones.


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Backup%20Current%20Firewall%20Policy/Restore%20Default%20Policy.png">
</p>







# Creating Outbound Firewall Rules

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/New%20Rule.png">
</p>

* Select **Outbound Rules** on the left pane
* Click **New Rule...** on the right pane
* Under Rule Type select **Program**

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/Allow%20OpenVPN.png">
</p>

* Under **Program** set your path to your OpenVPN installation (ot the VPN Client which manage your VPN Client side connection)eg `C:\Program Files\OpenVPN\bin\openvpn.exe`.


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/Allow%20OpenVPN%20connection.png">
</p>


* Action > **Allow the connection**

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Outbound%20Firewall%20Rules/Allow%20OpenVPN.png">
</p>

* Under Profile select everything, **Domain**, **Private** and **Public** ensure it's checked (default).

* **Name** > Name: For example **VPN Kill Switch** or whatever you want. 






# Block all Connections for Private/Domain

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Block%20all%20Connections%20for%20Private%20%26%20Domain%20connections/Windows%20Firewall%20with%20Advanced%20Security%20on%20Local%20Computer.png">
</p>

* Select Windows Firewall with Advanced Security on Local Computer on the left pane

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Block%20all%20Connections%20for%20Private%20%26%20Domain%20connections/Domain%20and%20Private%20Profile.png">
</p>


* Click Windows Firewall Properties on the middle pane


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Block%20all%20Connections%20for%20Private%20%26%20Domain%20connections/Public%20Profile.png">
</p>

* Under the Domain Profile and Private Profile change Outbound connections: to **Block**.




# Giving Internet permission to applications manually


<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Allow%20internet%20permission%20for%20applications%20manually/Allow%20applications%20manually.png">
</p>
 
* Select Outbound Rules on the left pane
* Click New Rule... on the right pane

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/Allow%20internet%20permission%20for%20applications%20manually/Profile.png">
</p>

* Do the same as the OpenVPN rules you've created, but this time only select the "Public" network space.


# Notice for WFC users

* Some firewall GUI applications such as WFC have special options, ensure that 

<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/WFC/WFC%20Secure%20Rules.png">
</p>

* **Uncheck** the following 3 options during the import/export process. 



<p align="center"> 
<img src="https://raw.githubusercontent.com/CHEF-KOCH/CK-s-Firewall-killswitch/master/WFC/WFC%20rules.png">
</p>

* Ensure you only allow **public* rules creation, you can make your life easier and avoid making mistakes by unchecking the **Domain** and ''Private** network locations.
