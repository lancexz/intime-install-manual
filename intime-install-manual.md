# INtime Installation Manual
This document is a manual for installing and configuring the INtime OS with Denso Robot.

<b>Contents</b>
- [INtime Installation Manual](#intime-installation-manual)
- [Config Wins 10 on client PC](#config-wins-10-on-client-pc)
  - [Enter BIOS and disable the following options:](#enter-bios-and-disable-the-following-options)
  - [Set PC's power option](#set-pcs-power-option)
  - [Prepare softwares](#prepare-softwares)
  - [Uninstall Windows Defender](#uninstall-windows-defender)
  - [Set IP Address(Server version only)](#set-ip-addressserver-version-only)
  - [Config INtime](#config-intime)
  - [Install EtherCat Master/Studio](#install-ethercat-masterstudio)
  - [Config the EhterCat connection](#config-the-ehtercat-connection)
  - [Config the controller](#config-the-controller)
- [Config the INtime license server](#config-the-intime-license-server)
  - [Install INtime](#install-intime)
  - [Set IP Address](#set-ip-address)

<br><br>

# Config Wins 10 on client PC
## Enter BIOS and disable the following options:
Settings  
- Security
	- Secure Boot -> `[Disabled]`				   
	- Boot -> Fast Boot -> `[Disable] `
- Features 
	- Advanced CPU Configuration
		- Hyper-Threading -> `[Disable]`
		- Intel C-State -> `[Disable]`
		- Intel Turbo Boost -> `[Disable]`
		- Inter Turbo Boost Max Technology 3.0 -> `[Disable]`
		- Intel Speed Shift Thchnology -> `[Disable]`    
- Save and reboot
<br><br>

## Set PC's power option
Control Panel
- Programs -> Turn Windows features on or off -> .NET Framework `4.8 and 3.5` Advanced Services -> `[Enable all]`
- System and Security
    - Administrative Tools -> Services -> Windows update(`double click`) -> `[Disable]` 
	- Windows update(`right click`) -> Properties -> Recovery
        - Reset fail count after: `9999`
		- First/Secoun/Subsequent failure(s): `[Take No Action]`
	- Power Options
        - Change settings for the plan: Balanced -> both change to `[Never]`
		- System Settings
            - `[Disable]` the 'Turn on fast startup(recommanded)', 'Sleep', 'Hibernate', 'Lock'
			- button behaviors: both change to `[Do nothing]`
<br><br>

## Prepare softwares
Browser
- Win10Rcap -> Download and install
- VS 2019 Community -> Download -> Run as administrator -> Select 'Desktop development with C++' and '.NET desktop development' -> Install(need to wait for a long time)

Load Installation Disk
- `Right click` the DVD -> Open -> intime64full-net_installer -> open(need to wait for a long time)
<br><br>

## Uninstall Windows Defender
Control Panel
- Programs -> Uninstall -> `Uninstall` any antivirus software
- System and Security -> Windows Defender Firwwall-> Turn Windows Defender on or off -> Turn all to `[off]`
    - `Disconnect Internet forever` after this step
<br><br>

## Set IP Address(Server version only)
For Server License, you need to connect the client PC with server PC through a hub. Config the INtime license server PC and set the IP Addresses. Otherwise, you can skip this step.

Control Panel -> Network and Internet -> Network and Sharing Center -> Change adapter
- Ethernet(`right click`) -> Properties -> IPv4 -> Use the following IP address
    - IP: `'192.168.1.102'`(any available IP is ok under the same LAN)
    - Mask: `'255.255.255.0'`, Gateway: `keep empty`

Reboot may be needed

Check connection:
- on client PC's terminal: ping 192.168.1.101(the server)
- on server PC's terminal: pint 192.168.1.102(the client)

Make sure the connection is correct between both machines. If the connection failed, please make sure all the PCs can be discovered in LAN. Search toturial about it.
<br><br>

## Config INtime
The controller code package -> Open 'Trajectory' sollution flie with VS -> 'Trajectory/Header Files/Global.h'
- Find '#define CONTROL_NODE', '#define TRAJECTORY_NODE'
    - Check the names of the Nodes, you need to create all nodes in INtime Configuration Management

Bottomright manu of Windows -> All INtime Kenels are stopped(`right click`)
- `Start NodeA` -> Icon turn red(wait) means the connection is correct -> `Stop NodeA`
- INtime Configuration
    - Node Management(`double click`)
        - New Node -> Node name: 'NodeB', Node type: local
        - Repeat for nodes 'TrajectoryNodeA', TrajectoryNodeB', ...
        - NodeA -> Kenel memory -> Enable 'Advanced Memory Configuation'
            - Edit area -> `128`(`[Legacy]`)
            - Add area -> `4095`(`[Force high]`)
            - Repeat: Add area -> `4095`(`[Force high]`)
        - NodeB -> Same as NodeA
        - TrajectoryNodeA/B -> `132 [legacy]` and `4095 [Force high]`
        - All nodes -> Kenel Clock Rate -> `250`
    - Development Tools -> Check VS 2019 installed
	- Exit and reboot
- `Start All` -> Icon turn red(wait) -> `Stop All`
<br><br>

## Install EtherCat Master/Studio
Load the EtherCat Master disk -> `right click` the DVD -> Open
- RSIECATDC_Setup(`double click`) and `install`
- INpassSetup(`double click`) and `install`


`[WARNING]` For the following part, the installation order depends on the version of Windows. Please make sure the installation order is correct. Otherwise, you may need to uninstall everything in this part and reinstall them from the beginning. Here is the installation order for `Win 10`.

Load the EtherCat Studio disk -> `right click` the DVD -> Open
- WinPcap(`double click`) and `install`
- KPALicensing...(`double click`) and `install`
- KPAEtherCATStudio..._Setup_86(`double click`) and `install`
- MRT_WIN32_TRAIL...(`double click`) and `install`

If failure occurs, check the Readme.txt(Use DeepL to translate)
<br><br>

## Config the EhterCat connection
Connect PC and Controller with LAN cable. For the controller, use the upper-left LAN port(EtherCat in), and then turn on the controller.

Connect the EtherCat Studio license key to PC
- `Double click` the installed Studio -> Browse -> Select the USB Key file

## Config the controller

<br><br>

# Config the INtime license server
Check the Windows version corresponding to the version of INtime.

Change the Setting of this PC and make sure it will not sleep to keep the connection between Server PC and Client PCs

## Install INtime
Load Installation Disk
- `Double click` the DVD -> Start without ... -> Start without ... -> Confirm
- `Right click` the DVD -> Open
    - NetUtil -> Server -> setup(doubleclick) -> keep default option and install
	- copy files 'WlmAdmin, Isapiw32.dll, Isusage' to 'C:/Users/INtimeServer(PC_NAME)'

Load License Files Disk and USB Key
- Open WlmAdmin -> Subnet Servers -> DESKTOP...(`right click`) -> Add Feature from file -> To server and its files -> select 'D:/.lic' file from the disk
<br><br>

## Set IP Address
Control Panel -> Network and Internet -> Network and Sharing Center -> Change adapter
- Ethernet(`right click`) -> Properties -> IPv4 -> Use the following IP address
    - IP: `'192.168.1.101'`
    - Mask: `'255.255.255.0'`, Gateway: `keep empty`