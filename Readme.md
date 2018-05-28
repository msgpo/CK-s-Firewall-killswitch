[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/fold_left.svg?style=social&label=Follow%20%40CHEF-KOCH)](https://twitter.com/CKsTechNews)
[![Say Thanks!](https://img.shields.io/badge/Say%20Thanks-!-1EAEDB.svg)](https://saythanks.io/to/CHEF-KOCH)
[![Discord](https://discordapp.com/api/guilds/418256415874875402/widget.png)](https://discord.me/CHEF-KOCH)


# Why is a VPN kill switch is required?

A kill switch ensures that no traffic or IP leakage can happen which might exposes you. With the Windows integrated firewall you can eliminate accidental leakage. 


## Is this tutorial secure, or do I need to monitor something manually or with a external program?


* The Windows own firewall will instantly close all your connections when your VPN disconnects, while other programs only disabling your network interface(s).
* Layering your OS with software that does the same thing will not add any extra security, it will likely make it less secure because it's maybe vulnerable, you need to trust the download source and monitor it if it's up2date.


## What about inbound connections?

* Inbound rules are only in case your device as a server, and has other remote devices connecting to it. In such a case you will have to create or enable rules to allow connections to come through to your server (RDP etc).



## Requirements

* Windows 7 up to 10 (Home Editions doesn't include secpol.msc)
* A VPN provider
* **No** third-party firewall application! 
* Network adapters need to be set from Private to Public


## Warnings

* Using a software or hardware firewall is usually the best way to go for locking down your network, however as compliment for the Windows own firewall you could use WFC (binisoft.org) or Windows 10 Firewall Control (sphinx-soft.com)
* If you ever get a **firewall popup** to add program, make sure to **uncheck Private networks** and only have **Public** networks checked before clicking Allow access. If you fail to monitor this, the kill switch will be pointless.
* Never allow any program to automatically add firewall exceptions. You should only do this manually or whenever you get prompted by Windows Firewall. This isn't a setup and forget solution.
* Existing firewall rules that are assigned the Private/Domain network spaces will be able to still connect, usually it's just local network related stuff. It would be good if you reviewed all rules and adjust them accordingly to your needs.



## What's the difference between TorGuards VPN Client kills witch and a Firewall kills witch? 

Simple, the client disables your main network interface, while the firewall simply blocks all traffic without disabling any network interface. Private Internet Access for example has an option within their Client Software, which also doesn't 'prevent' the leakage is simply blocks the interface so no communication can pass the interface.


## Problems with third-party integration?

The main problem with any third party application that disables your network adapter is when the VPN connection is terminated, there is a very small window where your IP address can be leaked. Let's not forget to mention that if the client cannot disable the adapter, perhaps due to: security suite, permissions, or when a malfunctioning operating system interferes. A firewall, especially Windows Firewall will have minimum chances of failure if configured correctly; it is arguably the best firewall for Windows in my opinion.


## Switching your main Network Adapter from 'Public' to'Private'


* Press WinKey+R to bring up the runbox type in: `control.exe /name Microsoft.NetworkAndSharingCenter`


* Press WinKey+R to bring up the runbox type in: `secpol.msc`

* Select Network List Manager Policies from the left pane and double click on Network(name may differ) from the right pane. Right click and select the **Network Location** tab

* Change the **Location type** to **Private**

* Change **User permissions** to **User cannot change location** and click **Apply**.


### Open Windows Firewall with Advanced Security


* Press WinKey+R to bring up the runbox and enter **wf.msc**


# Backup Current Firewall Policy


* Click Action from the top menu bar >> Select Export Policy...


* Save to Desktop or other location
* If you mess up your firewall rules some how, you can always Import Policy... to restore
* Click Action from the top menu bar > Select Restore Default Policy”. This will revert your firewall rules to the default ones.







# Create Outbound Firewall Rules

* Select Outbound Rules on the left pane
* Click New Rule... on the right pane
* Rule Type > Program
* Program > This program path: C:\Program Files\OpenVPN\bin\openvpn.exe
* Action > Allow the connection
* Profile > Domain/Private/Public all checked
* Name > Name: VPN Kill Switch 






# Block all Connections for Private/Domain


* Select Windows Firewall with Advanced Security on Local Computer on the left pane

* Click Windows Firewall Properties on the middle pane

* Under the Domain Profile and Private Profile change Outbound connections: to Block








# Giving Internet permission to applications manually
 
* Select Outbound Rules on the left pane

* Click New Rule... on the right pane

* Do the same as the TorGuard rule you created, but this time only select the "Public" network space.
