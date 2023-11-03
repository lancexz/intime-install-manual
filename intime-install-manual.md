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
  - [Config the teaching panel(The second robot of the dual-arm system configuration start from here)](#config-the-teaching-panelthe-second-robot-of-the-dual-arm-system-configuration-start-from-here)
  - [Config the EhterCat connection](#config-the-ehtercat-connection)
  - [Start EtherCat Studio](#start-ethercat-studio)
  - [Start INpass(Bridge between Windows and INtime ports)](#start-inpassbridge-between-windows-and-intime-ports)
  - [Test](#test)
- [Config the INtime license server](#config-the-intime-license-server)
  - [Install INtime](#install-intime)
  - [Set IP Address](#set-ip-address)

<br><br>

# Config Wins 10/11 on client PC
## Enter BIOS and disable the following options:
- Make sure the display cable is connecting to the GPU port. Please do not use wireless keyboard for entering the BIOS.
Settings  
- Security
	- Secure Boot `[Disable]` / minimal security [DELL]				   
	- Boot -> Fast Boot `[Disable]` / Slowest boot time [DELL]
- Features 
	- Advanced CPU Configuration
		- Hyper-Threading -> `[Disable]`
		- Intel C-State -> `[Disable]`
		- Intel Turbo Boost -> `[Disable]`
		- Intel Turbo Boost Max Technology 3.0 -> `[Disable]` (Can not be found in DELL)
		- Intel Speed Shift Technology -> `[Disable]`    
- Save and reboot
<br><br>

## Set PC's power option
Control Panel
- Programs -> Turn Windows features on or off -> .NET Framework `4.8 and 3.5` Advanced Services -> `[Enable all]`(Please make sure all the suboptions are enabled. The icon should be a tick instead of square.) -> Reboot after downloading and installation.
- System and Security
    - Administrative Tools -> Services(You can search it in start manu.) -> Windows update(`double click`) -> Startup type: `[Disable]` 
	- Administrative Tools -> Services(You can search it in start manu.) -> Windows update`[Right-click]` and stop.
	- Windows update(`right click`) -> Properties -> Recovery
        - Reset fail count after: `9999`
		- First/Second/Subsequent failure(s): `[Take No Action]` -> `[Right-click]` and stop.
	- Power Options
        - Change settings for the plan: Balanced -> both change to `[Never]`
		- Choose what the power buttom do
			- button behaviors: both change to `[Do nothing]`
		- Change setting that are currently unavailable
            - `[Disable]` the 'Turn on fast startup(recommended)', 'Sleep', 'Hibernate', 'Lock'
<br><br>

## Prepare software
Browser
- Win10pcap -> Download and install
- VS 2019 Community -> Download -> Run as administrator -> Select 'Desktop development with C++' and '.NET desktop development' -> Install(need to wait for a long time)

Load INtime Installation Disk
- `Right click` the DVD -> Open -> intime64full-net_installer -> open(need to wait for a long time for loading) -> Install all -> It will take a long long time. Just wait until it finish.
<br><br>

## Uninstall Windows Defender
Control Panel
- Programs -> Uninstall -> `Uninstall` any antivirus software
- System and Security -> Windows Defender Firewall-> Turn Windows Defender on or off -> Turn all to `[off]`
    - `Disconnect the Internet forever` after this step
<br><br>

## Set IP Address(Server version only)
For the Server License, you need to connect the client PC with the server PC through a hub. Config the INtime license server PC and set the IP Addresses. Otherwise, you can skip this step.

Control Panel -> Network and Internet -> Network and Sharing Center -> Change adapter setting
- Ethernet(`right click`) -> Properties -> IPv4 -> Use the following IP address
    - IP: `'192.168.1.102'`(any available IP is ok under the same LAN)
    - Mask: `'255.255.255.0'`, Gateway: `keep empty`

A reboot may be needed

Make sure the connection is correct between both machines. If the connection fails, please make sure all the PCs can be discovered in LAN. Search tutorial about it.

Check connection:
- On the client PC's terminal: ping 192.168.1.101(the server)
- On the server PC's terminal: pint 192.168.1.102(the client)
<br><br>

## Config INtime
The controller code package -> Open the 'Trajectory' solution file with VS -> 'Trajectory/Header Files/Global.h'
- Find '#define CONTROL_NODE', '#define TRAJECTORY_NODE'
    - Check the names of the Nodes. You need to create all nodes in INtime Configuration Management

Bottom right manu of Windows -> All INtime Kenels are stopped(`right click`)
- `Start NodeA` -> Icon turn red(wait) means the connection is correct -> `Stop NodeA`
- INtime Configuration
    - Node Management(`double click`)
        - New Node -> Node name: 'NodeB', Node type: local
        - Repeat for nodes 'TrajectoryNodeA', TrajectoryNodeB', ...
        - NodeA -> Kernel -> Kernel memory(in MB) -> (Right hand side [...] icon) Enable 'Advanced Memory Configuration
            - Select one row -> Edit area -> `128`(`[Legacy]`)
            - Add area -> `4095`(`[Force high]`)
            - Repeat: Add area -> `4095`(`[Force high]`)
        - NodeB -> Same as NodeA
        - 
        - TrajectoryNodeA/B/ForcesensorNode -> `132 [legacy]` and `4095 [Force high]`
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

## Config the teaching panel(The second robot of the dual-arm system configuration start from here)
- Connect the teaching panel -> Turn on the controller -> Don't need 'Easy settings'
  - If you encounter an Error when power-on, please follow the steps in the 'Error at RC8A robot Controller Power-on' paper sheet inside the manipulator delivery package.
- Setting(F6)
	- Login（F1) -> Maintainer -> Password: `5596060`
	- System info(F2) -> VRC Steeing(F6)
    	- If any of the following Slave motions does not exist, you need to change settings from WinCap III
        	- Check the IP address of the penal: Setting(F6) -> Communication and ...(F5) -> Network and commu(F2) -> Remember the IP address for next step(192.168.0.1 for example)
        	- Find the license key and load the WinCap III disk(Please make sure the language of this software is the same as the language in the teaching panel. When you change the language of the panel, keep the controller turning on until the display of the panel shows black-white-noise with loud fan noise, then you can reboot the controller. The panel will automatically restart three times. ).
        	- DVD(`double click`) -> install -> Input the license key
        	- Connect PC and controllor using the individual single LAN port(below the emergency stop port, bottomleft position)
        	- Change the IP address of the PC to the same local address with controller(192.168.0.2), ping 192.168.0.1
        	- Start WinCap III -> `[0-operator]` -> login -> file -> new project -> select the second one(create a new project with information from the controllor), next -> project name: any, path: any, next -> input the IP address of the controllor, finish -> file -> save -> flie -> close
        	- open the saved_project_folder/project_name_folder/Source Files/
            	- Copy 'ChangeAccessLevel.pcs' to the folder(TODO: 这一步把文件提前放公共U盘）
          	- WinCap III -> file -> open project -> open the saved project file -> communication -> data something(the first one, Ctrl+T) -> left side, program, click the 'ChangeAccessLevel.pcs', send(the teaching panel should be under init interface, the main page, all blue interface)
          	- Check No.370, 373, 378, 385 are all visible in the settings(If any of them are invisible)
            	- Try to execute the manipulator with the teaching panel
            	- Init page(All blue)
          	  	- Check F6 -> F5 -> F5 -> 370(Slave motion setting)/371(Slave motion sync option). If the values are 1, switch them to 0, and restart the RC8
                	- F1 -> Check the 'ChangeAccessLevel.pcs' is loaded
                	- F6 -> F5 -> F1 -> touch TP(Keep the key mode in `[MAN]`) -> OK, OK -> rotate the key to `[AUTO]`
                	- F1 -> F4 -> select the first choice, OK(the numbers will change)
                	- F6 -> F5 -> F1 -> rotate the key to `[MAN]` -> touch I/O, OK, OK
                	- F6 -> Login（F1) -> Maintainer -> Password: `5596060`
              	- Check No.370, 373, 378, 385 are all available in the settings
          	- Change the IP of the PC back to automatically(dynamic IP)
		- No.370: Slave motion setting: `[Enable]`
		- No.373: Communication cycle of Slave Motion: `[250us]`
		- No.378: Slave Motion control mode: `[Velocity control]`
		- No.385: Slave Motion control mode 2: `[pulse]`
- Arm(F2) (Login with Maintainer first)
  	- SHIFT -> Maintenance(F6) -> CALSET(F7) -> Input number -> Copy numbers in CALSET collum to the 'Global.h' `CALSET_1[AXIS_NUM]` -> save the file and rebuild the program(Compile_ALL)
- Turn off the controller -> Disconnect the teaching panel and connect the plug
<br><br>

## Config the EtherCat connection
Connect PC and Controller with LAN cable. For the controller, use the upper-left LAN port(EtherCat in), and then turn on the controller.

Connect the EtherCat Studio license key to the PC(For the second robot, you can skip)
- `Double click` the installed Studio -> Browse -> Select the USB Key file
<br><br>

## Start EtherCat Studio
Slaves Library(`Right click`)
- Open slaves library folder -> (TODO: 这一步把文件提前放公共U盘）Copy 'RC8 ECS MOTION V1.2.XML' from the already configured PC to this folder
- Reload -> The DENSO WAVE INCORPORATED
Start Controller
- File -> New Project -> Network card -> Select the port collect to the robot controllor(check in the network setting) -> Proces Image -> Create PI by tree view -> Attach Master(row manu icon) -> You can find Slave 1(RC8...) if nothing wrong -> Auto Merge
- State(Icon) -> Click Init/Pre-Operational/Safe-Operational/Operational one by one and check the Current/Requested states are the same -> Detach Master(row menu icon) -> Export Master Configuration KPA(row menu icon) -> Save as 'RtEcHdr.xml' on desktop -> Move the file to 'C:Program Files(x86)/Micronet/RSIECAT/bin/NodeA/'(You need to create 'NodeB/' manually and copy 'ecatmkpa' from 'NodeA/' to 'NodeB/')
- Close EtherCat Studio
<br><br>

## Start INpass(Bridge between Windows and INtime ports)
- Next -> Next(If there are several versions, select the latest version of INtime) -> Choose the port connected to the robot(check in the network setting) -> Next -> 'Pass to INtime with MSI', select the current node -> Next -> EtherCAT ->Next ->Next-> OK(Reboot automatically
- INtime configuration -> Node Management -> NodeA -> Auto Load -> Add ->Title:'RSI-ECAT-Master', Path:'C:Program Files(x86)/Micronet/RSIECAT/bin/RtEcHdr.rta'(Not the same one with the one we save), Click all the optional boxes -> OK -> Save -> Close the Node Management
-  (TODO: 这一步把文件提前放公共U盘）Copy the 'EtherCAT_API-Library' folder from the already configured PC to 'C://'
<br><br>

## Test
Start the program .NET -> Test the robot motion. -> `Check the init position of the robot using the simulation first!`

If everything is okay, congratulations!

<br><br>

# Config the INtime license server
Check the Windows version corresponding to the version of INtime.

Change the Setting of this PC and make sure it will not sleep to keep the connection between the Server PC and the Client PCs

## Install INtime

Load Installation Disk and insert the USB license key.
- `Double click` the DVD -> Start without ... -> Start without ... -> Confirm
- `Right click` the DVD -> Open
    - NetUtil -> Server -> setup(`double click`) -> keep default option and install
	- copy files NetUtil/'WlmAdmin, Isapiw32.dll, Isusage' to 'C:/Users/{PC_NAME}'

Load License Files Disk and USB Key(INtime)
- Open WlmAdmin -> Subnet Servers -> DESKTOP...(`right click`) -> Add Feature from file -> To server and its files -> select 'D:/.lic' file from the disk
<br><br>

## Set IP Address
Control Panel -> Network and Internet -> Network and Sharing Center -> Change adapter
- Ethernet(`right click`) -> Properties -> IPv4`[double-clik]` -> Use the following IP address
    - IP: `'192.168.1.101'`
    - Mask: `'255.255.255.0'`, Gateway: `keep empty`