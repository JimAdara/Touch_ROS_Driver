# Touch_ROS_Driver
力反馈手控器安装步骤(在linux ubuntu18下完成)
=====
1.下载openhaptics的驱动
-----
	下载地址：https://3dssupport.microsoftcrmportals.com/knowledgebase/article/KA-03284/en-us
 	1.下载openhaptics for Linux v3.4驱动.执行./install命令进行安装，安装路径是/opt/.
2.下载Touch device driver v2019.2.15	
----
	下载地址：https://3dssupport.microsoftcrmportals.com/knowledgebase/article/KA-03284/en-us		
	
	Step 1: Download the .tar file and extract the folders. Once extracted there must be 2 main folders namely “Bin and Lib”
	
	Step 2: Copy the executables for Touch Setup and Touch diagnostics from the bin folder to /usr/bin and copy LibPhantomIOLib42.so to /usr/lib
	
	Step 3: In case the user does not have QT, please install QT using the command Install qt: This command is only for Debian based systems
    > sudo apt-get install qt-default
		
	Step 4: Create 3DS directory to configure and save the configuration files needed to setup the Touch and Touch X devices
 	>sudo mkdir /usr/share/3DSystems
	
	Step 5: Set environment variable GTDD_HOME to map path of the configuration file directory created above. Add the following environment variable to the /etc/environment to make it permanent.
	>sudo -H gedit /etc/environment
	>GTDD_HOME="/usr/share/3DSystems"
	>echo $GTDD_HOME
	Logout and login again for the environmental variables to take
	effect use the command:
	>gnome-session-quit
	
	Step 6: Create folder called "config" inside /usr/share/3DSystems.
	>sudo mkdir /usr/share/3DSystems/config
	
	Step 7: Connect the Haptic Device (Touch or Touch X). Use the “ListUSBHapticDevices” script to find out which port the haptic device has been assigned.Set permission for each com port using the command
	Disclaimer: Every time user disconnects and re connects the USB device or the system to which the device is connected goes into hibernation or sleep mode, the user needs to set read/write/execute (Chmod 777) permissions to the COM port. sudo chmod 777 /dev/ttyACM0 (ACM1, ACM2,ACM3 etc..)
	(具体安装步骤可以参考https://s3.amazonaws.com/dl.3dsystems.com/binaries/Sensable/Linux/Touch_Device_Drivers_2019.2.15_Linux_Installation_Guide.pdf)
3.安装依赖项。
-----
	sudo apt-get install --no-install-recommends freeglut3-dev g++ libdrm-dev libexpat1-dev libglw1-mesa libglw1-mesa-dev libmotif-dev libncurses5-dev libraw1394-dev libx11-dev libxdamage-dev libxext-dev libxt-dev libxxf86vm-dev tcsh unzip x11proto-dri2-dev x11proto-gl-dev x11proto-print-dev
4.创建链接。
----
	sudo ln -s /usr/lib/x86_64-linux-gnu/libraw1394.so.11.0.1 /usr/lib/libraw1394.so.8
	sudo ln -s /usr/lib64/libPHANToMIO.so.4.3 /usr/lib/libPHANToMIO.so.4
	sudo ln -s /usr/lib64/libHD.so.3.0.0 /usr/lib/libHD.so.3.0
	sudo ln -s /usr/lib64/libHL.so.3.0.0 /usr/lib/libHL.so.3.0 
5.启动设备。
-----
	The haptic device always creates a COM Port as /dev/ttyACM0 and requires admin priviliges

	chmod 777 /dev/ttyACM0

	Run Geomagic_Touch_Setup in /opt/geomagic_touch_device_driver/ Ensure that the device serial number is displayed
6.诊断设备是否连接
-----
	Run Geomagic_Touch_Diagnostic in /opt/geomagic_touch_device_driver/	
7.启动ROS节点
-----
	Clone and build this repository.

	cd <ROS_workspace>/devel
	source setup.bash
	roslaunch omni_common omni_state.launch 
	Data from the haptic device can be read from the following topics:

	/phantom/button

	/phantom/force_feedback

	/phantom/joint_states

	/phantom/pose

	/phantom/state
