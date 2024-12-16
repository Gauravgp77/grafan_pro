README.TXT             				   May 2024

Seqrite Endpoint Protection for LINUX - Version 18.00
Copyright © 2018–2024 Quick Heal Technologies Ltd.
All rights reserved.


====================
System Requirements
====================

Hardware Requirements:

Intel based processor (or compatible), 1 GHz processor or faster 
1 GB free hard disk space 
RAM: 1 GB or more Recommended

Software Requirements:

32-Bit: 
Supported Versions:
GNU C Library 2.5 and later
SAMBA version 4.16 and earlier

Supported Distributions for EPP client:
Debian 9, 10
Ubuntu 14.04,16.04
Boss 6.0
Linux Mint 19.3

Note: Supported only till Linux agent version 10.9.

64-Bit: 
Supported Versions:
GNU C Library 2.5 and later
SAMBA version 4.16 and earlier

Supported Distributions for EPP client:
Fedora 30, 32, 35
Linux Mint 19.3, 20
Ubuntu 16.04, 18.04, 20.4, 22.04
Debian 9, 10
CentOS 7.8, 8.2
RHEL 7.5, 7.8 , 8.2, 8.6, 8.8 Enterprise, 9.0, and 9.3
SUSE Linux 12. SP4 / Enterprise Desktop 15
Rocky Linux 8.4
Boss 6.0, 8.0, 9.0 (Desktop), 8.0 (Server)
Oracle Linux 7.1, 7.9, and 8.1

Note: The Linux agent version 10.11 and onwards, only 64-bit Linux systems are supported.


To check for the latest system requirements, visit the Seqrite website at www.seqrite.com


============= 
Installation
=============

Installing Seqrite Endpoint Protection 
 
To install Seqrite Endpoint Protection Linux Client successfully, 
you have to create installer package, install the antivirus software, 
and install Online Protection. 
 
Creating installer package
To create a Linux Seqrite Client installer, follow these steps: 
1. Log on to Seqrite Endpoint Protection . 
2. Click the Deployment menu. 
3. To create the client installer, click the Create 
   Client Installer button. The Create Client Installer dialog opens.
4. Write the Installer Name and select Group.
5. In the OS Platform list, select Linux.
6. Select the Setup Type (TAR/32 or TAR/64) and the Validity Period. 
   (The validity period can be of 30, 60, or 90 days.)
7. Click Create.
    The <Package Name>.TAR file is created.
8. The installer is created and appears in the list of installers 
   on the Deployment page. Download the installer that you created on 
   your Linux system. 
 
Note 
Password Protection and Proxy settings support is not supported while creating Linux client installer.

Installing Seqrite Endpoint Protection.
To install Seqrite Endpoint Protection, follow these steps:
1. Log in as root and go to the terminal.
2. Go to the directory containing Seqrite Endpoint Protection .
   installation folder and give executable permission to “install” then run ./install script.
    The installation script will copy the necessary files to 
    the /usr/lib/Seqrite/Seqrite folder.
3.Configure Seqrite and save your settings.





=====================================
Using Seqrite Scanner (QT based GUI)
=====================================

Seqrite GUI Scanner, based on QT library has been introduced in this version. It is a separate 
scanner and uses different settings. It also contains Update module which update Seqrite 
AntiVirus in real time. Seqrite GUI Scanner can be executed by giving following command.   

Usage:qhscanui

=====================================
Using Seqrite Command Line Scanner
=====================================

Seqrite Scanner can be executed by giving following command.

Usage: qhscan [drive/path] [options]

qhscan Options:
-DELETE               Delete infected files.
-REPAIR               Disinfect whenever possible.
-DUMB                 Do a "dumb" scan of all files.
-DNAScan              Do a "heuristic" scan of all files.
-WARE[-]              Scan for Adware/Spyware.
-MIME                 Scan for eml files.
-HELP or -?           Display this help.
-LIST                 List all files checked.
-NOSUB                Do not scan subdirectories.
-ARCHIVE[-]           Scan inside archive files.
-PACKED[-]            Unpack compressed executables.
-REPORT=FileName      Create a report file.
-TEMPDIR=DirPath      Temporary Directory name.         

NOTE: Scanning is recommended with root (Super User) privileges.

===============
Schedule Scan
===============

With Seqrite protection, User can schedule scan to be initiated automatically at the frequency defined. 
During scan, defined directories are scanned for viruses and appropriate action can be taken using various commands. 

Usage: confschd [--option]

confschd Options:
--help	Display help and exit. 
--add	Add a new scheduled scan. 
--view	View scheduled scans. 
--modify	Modify an existing schedule scan. 
--delete	Delete a scheduled scan. 
--report	View reports for a scheduled scan. 
--delrpt	Delete reports for a scheduled scan.

Note: Schedule scan is recommended with root (Super User) privileges.


======================
Securing SAMBA Share
======================

Seqrite scans files that are accessed using the SAMBA share.

Supported versions:-
SAMBA version 4.16 and earlier

To secure SAMBA share,
1. On terminal, execute file 'confsmbpro' from /usr/lib/Seqrite/Seqrite/.
2. Do Initial Configuration to add VFS object in the Samba configuration.
3. Select the Samba shares defined in your smb.conf file.
4. Execute configuration file, 'confsmbpro' again to configure Samba Protection Settings.
5. Enable Samba Protection Configuration.
6. Save the configuration.

Note:
1. Restart the SAMBA services, if any modification is made in the smb.conf file.
2. If you disable SAMBA protection using '/usr/lib/Seqrite/Seqrite/confsmbpro', then you should comment or remove the vfs object entry from /etc/samba/smb.conf file.
3. Requires Super User privilege to configure Samba Protection.


===============================
Seqrite Command Line Interface 
===============================
To access Seqrite command line interface execute ./qhavcli script on terminal from '/usr/lib/Seqrite/Seqrite'. 

This will help to configure following features.





=====
Scan
=====
With Scan, you can scan your entire system, directories, or files based on your requirements.

To initiate Scan,

1. Go to the terminal.
2. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
3. Type 1 to select Scan. A screen with the following Scan options appears — [1] Full System Scan [2] Custom Scan.
4. Type 1 to initiate Full System Scan. Your entire system will be scanned. Type 2 to initiate Custom Scan. 
    If you select Custom Scan, you have to type the file path to scan a file or the directory path to scan a folder. 
    If you type directory path, you need to allow whether to scan sub-directory also.

After successful scanning, a scan summary is displayed.


================   
Files & Folders
================
With Files & Folders, you can scan your entire system, folders, or files based on your requirements.

To configure Files & Folders,

1. Go to the terminal.
2. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
3. Type 2 to select Files & Folders. A screen with the following Scan option appears — 	[1] Scan Settings.
   [2]Exclude files and folders.
   [3]Quarantine and backup.

4. Type 1 to select Scan Settings. The Scan Mode screen appears.
5. On the Scan Mode screen, type 1 to select Automatic scan setting that is recommended. 
    Type 2 to select Advanced scan setting that allows you to configure further settings for scan.

Further scan settings for Advanced scan mode are: Scan Packed, Scan Archive, Scan Adware/Spyware, Dumb Scan, Scan Mime, Scan Mailbox, 
DNAScan, and Action on Virus found. You can set On or Off as required while you can set an action when a virus is found from Repair, 
Delete, and Report only.

Note: The setting that you do here will be applicable for the scan process for Scan and External Drives & Devices.
   
===================
Internet & Network
===================
With Internet & Network, you can set the protection rules to protect your system from malicious files that can sneak into your system 
during online activities such as banking, shopping, and surfing. You can enable or disable Browsing and Phishing protections.

To configure Internet & Network:

1. Go to the terminal.
2. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
3. Type 3 to select Internet & Network. A screen with the following options appears — 
    [1] Browsing Protection [2] Phishing Protection.
4. Type 1 to enable Browsing Protection. Type 2 to enable Phishing Protection. 
    You can confirm or change your preference in the succeeding message command line.


==========================
External Drives & Devices
==========================

Whenever your system comes in contact with any external devices, your system is at risk that viruses and malwares 
may infiltrate through them. This feature allows you to set protection rules for external devices such as CDs, DVDs, and 
USB-based drives.

To configure External Drives & Devices,

1. Go to the terminal.
2. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
3. On the Main Menu screen, type 4 to select External Drives & Devices. A screen with the following options appears 
    [1] Scan External Drives.
4. Type 1 to enable Scan External Drives. Confirm your preference in the succeeding message command line.
5. On the Scan External Drives screen, the following options appear 
    [1] Scan files on root of the drive only. 
    [2] Scan full drive.
6. Type 1 to enable Scan files on root of the drive only. Scan files on root of the drive only will help to scan 
    the files on the root of the drive only. The files within the folders on the root drive are skipped. 
    This scan takes little time but is less safe. However, this option is selected by default.
7. Type 2 to enable Scan full drive. Scan full drive helps you to scan all the files on the USB-based drive. 
    This scan takes longer time but is safer. You can confirm or change your preference in the succeeding message command line.


==============
Device Control
==============

With Device Control, you can block access to removable devices to prevent data theft from your system to 
external devices.

With Device Control you can block transfer of the data between the system and external devices such as USB drives and CD/DVD devices. 
Device Control ensures no files or data can be copied from your system to any external devices or vice versa. 
It ensures data security and also eliminates the possibility of transfer of any harmful files.

1. To configure Device Control:
2. Go to the terminal.
3. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
4. Type 5 to select Device Control.
5. On the Device Control screen, the following options appear 
    [1] Storage Devices 
    [2] Wireless Devices.
6. Type 1 to enable Storage Devices.
    Type 2 to enable Wireless Devices. You can confirm or change your preference in the succeeding message command line.


Storage Devices:
----------------

To configure Storage Devices, follow these steps:

1. On the Storage Devices screen, the following options appear 
    [1] Allow 
    [2] Block 
   
2. Type 1 to allow access to a storage device. 
    Type 2 to block access to a storage device. 
    After you type 1 or 2 as per your preference, 
  type 1 or 2 to apply the access rights to the device type.
  


Wireless Devices:
-----------------

To configure Wireless Devices,follow these steps:

1. On the Wireless Devices screen, the following options appear  
    [1] Allow 
    [2] Block.

2. Type 1 to allow access to a wireless device. 
    Type 2 to block access to a wireless device. After you type 1 or 2 as per your preference, 
    type 1 or 2 to apply the access rights to the device type.   
   
Note:
If the policy for storage device on EPP Server is set as Read only, then on Linux client it will be treated 
as blocked and the connected USB device will get blocked. 

=========
Settings
=========
This feature allows you to configure Password Protection, Internet Settings and Update Settings. 
We recommend that you change the settings only when absolutely necessary.

===================
Password Protection
===================
With Password Protection, you can restrict unauthorized people from modifying the Seqrite Endpoint Protection settings so that your security is not compromised. 
It is recommended that you always keep Password Protection turned on.

To configure Password Protection,

1. Go to the terminal.
2. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
3. On Main Menu, type 21 to select Settings.
4. On Settings, type 1 to configure Password Protection setting.

=================
Internet Settings
=================

With Internet Settings, you can turn proxy support on, set proxy type, configure IP address, and port of the proxy for using Internet connection. 
If you are using a proxy server on your network, or Socks Version 4 & 5 network, you need to enter the IP address (or domain name) and port of the proxy, 
SOCKS V4 & SOCKS V5 server in the Internet settings. However, if you configure Internet Settings, you have to enter your user name and password credentials.

To set Internet Settings

1. Go to the terminal.
2. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
3. On Main Menu, type 21 to select Settings.
4. In Enter menu number, type 2.
5. Configure Proxy Server as per the requirement.

================
Update Settings
================
This feature helps you take the updates of latest virus signatures automatically. 
Updating your virus signatures regularly protects your system from the latest malware and virus threats.

To set Update Settings
1. Go to the terminal.
2. Run the command ./qhavcli from the /usr/lib/Seqrite/Seqrite folder.
3. On Main Menu, type 21 to select Settings.
4. In Enter menu number, type 3.
5. Configure Update settings as per the requirement.  
Note
To take updates from the Update Agent, hostname and IP of the Update Agent should be added in the hosts file on Linux system.
To add the hostname in the hosts file, do the following: 
i.Open the Terminal on Linux OS.
ii.Enter command cd /etc.
iii.Enter command sudo vi hosts 
iv.Enter the hostname and IP of the available Update Agents in the hosts file.
v.Save the hosts file.

===============
About section
===============
About section will be displayed with option '23' on the top of CLI menu bar, when user enters '23', About section screen will be displayed with following details:
1.Server details.
2.Sync Now
3.Previous Menu
			
=============
Exit
=============
Exit option is replaced with option ‘24’ in CLI. (Previously it was coming with ‘23’ option)





============
Known Issues
============
1. Online Protection
   - Online Protection feature may not work on some operating systems mentioned under Supported Distributions list.
2. SAMBA Protection
   - If infected files are moved within SAMBA share, the files will not be detected.
   - Samba Protection will not be supported when samba share is accessed from Macintosh to Linux and Linux to Linux.
3. Web Security
   - Websites if accessed by their IP addresses may not be accessible in the following cases: 
     - DNS is not configured when setting the domain based policy.
     - Websites which change IP addresses periodically will not work for the changed IP addresses.
   - Web security will not work if iptables/firewall service is turned off. 
   - SSL versions earlier than 3.1 are not supported for HTTPS protocol. 
4. External Drive & Devices:
   - Scanning is not supported for External CD/DVD drives and mobile device phones.
5. Advance Device Control:
   - Bluetooth USB dongle may not be supported on few operating systems (OS). 
   - USB Hub is not supported. 
   - If any process is working on device mount point, the process will not be cleared or deleted from the mounted directory when the storage device is ejected.
6. On some Linux distros, user needs to disable SELinux or AppArmor to allow Seqrite features such as Online Protection, Samba Protection, Schedule Scan, Device Control to function properly.
7. File /Directory path longer than 1024 characters might be skipped from scanning.
8. System tray icon and qhscanui are not supported on Wayland Display server (type of desktop environment). Some OS like Red Hat based - Fedora 30, 32, CentOS8, CentOS8.1, CentOS8.2, RHEL (newer versions) use Wayland as default desktop environment.
9.System tray might not be available on some OS like Fedora, Debian10, Suse 15 etc. This is an OS limitation and it can be enabled by installing extensions by the user to support app indicator (system tray icon).
10.No Seqrite icon on taskbar when Seqrite application is open. 
11. Cannot connect to X Server may observe on some operating systems when running ./qhscanui with sudo if X server is not having the root user access. Please use commands to resolve : 1. xhost local:root, 2. sudo DISPLAY=$DISPLAY /usr/lib/Seqrite/Seqrite/qhscanui
12. Tray icons and notifications are not supported on systems using the Wayland display protocol.
13. Desktop shortcuts for Seqrite needs a user's Trust Approval to display product's icon shortcut. Double click the shortcut and click "Trust & launch". 







==========
MAN Pages
==========

For more details, please visit below man pages:
confschd:- Schedule Scan Configuration for Seqrite
qhsmbconf:- Seqrite Linux Samba Protection Configuration Tool
qhregister:- Seqrite Linux Registration
qhuninstall:- To uninstall Seqrite AntiVirus


=================
Technical support
=================
Seqrite provides extensive technical support to its registered users.

If you need any support, or have a query, call Seqrite Technical Support on 18002127377

Monday to Saturday 9.00 AM to 9.00 PM (IST)

For customers outside India, visit our website https://www.seqrite.com/seqrite-support-center