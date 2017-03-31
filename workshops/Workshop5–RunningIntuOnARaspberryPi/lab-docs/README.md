# Workshop 5 – Running Intu on a Raspberry Pi

In this workshop you will assemble your own Raspberry Pi, which is a credit card-sized computer. You will then program an LED gesture for Intu, run Intu on your Raspberry Pi, and see your LED gesture in action.

**In this workshop you will complete the following tasks:**

1. [Assemble the Raspberry Pi](#assembling-the-pi)
2. [Set up the Wi-Fi connection for your Raspberry Pi](#wifi-for-pi)
3. [Set up your Raspberry Pi for Builds](#pi-for-builds)
4. [Download and Build](#download-and-build)
5. [Update the configuration](#updating-the-config)
6. [Run Intu on your Raspberry Pi](#run-intu-on-pi)
7. [Create a Plugin](#creating-a-plugin)
8. [Extra Credit -- Using Intu to make a TJBot move its arm](#extra-credit-wave)

**Before you begin:** 

1. You must have a Mac or Windows laptop and you must have completed **Workshop 1: Say Hello!**. After completing Workshop 1 you will have:

	* Your own Organization, Group, and Parent on the [Intu Gateway](rg-`gateway.mybluemix.net`)
	* Your own [Intu Gateway](rg-`gateway.mybluemix.net`) credentials 

2. You must have the following to complete Workshop 5:
	* Raspberry Pi 3 with power cable
	* Anker Bluetooth Speaker with power cable and 3.5mm audio cable
	* USB Mini Microphone
	* PiCamera for the Raspberry Pi 3
	* Neopixel RGB LED
	* Monitor (with a HDMI connection)
	* Keyboard and Mouse (with USB connections)
	* An imaged 32 GB SD card

3. You will notice that *Intu* and *Self* are used interchangeably. *Self* is the technical name for *Intu*.  


**Notes:** 

1. If you **do not** have an imaged card, please go to the [Appendix: Instructions for Running Intu on a Raspberry Pi (Without a Pre-Imaged SD Card)](#appendix-pi)

2. In this workshop commands are issued from **Terminal** on **Mac** or **PuTTY** on **Windows**. For **Windows** users, if you do not have **PuTTY** installed, you can download it using this [link](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). **Windows** users will also require a file management tool to copy files over a network between their local machine and the Raspberry Pi. You can use a stand-alone tool like **Filezilla**, or you may prefer scp via Putty. **Filezilla** can be downloaded using this [link](https://filezilla-project.org/).

## 1. <a name="assembling-the-pi">Assembling the Raspberry Pi</a>

### A. Speaker

1. Before assembling your Raspberry Pi, charge your speaker by connecting it to your laptop using the USB to micro-USB cable. The indicator light will be red while the speaker is charging and blue when fully charged.

	![Speaker charging.](./speaker_charging_in_laptop.png?raw=true)

2. After your speaker is charged, connect it to your Raspberry Pi using the 3.5mm audio cable.

	![Speaker plugged into your Raspberry Pi.](./speaker_in_pi.png?raw=true)

### B. Microphone

Plug the USB microphone into any one of the USB ports of your Raspberry Pi.

![Microphone](./microphone.png?raw=true)

### C. Camera

1.	Locate the connector situated to the **right** of the HDMI port.

2.	Pull the tabs on the top of the connector **upwards** and then **away** from the HDMI port.

3.	Insert the camera’s flex cable firmly into the connector, taking care not to bend the cable at too sharp an angle.

4.	The top part of the connector should then be pushed towards the HDMI port and down while the flex cable is held in place.

5.	Refer to the image below for a properly assembled camera.

	![Camera.](./camera4.png?raw=true)

### D. LED

1.	Obtain the following parts:
	1.	Neopixel RGB LED
	2.	Two female-female jumper wires

	![Neopixel RGB LED](./neopixel_RGB_LED.jpg?raw=true)

2.	Connect Jumper Wire 1 to the GND pin. This is called the **cathode** pin

3.	Connect Jumper Wire 2 to the +3.3V pin. This is called the **anode** pin

4.	Refer to the image below for how your LED will look on an Raspberry Pi 3 Model B, with both the cathode and anode connected.

	![Assembled Raspberry Pi.](./anode_cathode.png?raw=true)
	
### E. Power

To power up your Raspberry Pi, connect the power cable to your Raspberry Pi as shown in the image below. 

![Power cable for Raspberry Pi.](./pi_charger.png?raw=true)

### F. Connecting the Raspberry Pi to an external monitor, keyboard, and mouse

Connect your Raspberry Pi to an external monitor, keyboard, and mouse as shown in the image below.

![Raspberry Pi and external connections.](./external_monitor_keyboard_to_pi.png?raw=true)

## 2. <a name="wifi-for-pi">Set up the Wi-Fi connection for your Raspberry Pi</a>

1. Insert your SD card into your Raspberry Pi if you have not done so already.

2. Connect your Raspberry Pi to a power source. Connect an external keyboard, mouse, and monitor to your Raspberry Pi.

3. You should see a window open on your monitor. Sometimes your power strip might not work correctly. If your Pi does not start, plug it directly into a wall socket. Click on the **Wifi networks** icon at the top of the window, select your network, and enter your password. The network icon looks like:
	
	![wifi](./wifi.png?raw=true)

4. Ensure ssh is enabled on your Raspberry Pi.
	1.	    sudo raspi-config
	2.	    Advanced Options -> SSH -> Enable
	
	Note: If this doesn't work, issue this command on the terminal `sudo service ssh restart`

5.	Get the IP address of your Raspberry Pi.
	1.	Click on the black **Terminal** icon on the top left toolbar.
	2.	Type in `ifconfig` and hit Enter.
	3. Look for the **wlan0** section. The **inet addr** gives you the IP address of your Raspberry Pi (e.g. 10.0.1.2). 

6.	**Important:** Ensure that your laptop is on the **same** network as your Raspberry Pi from this point onwards.
	
	**For Mac users:**
	1. Open a new Terminal window, and connect to the Raspberry Pi using: `ssh pi@{pi's_IP_address}` (e.g. ssh pi@10.0.1.2)
	2. You will be prompted for the Raspberry Pi's password. The default password is **raspberry**.

	**For Windows users:** 
	
	1. Open a new PuTTY window and type in the Raspberry Pi's IP address. 
	2. You will be prompted for the Raspberry Pi's username and password. The default username is **pi** and default password is **raspberry**.


## 3. <a name="pi-for-builds">Setting up your Raspberry Pi for Builds</a>

**Note:** If any step below fails or errors, run: `sudo apt-get update`, then repeat the step.

1.	Download Anaconda on the Raspberry Pi: `wget https://repo.continuum.io/archive/Anaconda2-4.2.0-Linux-x86.sh`

2.	Install Anaconda on your Raspberry Pi and set up the qiBuild.
	
	1. In a new terminal on your Raspberry Pi, run:  `bash Anaconda2-4.2.0-Linux-x86.sh`
	
	2. Follow the steps on the screen to install Anaconda. When you get to the license, keep hitting **Enter** to jump to the bottom. Type **yes** to approve the license.
	
	3. Hit **Enter** to install Anaconda in the default location. **Note**: It may take a while for the progress to update, and if you get the following error, please ignore it. Proceed with the next step.

		```
Anaconda2-4.2.0-Linux-x86.sh: line 484: /home/pi/anaconda2/pkgs/python-3.5.2-0/bin/python: cannot execute binary file: Exec format error
ERROR:
cannot execute native linux-32 binary, output from 'uname -a' is:
Linux raspberrypi 4.4.21-v7+ #911 SMP Thu Sep 15 14:22:38 BST 2016 armv7l GNU/Linux

		```

	5. Once Anaconda has successfully installed, run: `sudo apt-get install python-pip cmake` 
	
		**Note:** If this fails, run `sudo apt-get update` and then rerun: `sudo apt-get install python-pip cmake`

	6.	Run: `sudo pip install qibuild`

3.	Install the wiringPi library on the Raspberry Pi.
	
	1. In a new Terminal/PuTTY window, SSH into your Raspberry Pi: `ssh pi@{ip_address}`. You will be prompted for the username (**pi**) and/or password (**raspberry**) for the Raspberry Pi.
	
	2.	Navigate to your Raspberry Pi's **home directory** by running: `cd /home/pi` 
	
	3.	Run: `git clone git://git.drogon.net/wiringPi`
	
	4.	Now navigate into the wiringPi directory by running: `cd wiringPi/`
	
	5.	Run: `./build`

	You should see a list of classes compiled and "All Done" at the end.

## 4. <a name="download-and-build">Download and Build</a>

### A. Download the Self SDK

1. [Download the Self SDK](https://github.com/watson-intu/self-sdk). Make sure the branch is master and click on download.

2. Create a new directory named **intu** in your **home** directory.

3. Unzip the **self-sdk-master.zip** file into **intu**, making sure that you retain the folder structure, i.e. your intu directory should now contain the unzipped **self-sdk-master** folder. This may take some time.


### B. Building Self SDK

1. Build the SDK by running the following two commands in your terminal:

	```
	cd ~/intu/self-sdk-master
	scripts/build_raspi.sh
	```

**NOTE:**  You may need to mark this script as executable by running the `chmod +x scripts/build_raspi.sh`. If you have any build errors, run: `scripts/clean.sh` and then rerun: `scripts/build_raspi.sh`

## 5. <a name="updating-the-config">Updating the configuration</a>

1. [Log in to the Intu Gateway](https://rg-gateway.mybluemix.net/). 

2. Click on **VIEW CREDENTIALS** in the left hand navigation bar.

3. Select your Organization and Group in the top Filter by menu. Click on the **Get Credentials** button.

4. On the **Raspberry Pi**, navigate to **~/intu/self-sdk-master/bin/raspi**. If there is no `config.json` file present, create a file and name it `config.json`. If there is already a `config.json` file, delete its contents. Paste the credentials obtained from the gateway in step 3 in the `config.json` file.

## 6. <a name="run-intu-on-pi">Run Intu on your Raspberry Pi</a>

1. Run Intu on your Raspberry Pi by executing the following steps in your terminal:

	```
	cd  ~/intu/self-sdk-master/bin/raspi
	./run_self.sh
	```

When Intu starts, it will alert you by saying "Ah, I feel so much better".  After hearing this you will be able to ask questions to your Raspberry Pi like "how are you", "tell me a joke", or "what is your name" and it will respond. You can stop the program by pressing **Control-C**.

**NOTE:** If you have a HDMI cable plugged into your Raspberry Pi, verify that the sound is set to **analog**. This can be done by right clicking the **speaker icon** at the top right hand corner of the Raspberry Pi's homescreen and selecting **analog**.  Verify that you have a microphone and speaker plugged into your Raspberry Pi. Note that your speaker may need to be charged before use. 


## 7. <a name="creating-a-plugin">Creating a Plugin</a>

Now create your own LED plugin that will blink when Intu tells a joke.

1. SSH into your Pi or use an existing SSH connection and run the following two commands:

	```
	cd ~/intu/self-sdk-master/examples/
	mkdir workshop_five
	```

2. Create your CMakeLists.txt by executing the following two commands in your terminal:

	```
	cd workshop_five
	nano CMakeLists.txt
	```

3. Paste or type the following code into the file:
	
	```
	include_directories(. wiringPI)
	SET(GCC_COVERAGE_LINK_FLAGS "-lwiringPi")
	add_definitions(${GCC_COVERAGE_LINK_FLAGS})

	file(GLOB_RECURSE SELF_CPP RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "*.cpp")
	qi_create_lib(workshop_five_plugin SHARED ${SELF_CPP})
	qi_use_lib(workshop_five_plugin self wdc)
	qi_stage_lib(workshop_five_plugin)

	target_link_libraries(workshop_five_plugin wiringPi)
	```

4. Create a new file called **WorkshopFiveGesture.h** by running `nano WorkshopFiveGesture.h` in the terminal. Paste in the following code:

	```
	#ifndef SELF_WORKSHOPFIVEGESTURE_H
	#define SELF_WORKSHOPFIVEGESTURE_H

	#include "gestures/AnimateGesture.h"

	//! This is the class for animating a Raspberry Pi, such as changing LED colors.
	class WorkshopFiveGesture : public AnimateGesture
	{
	public:
		RTTI_DECL();

		//! IGesture interface
		virtual bool Execute( GestureDelegate a_Callback, const ParamsMap & a_Params );
		virtual bool Abort();

		//! Construction
		WorkshopFiveGesture() : m_PinNumber( 7 ), m_bWiredPi( false )
		{}

	private:
		//! Data
		bool m_bWiredPi;
		int m_PinNumber;

		//! Callbacks
		void AnimateThread( Request * a_pReq );
		void DoAnimateThread( Request * a_pReq );
		void AnimateDone( Request * a_pReq );
	};
	#endif //SELF_WORKSHOPFIVEGESTURE_H
	```
	
5. Create a file called **WorkshopFiveGesture.cpp** by running `nano WorkshopFiveGesture.cpp` in the terminal. Paste in the following code:

	```
	#include "wiringPi.h"
	#include "WorkshopFiveGesture.h"
	#include "SelfInstance.h"
	#include "utils/ThreadPool.h"
	#include "blackboard/BlackBoard.h"
	#include "blackboard/Status.h"

	REG_OVERRIDE_SERIALIZABLE( AnimateGesture, WorkshopFiveGesture );
	REG_SERIALIZABLE(WorkshopFiveGesture);
	RTTI_IMPL( WorkshopFiveGesture, AnimateGesture );

	bool WorkshopFiveGesture::Execute( GestureDelegate a_Callback, const ParamsMap & a_Params )
	{
		if ( PushRequest( a_Callback, a_Params ) )
			ThreadPool::Instance()->InvokeOnThread(  DELEGATE( WorkshopFiveGesture, AnimateThread, Request *, this ), ActiveRequest() );
		return true;
	}

	bool WorkshopFiveGesture::Abort()
	{
		return false;
	}

	void WorkshopFiveGesture::AnimateThread( Request * a_pReq )
	{
		try {
			DoAnimateThread(a_pReq);
		}
		catch( const std::exception & ex )
		{
			Log::Error( "WorkshopFiveGesture", "Caught Exception: %s", ex.what() );
		}

		// now invoke the main thread to notify them the move is completed..
		ThreadPool::Instance()->InvokeOnMain( DELEGATE(WorkshopFiveGesture, AnimateDone, Request *, this ), a_pReq );
	}

	void WorkshopFiveGesture::DoAnimateThread(WorkshopFiveGesture::Request * a_pReq)
	{
		Log::Debug("WorkshopFiveGesture", "in DoAnimateThread");
	}

	void WorkshopFiveGesture::AnimateDone( Request * a_pReq )
	{
		Log::Debug("WorkshopFiveGesture", "In AnimateDone");
		Status::SP spStatus(new Status());
		SelfInstance::GetInstance()->GetBlackBoard()->AddThing(spStatus);
		// pop the request, if true is returned then start the next request..
		if ( PopRequest() )
			ThreadPool::Instance()->InvokeOnThread(DELEGATE(WorkshopFiveGesture, AnimateThread, Request * , this), ActiveRequest());
	}
	```
6. Update the **DoAnimateThread** function with the following code:

	```
	// Code snippet for DoAnimateThread()
	Log::Debug( "WorkshopFiveGesture", "Gesture %s is running.", m_GestureId.c_str() );

	if ( !m_bWiredPi )
	{
		wiringPiSetup();
		pinMode(m_PinNumber, OUTPUT);
		m_bWiredPi = true;
	}
	for(size_t i=0;i<5;++i)
	{
		digitalWrite(m_PinNumber, HIGH);
		delay(200);
		digitalWrite(m_PinNumber, LOW);
		delay(200);
	}
	```
7. Add your new plugin into the build.

   1. Edit the **CMakeLists.txt** file in the examples directory by running the following commands in your terminal:
   
		```
		cd ~/intu/self-sdk-master/examples
	    nano CMakeLists.txt
		```
	
   2. Carefully add the following line at the end of the file: 
   
   		`add_subdirectory(workshop_five)`
   		
   3. Save your changes to the `CMakeLists.txt` file. 
           
8. Build Self on your Raspberry Pi with the following steps:

	```
    cd ~/intu/self-sdk-master
    scripts/build_raspi.sh
	```

9. To configure your `platform.json` file, run the following command in your terminal:
	
	```
	nano ~/intu/self-sdk-master/bin/raspi/etc/shared/platforms/raspi/platform.json
	```

    1. Locate the `m_Libs` variable and change it to read: 
    
    	`"m_Libs":[ "platform_raspi", "camera_plugin", "workshop_five_plugin"]` 
    
    2. Save your changes and close the file.

10. Run Intu on your Raspberry Pi by completing the following steps in your terminal window:
	
	```
	cd  ~/intu/self-sdk-master/bin/raspi
	./run_self.sh
	```
	
	Once Intu is running you will hear "Ah I feel so much better". After hearing this you will then be able to ask "Tell me a joke". Your LED will light up after it is done telling you a joke.
	
# <a name="extra-credit-wave">Workshop 5 Extra Credit</a>
## Using Intu to Make a TJBot move its arm

Now that you are familiar with the Raspberry Pi we can take this to the next stage and start working with the servo motor. You will program a Move Joint gesture for Intu, run Intu off of your Raspberry Pi, and see your TJBot's arm in action.  

## 1. Adding the Servo Motor

### A. Connecting the Servo Motor

As part of this lab we will be using the Tower Pro SG90 micoservo. You can see the pin out of this servo motor [here](./SG90Servo.pdf). You will need to connect the servo motor to the Raspi Board as below:

![Board Layout for Servo](./sevo_pin_layout.png?raw=true)

## 2. Creating the Waving Arm Gesture with Intu
1. Inside the **self-sdk-master** directory navigate to the **examples** directory. Inside here make an new directory called **move\_arm\_joint**

2. Create a **CMakeLists.txt** file in the **~/intu/self-sdk-master/examples/move\_arm\_joint** directory, and paste the following code:
	
	```
	include_directories(. wiringPI)
	SET(GCC_COVERAGE_LINK_FLAGS "-lwiringPi")
	add_definitions(${GCC_COVERAGE_LINK_FLAGS} "-DBOOST_ASIO_DISABLE_STD_CHRONO -DBOOST_FILESYSTEM_VERSION=3")

	file(GLOB_RECURSE SELF_CPP RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "*.cpp")
	qi_create_lib(move_joint_plugin SHARED ${SELF_CPP})
	qi_use_lib(move_joint_plugin boost boost_system self wdc tinythread++)
	qi_stage_lib(move_joint_plugin)

	target_link_libraries(move_joint_plugin wiringPi)	
	```
	
3. Add the following to the **~/intu/self-sdk-master/examples/CMakeLists.txt** :
	```
	add_subdirectory(move_arm_joint)
	```
	
4. Create a file called **RaspiMoveJointGesture.h** in the **move\_arm\_joint** directory and paste the following code:
	
	```
	#ifndef SELF_MoveArmJointGesture_H
	#define SELF_MoveArmJointGesture_H

	#include "gestures/MoveJointGesture.h"

	//! This is the class for waving arm joints for RaspberryPI
	class RaspiMoveJointGesture : public MoveJointGesture
	{
	public:
		RTTI_DECL();
		//! IGesture interface
		virtual bool Execute( GestureDelegate a_Callback, const ParamsMap & a_Params );
		virtual bool Abort();
		//! Construction
		RaspiMoveJointGesture() : m_PinNumber( 0 ), m_bWiredPi( false ), m_ArmLowPoint(0), m_ArmHighPoint(40)
		{}
	private:
		//! Data
		bool m_bWiredPi;
		int m_PinNumber;
		int m_ArmLowPoint;
		int m_ArmHighPoint;
		static int m_CurrentPWMArmState;

		//! Callbacks
		void MoveJointThread( Request * a_pReq );
		void MoveJointDone( Request * a_pReq );
		void MoveArm();
		int PWMValue();
	};
	#endif
	```
5. Create a file called **RaspiMoveJointGesture.cpp** in the **move\_arm\_joint** directory and paste the following code:
	
	```
	#include "wiringPi.h"
	#include "softPwm.h"
	#include "RaspiMoveJointGesture.h"
	#include "SelfInstance.h"
	#include "utils/ThreadPool.h"
	#include "blackboard/BlackBoard.h"
	#include "blackboard/Status.h"

	REG_OVERRIDE_SERIALIZABLE( MoveJointGesture, RaspiMoveJointGesture );
	REG_SERIALIZABLE(RaspiMoveJointGesture);
	RTTI_IMPL( RaspiMoveJointGesture, MoveJointGesture );

	int RaspiMoveJointGesture::m_CurrentPWMArmState = 0;

	bool RaspiMoveJointGesture::Execute( GestureDelegate a_Callback, const ParamsMap & a_Params )
	{
		if ( PushRequest( a_Callback, a_Params ) )
			ThreadPool::Instance()->InvokeOnThread(  DELEGATE( RaspiMoveJointGesture, MoveJointThread, Request *, this ), ActiveRequest() );
		return true;
	}

	bool RaspiMoveJointGesture::Abort()
	{
		return false;
	}

	void RaspiMoveJointGesture::MoveJointThread( Request * a_pReq )
	{
		try {
			if ( !m_bWiredPi )
			{
				m_bWiredPi = true;
				wiringPiSetup();
				softPwmCreate(m_PinNumber, 0, 50);
			}
			MoveArm();
		}
		catch( const std::exception & ex )
		{
			Log::Error( "RaspiMoveJointGesture", "Caught Exception: %s", ex.what() );
		}

		// now invoke the main thread to notify them the move is completed..
		ThreadPool::Instance()->InvokeOnMain( DELEGATE(RaspiMoveJointGesture, MoveJointDone, Request *, this ), a_pReq );
	}

	void RaspiMoveJointGesture::MoveArm()
	{
		if(PWMValue() > m_CurrentPWMArmState) //move arm upward
		{
			Log::Debug("RaspiMoveJointGesture", "Current Raspi Arm State %d", m_CurrentPWMArmState );
			Log::Debug("RaspiMoveJointGesture", "Moving Raspi Arm Upward to %d", PWMValue() );
			for(int i = m_CurrentPWMArmState ; i <= PWMValue(); i++)
			{
				softPwmWrite (m_PinNumber, i);
				delay(10);
				m_CurrentPWMArmState = PWMValue();
			}
			wiringPiSetup();
		}
		if(PWMValue() < m_CurrentPWMArmState) //move arm downward
		{
			Log::Debug("RaspiMoveJointGesture", "Current Raspi Arm State %d", m_CurrentPWMArmState );
			Log::Debug("RaspiMoveJointGesture", "Moving Raspi Arm Downward to %d", PWMValue() );
			for(int i = m_CurrentPWMArmState; i >= PWMValue() ; i--)
			{
				softPwmWrite (m_PinNumber, i);
				delay(10);
				m_CurrentPWMArmState = PWMValue();
			}
			wiringPiSetup();
		}

	}

	int RaspiMoveJointGesture::PWMValue(){
		return (  ( abs(m_fAngles.front()) % 360   )/360.0 ) * 100;
	}

	void RaspiMoveJointGesture::MoveJointDone( Request * a_pReq )
	{
		Status::SP spStatus(new Status());
		SelfInstance::GetInstance()->GetBlackBoard()->AddThing(spStatus);
		if ( PopRequest() )    // pop the request, if true is returned then start the next request..
		{
			ThreadPool::Instance()->InvokeOnThread(DELEGATE(RaspiMoveJointGesture, MoveJointThread, Request * , this), ActiveRequest());
		}
	}
	```

6. Build Intu on your Raspberry Pi with the following steps:

	```
	cd ~/intu/self-sdk-master
	scripts/build_raspi.sh
	```
	
7. Configure the `platform.json` file.

	1. run the following command in your terminal:
	
		```
		nano ~/intu/self-sdk-master/bin/raspi/etc/shared/platforms/raspi/platform.json
		```

    2. Locate the `m_Libs` variable and change it to read: 

		`"m_Libs":[ "platform_raspi", "camera_plugin", "move_joint_plugin", "workshop_five_plugin"]` 
    
8. Run Intu by executing the following two commands in your terminal:

	```
	cd ~/intu/self-sdk-master/bin/raspi
	./run_self.sh
	```

You have now added a gesture for moving a joint with Intu.  When you say, "Raise your right arm?" or "Lower your right arm" to the TJBot, it will move its arm. 



# <a name="appendix-pi">Appendix: Instructions for Running Intu on a Raspberry Pi (Without a Pre-Imaged SD Card)</a>
 
**Note:** Commands are assumed to be issued from **Terminal** on **Mac** or **PuTTY** on **Windows**. For **Windows** users, if you do not have **PuTTY** installed, you can download it using this [link](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). **Windows** users will also require a file management tool to copy files over a network between their local machine and the Raspberry Pi. You can use a stand-alone tool like **Filezilla**. **Filezilla** can be downloaded using this [link](https://filezilla-project.org/).

## 1. Set up the Wi-Fi connection for your Raspberry Pi

1.	Connect your Raspberry Pi to a power source. Connect an external keyboard, mouse, and monitor to your Raspberry Pi.

2.	You should see a window open on your monitor. Click on the **Wifi networks (w)** icon at the top of the window, select your network.

3.	Get the IP address of your Raspberry Pi.
	1.	Click on the black **Terminal** icon on the top left toolbar.
	2.	Type in **ifconfig** and hit Enter.
	3. Look for the **wlan0** section. The **inet addr** gives you the IP address of your Raspberry Pi (e.g. 10.0.1.2). 

4.	Ensure that your laptop is on the **same** network as your Raspberry Pi from this point onwards.
	
	For **Mac** users, open a new Terminal window, and connect to the Raspberry Pi using: `ssh pi@{ip_address}` (e.g. ssh pi@10.0.1.2)

	You will be prompted for a password. The default password for the Raspberry Pi is **raspberry**.

	For **Windows** users, open **PuTTY**, type in the Raspberry Pi's IP address, and when prompted, type in the username (**pi**) and password (**raspberry**) for the Raspberry Pi.

5. At this point, you can disconnect the external monitor, mouse and keyboard from the Raspberry Pi. **Do not disconnect the Raspberry Pi from its power source.**

6. Check that your laptop is on the same network as the Raspberry Pi. Reboot the Raspberry Pi by running `sudo reboot` in your SSH window. 

## 2. Install the Raspbian Operating System on your Raspberry Pi

1. Navigate to the [NOOBS Download page](https://www.raspberrypi.org/downloads/noobs/).

2. Click on the **Download ZIP** button under ‘NOOBS (offline and network install)’. **Note:** This may take an hour or more depending on your network speed. 

3. Extract the files from the zip to a local directory. 

4. Open the "Read Me" text file in the extracted folder and follow the instructions to format your SD card.

5. Once your SD card has been formatted, drag all the files within the extracted NOOBS folder and drop them onto the SD card.

6. The necessary files will then be transferred to your SD card.

7. When this process has finished, safely eject the SD card from your computer, remove the mini SD card from it, and insert it into your Raspberry Pi.

8. Connect your Raspberry Pi to a power source, and connect an external keyboard, mouse and monitor to your Raspberry Pi.

9.	You should see a window open on your monitor. Click on the **Wifi networks (w)** icon at the top of the window, select your network.

10. As this is the first time your Raspberry Pi and SD card have been used, you'll have to select an operating system and let it install. Select **Raspbian [RECOMMENDED]**.

11. At the bottom of your screen, you will see a **Language** panel for your keyboard. Select **English (US)**. 

12.	Click on the **Install** icon on the top left of the window. A **Confirm** window will appear; select **Yes**. The installation may take up to 30-40 mins. 

	**Note:** If your installation freezes at any point (as indicated by a frozen progress bar), **unplug** the Raspberry Pi from its power source and **plug it back in**, and repeat from step 8.

13.	Check that you are connected to the network. On the right of the Bluetooth icon in the top right corner of the screen, you will see a **successful connection** to your wifi network as shown by a **blue wifi icon**, or an **icon with two red crosses** if you have been **disconnected**. To reconnect, click on the two red crosses, select your network, and type in the password when prompted.

14. Get the IP address of your Raspberry Pi.
	1.	Click on the black **Terminal** icon on the top left toolbar.
	2.	Type in **ifconfig** and hit Enter. **Note:** If this command does not work, **unplug** the Raspberry Pi from its power source and **plug it back in**, and repeat this step.
	3. Look for the **wlan0** section. The **inet addr** gives you the IP address of your Raspberry (e.g. 10.0.1.2). 

15.	**Important:** Ensure that your laptop is on the **same** network as your Raspberry Pi from this point onwards.
	
	**For Mac users:**
	1. Open a new Terminal window, and connect to the Raspberry Pi using: `ssh pi@{pi's_IP_address}` (e.g. ssh pi@10.0.1.2)
	2. You will be prompted for the Raspberry Pi's password. The default password is **raspberry**.

	**For Windows users:** 
	
	1. Open a new PuTTY window and type in the Raspberry Pi's IP address. 
	2. You will be prompted for the Raspberry Pi's username and password. The default username is **pi** and default password is **raspberry**.

16. At this point, you can disconnect the external monitor, mouse and keyboard from the Raspberry Pi. 

17. Check that your laptop is on the same network as the Raspberry Pi. Reboot the Raspberry Pi by running `sudo reboot` in your SSH window. 


**Code Overview**

1. All classes in Intu inherit from the ISerializable interface. All objects can be deserialized from the Deserialize function and serialized from the Serialize function. All deserialization and serialization occurs from the body.json file found in the etc/profiles directory.

2. The flow of how this gesture gets executed has led up to working on the previous workshops. In Workshop 2, you have configured a Conversation service to have it respond to questions in a variety of ways. One thing that can be done in Conversation is to add an `[emote=show_laugh]` tag after a response. The text that gets returned from the Conversation service eventually makes its way to the SpeakingAgent. The SpeakingAgent will see the `[emote=show_laugh]` tag, and create an Emotion object and place that on the Blackboard. The agent that was developed in Workshop 3 subscribes to Emotion objects on start up. In the callback, the OnEmotion() function will execute an AnimateGesture skill.

3. All gestures inherit from the IGesture interface. Therefore, all gestures have a Start(), Stop(), Execute(), and Abort():
  
  * **Start():** This function is called from the GestureManager class. If the function returns false, then the GestureManager will not register the gesture.
  
  * **Stop():** This function is called when the GestureManager is stopped
  
  * **Execute():** The main implementation on how to carry out the execution of the gesture. If we look at the top of the cpp file, you will see two macros defined: REG_SERIALIZABLE and RTTI_IMPL. REG_SERIALIZABLE will serialize the object type to our system so we can get a handle on it using reflection, while RTTI_IMPL states that our implementation of the class WorkshopFiveGesture will override our base AnimateGesture class. The power of this allows us to have platform specific code to carry out execution of gestures while still keeping the core Intu platform agnostic. Therefore, when AnimateGesture is called to execute, our WorkshopFiveAnimation execute function will be called.
  
  * **Abort():** Will stop the execution of the gesture if the gesture is still in progress.

