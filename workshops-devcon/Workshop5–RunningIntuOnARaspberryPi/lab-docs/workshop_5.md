# Workshop 5 – Running Intu on a Raspberry Pi

In this workshop, you will assemble your own Raspberry Pi, which is a credit card-sized computer. You will then program an LED gesture for Intu, and run Intu off of your Raspberry Pi and see your LED gesture in action.

**Before you begin:** 

1. You must have a Mac or Windows laptop, and you must have completed Workshop 1: Say Hello!. After completing Workshop 1, you will have:

	1. Your own account, Organization, Group and Parent on the [Intu Gateway](rg-`gateway.mybluemix.net`)
	2. Your own [Intu Gateway](rg-`gateway.mybluemix.net`) credentials 

2. You must have the following to complete Workshop 5:
	* Raspberry Pi 3 with power cable
	* Anker Bluetooth Speaker with power cable and 3.5mm audio cable
	* USB Mini Microphone
	* PiCamera for the Raspberry Pi 3
	* Neopixel RGB LED
	* Monitor (with a HDMI connection)
	* Keyboard and Mouse (with USB connections)
	* An imaged 32 GB SD card

3. You will notice that Intu and Self are used interchangeably. Self is the technical name for Intu.  

**Notes:** 

1. If you **do not** have an imaged card from Devcon, please go to the **Appendix: Instructions for Running Intu on a Raspberry Pi (Without a Pre-Imaged SD Card)**

2. In this workshop, commands are issued from **Terminal** on **Mac** or **PuTTY** on **Windows**. For **Windows** users, if you do not have **PuTTY** installed, you can download it using this [link](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). **Windows** users will also require a file management tool to copy files over a network between their local machine and the Raspberry Pi. You can use a stand-alone tool like **Filezilla**, or you may prefer scp via Putty. **Filezilla** can be downloaded using this [link](https://filezilla-project.org/).

Complete the following tasks:

1. [Assembling the Raspberry Pi](#assembling-the-raspberry-pi)
2. [Set up the Wi-Fi connection for your Raspberry Pi](#set-up-the-wi-fi-connection-for-your-raspberry-pi)
3. [Download the Self SDK onto your computer and add in the code for the LED gesture](#download-the-self-sdk-onto-your-computer-and-add-in-the-code-for-the-led-gesture)
4. [Updating your Raspberry Pi with the LED gesture](#updating-your-raspberry-pi-with-the-led-gesture)
5. [Updating the body.json configuration](#updating-the-body.json-configuration)
6. [Run Intu on your Raspberry Pi](#run-intu-on-your-raspberry-pi)

## 1. Assembling the Raspberry Pi

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

4.	Now position your Raspberry Pi such that the **power cable** is on the **bottom**. Connect the **Cathode end** to **pin #3** on the **top row**. Then connect the **Anode end ** to **pin #4** on the **bottom row**. Refer to the image below for a fully assembled Raspberry Pi.

	![Assembled Raspberry Pi.](./anode_cathode.png?raw=true)
	
### E. Power

To power up your Raspberry Pi, connect the power cable to your Raspberry Pi as shown in the image below. 

![Power cable for Raspberry Pi.](./pi_charger.png?raw=true)

### F. Connecting the Raspberry Pi to an external monitor, keyboard and mouse

Connect your Raspberry Pi to an external monitor, keyboard and mouse as shown in the image below.

![Raspberry Pi and external connections.](./external_monitor_keyboard_to_pi.png?raw=true)

## 2. Set up the Wi-Fi connection for your Raspberry Pi

1. Insert your SD card into your Raspberry Pi if you have not done so already.

2. Connect your Raspberry Pi to a power source, and connect an external keyboard, mouse and monitor to your Raspberry Pi.

3. You should see a window open on your monitor. Sometimes it might so happen that your power strip might not work correctly. If your Pi does not start, plug it directly into a wall socket. Click on the **Wifi networks** icon ![wifi](./wifi.png?raw=true) at the top of the window, select your network (at DevCon, it will be **ROBOT_PED1**), and enter your password (**panda$123** for ROBOT_PED1).

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


## 3. Setting up your Raspberry Pi for Builds

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

## 4. Download the Self SDK onto your computer and add in the code for the LED gesture
**Note:** Download the Self SDK on both the Raspberry Pi and the laptop. We will use the laptop as the development machine and transfer the updated files to the Raspberry Pi.

### A. Download the Self SDK

1. [Download the Self SDK](https://github.com/watson-intu/self-sdk). Make sure the branch is master and click on download.
2. Create a new directory named **intu** in your **home** directory.

3. Unzip the **self-sdk-master.zip** file into **intu**, making sure that you retain the folder structure, i.e. your intu directory should now contain the unzipped **self-sdk-master** folder. This may take some time.

### B. Creating the LED gesture on OS X

1. If you do not have it installed already, download the trial version of the [CLion C++ IDE](https://www.jetbrains.com/clion/download/).

2. In **CLion**, select Open -> home directory -> intu -> self-sdk-master and click **OK**. 

	Note that a window may appear prompting you to open your project in a New Window or This Window. Select **New Window**. At the bottom of your CLion window, in the Problems tab, you will see the following Error, which you do not need to worry about:
	
	```
	Error: By not providing "FindSELF.cmake" in CMAKE_MODULE_PATH this project has asked CMake to find a package configuration file provided by "SELF", but CMake did not find one.
Could not find a package configuration file provided by "SELF" with any of the following names:
  SELFConfig.cmake   self-config.cmake
Add the installation prefix of "SELF" to CMAKE_PREFIX_PATH or set "SELF_DIR" to a directory containing one of the above files.  If "SELF" provides a separate development package or SDK, be sure it has been installed.
	```

 2i. Inside the CLion **self-sdk-master project**, right-click **examples**, select **New**, and select **Directory**. Type in **workshop_five** for the new directory name, and click **OK**.
 
 2ii. Right-click the `CMakeLists.txt` file in the **examples** directory, and click **Copy**. (If you are unsure of the directory you are in, look in the top-left navigation bar.)
  
 2iii. Right-click the **workshop_five** directory, and click **Paste**. This file helps to build the plugin for the the LED gesture.

 2iv. Open the `CMakeLists.txt` file in the **workshop_five** directory, and overwrite all of its contents with the following code:

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

3. Create a new directory inside **workshop_five** called **gestures**.

4. Locate the Workshop 5 code snippet files **to be filled in** in:
`self-docs/workshops-devcon/Workshop5–RunningIntuOnARaspberryPi/code-snippets/WorkshopFiveGesture_start/`

5. Copy the `WorkshopFiveGesture.cpp` and the `WorkshopFiveGesture.h` files and paste them into the **gestures** directory that you created.

	* Open the `WorkshopFiveGesture.cpp` file, which contains the following functions that enable the gesture you'll create:

	* The Execute, Abort, AnimateThread and AnimateDone functions are already completely built out.

	* In the next step, you will build out the **DoAnimateThread** function using the example code provided.

6. In **self-sdk-master/docs/workshops-devcon/5/code-snippets/WorkshopFive_Snippets**, you will see the `WorkshopFiveCodeSnippets.txt` file. Open this file and find the **DoAnimateThread** function.

7. Copy the entire contents of `WorkshopFiveCodeSnippets.txt ` for the **DoAnimateThread()** function. Paste this inside the function body **{}** of **DoAnimateThread()** in `WorkshopFiveGesture.cpp` located in your **gestures** directory, directly above the line of code which reads: `Log::Debug("WorkshopFiveGesture", "in DoAnimateThread");`. The code which you need is displayed below for completeness; however, it is **not** recommended for you to copy it from here due to formatting issues.


  ```
    std::vector<AnimationEntry> anims;
    for(size_t i=0;i<m_Animations.size();++i)
    {
        anims.push_back( m_Animations[i] );
    }

    if ( anims.size() > 0 )
    {
        srand( (unsigned int)Time().GetMilliseconds() );
        const AnimationEntry & entry = anims[ rand() % anims.size() ];
        Log::Debug( "WorkshopFiveGesture", "Gesture %s is running behavior %s.", m_GestureId.c_str(), entry.m_AnimId.c_str() );

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
    }
    else
    {
        Log::Warning( "WorkshopFiveGesture", "No valid animations found for gesture %s", m_GestureId.c_str() );
    }
    
    ```

8. Save your changes (**Cmd + S**). 

### C. Creating the LED gesture on Windows

1.	Open up File Explorer and navigate to your **home** directory. This should be: **C:\Users\username** ("username" should read your name)

2. In your home directory, create a new directory called **workshop_five**, and a second directory called **gestures** inside **workshop_five**.

3. Navigate to **self-docs/workshops-devcon/Workshop5–RunningIntuOnARaspberryPi/code-snippets/WorkshopFiveGesture_start/**, where you will find the following two files:
	
	`WorkshopFiveGesture.cpp` 
	
	`WorkshopFiveGesture.h`
		
4. Copy `WorkshopFiveGesture.cpp` and `WorkshopFiveGesture.h` from **WorkshopFiveGesture_Start/** to the **gestures** directory which you just created.
	
5. Now navigate to **self-docs/workshops-devcon/Workshop5–RunningIntuOnARaspberryPi/code-snippets/WorkshopFiveGesture_Snippets** and locate the `WorkshopFiveCodeSnippets.txt` file and find the **DoAnimateThread** function.

6. Copy the entire contents of `WorkshopFiveCodeSnippets.txt ` for the **DoAnimateThread()** function. Paste this inside the function body **{}** of **DoAnimateThread()** in `WorkshopFiveGesture.cpp` located in your **gestures** directory, directly above the line of code which reads: `Log::Debug("WorkshopFiveGesture", "in DoAnimateThread");`. The code which you need is displayed below for completeness; however, it is **not** recommended for you to copy it from here due to formatting issues.


  ```
    std::vector<AnimationEntry> anims;
    for(size_t i=0;i<m_Animations.size();++i)
    {
        anims.push_back( m_Animations[i] );
    }

    if ( anims.size() > 0 )
    {
        srand( (unsigned int)Time().GetMilliseconds() );
        const AnimationEntry & entry = anims[ rand() % anims.size() ];
        Log::Debug( "WorkshopFiveGesture", "Gesture %s is running behavior %s.", m_GestureId.c_str(), entry.m_AnimId.c_str() );

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
    }
    else
    {
        Log::Warning( "WorkshopFiveGesture", "No valid animations found for gesture %s", m_GestureId.c_str() );
    }
    
    ```
 
7.	Navigate to the **workshop_five** directory you created in your home directory, and create a new text file called `CMakeLists.txt`. 

8. Open this file and add in the following lines of code:

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
	
9. Save your changes (**Ctrl + S**). 

## 5. Updating your Raspberry Pi with the LED gesture

1.	Copy the **workshop_five** directory from your local machine over to your Raspberry Pi. 
   
   **For Mac users:** 
   1. Open a new terminal window and navigate to the **examples** directory (the parent directory of workshop_five) by running: `cd intu/self-sdk-master/examples`
   2. Run: `scp -r workshop_five pi@{pi's_ip_address}:~/intu/self-sdk-master/examples`

	**For Windows users:** 
	
   1. Open Filezilla and connect to your Raspberry Pi. 
		1. In the **Host** field, specify your Raspberry Pi's IP address.
		2. In the **Username** field, specify your Raspberry Pi's username (**pi**).
		3. In the **Password** field, specify your Raspberry Pi's password (**raspberry**).
		4. In the **Port** field, specify **22**.  	
	2. Navigate to **intu/self-sdk-master/examples/** on the **Remote site** side of the screen.
	
	3. Navigate to the **intu/self-sdk-master/examples/** directory on the **Local site** side of the screen.
	
	4. Drag your **examples** directory from the **Local site** to the **Remote site** to copy the directory across to your Raspberry Pi. You can monitor the progress of the transfer in the panel located at the bottom of the Filezilla screen.

2.	SSH to the Raspberry Pi in a new SSH window (Terminal for Mac or PuTTY for Windows):

	1. Run:`ssh pi@{pi's_ip_address}`	
  	2. Run: `cd /home/pi/intu/self-sdk-master/examples`

3.	Edit the `CMakeLists.txt` file in the examples directory you're currently in.

	1. Run: `nano CMakeLists.txt`
	2. Carefully add the following line at the end of the file:		`add_subdirectory(workshop_five)`
	3. Save your changes to the `CMakeLists.txt` file. 
   		1. Use **Ctrl + X** to `Exit`.
   		2. When prompted with: `Save modified buffer (ANSWERING "No" WILL DESTROY CHANGES) ? `, type **Y** for **Yes**. 
   		3. When prompted with: `File Name to Write: CMakeLists.txt`, hit **Return** or **Enter** to finalise your changes.
   		
4. Build Self on your Raspberry Pi with the following steps:

	1.	Navigate into the **self-sdk-master** directory on your Raspberry Pi: `cd self-sdk-master`
	
	2.	Mark the build script as executable by running: `chmod +x scripts/build_raspi.sh`
	
	3. Run: `scripts/clean.sh` 
	
	4. Run: `scripts/build_raspi.sh`

## 6. Updating the `body.json` configuration

### A. Retrieving the credentials for your Organization in the Intu Gateway

1. [Log in to the Intu Gateway](https://rg-gateway.mybluemix.net/). 

2. Click on **VIEW CREDENTIALS** in the left hand navigation bar.

3. Select your Organization and Group in the top Filter by menu, and click on the **Get Credentials** box.

4. Create a `config.json` file in case it isn't present on the **Raspberry Pi** in **self-sdk-master/bin/raspi** and paste the credentials obtained from the gateway in step 3.

### B. Configuring your `body.json` file
1. The body.json file acts as an configuration for all the various parts of the INTU platform. Here we will configure it to allow self to pick up on the workshop plugin we added above. In this section we are expecting the edits to the body.json to be on the **Raspberry Pi** we have found vim to work well over SSH but editing directly in the NOOBs GUI works well too.

    1. On your Raspberry Pi open your `body.json` file located at `self-sdk-master/bin/raspi/etc/profile/body.json`

    2. Locate the `m_Libs` variable, and change it to read: 
    
    	`"m_Libs":["workshop_five_plugin"]` 
    
    	**If there are any addional values here like "platfrom_linux" DELETE them all. You should only have 2 values under `m_libs`**

    3. Save your changes and close the file.

### C. Building the Self SDK on your Raspberry Pi

1. In your current (or a new) SSH session to the Raspberry Pi, navigate to the **self-sdk-master** directory: `cd self-sdk-master`
	
2.	Mark the build script as executable by running: 
`chmod +x scripts/build_raspi.sh`

3.	Now build the Self SDK by running: `scripts/build_raspi.sh`
	
	**Note:** If you have any build errors, run: `scripts/clean.sh` and then rerun: `scripts/build_raspi.sh`

## 7. Run Intu on your Raspberry Pi

Run Intu on your Raspberry Pi by completing the following steps in your terminal window. 

**Note:** The following steps will need to be repeated each time you power up your Raspberry Pi (i.e. unplug and plug back in the power source to your Raspberry Pi).

1.	If you have a HDMI cable plugged into your Raspberry Pi, verify that the sound is set to **analog**. This can be done by right clicking the **speaker icon** at the top right hand corner of the Raspberry Pi's homescreen, and selecting **analog**. 

2.	Verify that you have a microphone and speaker plugged into your Raspberry Pi. Note that your speaker may need to be charged before use. Make sure that it is turned on before proceeding with the next step.

3.	Navigate to the **raspi** directory using: `cd /home/pi/self/self-sdk-master/bin/raspi`.

4.	Run: `./run_self.sh`

When Intu starts, it will give a notification like "Ah, I feel so much better". You have now added a gesture for the LED light.  When you say, "Can you laugh?" or "Tell me a joke" to the robot, the LED light should blink, i.e. you have added the blinking of the LED to Intu as a gesture. 


### What did we learn?
When Intu is asked "can you laugh" or "tell me a joke" and the Blackboard receives a [emote=show_laugh], how does Intu know that the LED gesture should be executed?

It is from the configuration file `raspi.anims`, in `Intu/self-sdk-master/bin/raspi/etc/gestures`. (More to come).

# Workshop 5 Extra Credit – Using Intu to Make a TJBot's Arm Wave

Now that you are familiar with the Raspberry Pi we can take this to the next stage and start working with the servo motor. You will program a Move Joint gesture for Intu, and run Intu off of your Raspberry Pi and see your TJBot's arm in action.

**Before you begin:** 

1. You must have a Mac or Windows laptop, and you must have completed Workshop 1: Say Hello!. After completing Workshop 1, you will have:
2. Your own account, Organization, Group and Parent on the [Intu Gateway](rg-`gateway.mybluemix.net`)
3. Your own [Intu Gateway](rg-`gateway.mybluemix.net`) credentials 
4. You must have the following to complete Workshop 5 Extra Credit:
    1. Raspberry Pi 3 with power cable
    2. Anker Bluetooth Speaker with power cable and 3.5mm audio cable
    3. USB Mini Microphone
    4. Monitor (with a HDMI connection)
    5. Keyboard and Mouse (with USB connections)
    6. An imaged 32 GB SD card (16 GB would also work)
    7. The basic workshop 5
5. *Note:* You will notice that Intu and Self are used interchangeably. Self is the code name for Intu.  

## 1. Adding the Servo Motor

### A. Connecting the Servo Motor

As part of this lab we will be using the Tower Pro SG90 micoservo. You can see the pin out of this servo motor [here](./SG90Servo.pdf). You will need to connect the servo motor to the Raspi Board as below:

![Board Layout for Servo](./sevo_pin_layout.png?raw=true)

## 2. Creating  the Waving Arm Gesture with INTU
1. To create this in the Raspberry Pi we are going to recreate the folder structure under **examples** that is found in this repo under self-sdk/docs/workshops-devcon/5/code-snippets/TJBotWave_Final. 
2. We have found the CLion IDE to be an nice environment for this type of work. If you do not have it installed already, download the trial version of the [CLion C++ IDE](https://www.jetbrains.com/clion/download/). (Alternatively, You may also use your favorite text editor and just follow the steps below.)
3. Inside the **self-sdk-master** project navigate to the **examples** directory. Inside here make an new directory called **move_arm_joint**
4. Edit the `examples/CMakeLists.txt` file to look like:
	```
	include_directories(".")
	
	add_subdirectory(move_arm_joint)
	add_subdirectory(sensor)
	add_subdirectory(workshop_five)
	```
5. Next we will move the files from **self-sdk/docs/workshops-devcon/5/code-snippets/TJBotWave_Final/examples/move_arm_joint/** into the **move_arm_joint** directory. There should be 3 new files: CMakeLists.txt, RaspiMoveJointGesture.cpp, and RaspiMoveJointGesture.h now inside the **move_arm_joint** folder.
6. Take a few moments to look at all three of these files as they are the core of the code that acts as a plugin to INTU allowing it to control the Raspberry Pi's GPIO pins (with PWM) allowing the arm to move. 
 
## 3. Updating your Raspberry Pi with the move joint gesture

1. Copy the entire **examples/move_arm_joint** directory from your local machine over to your Raspberry Pi.

    **For Mac users:** 
    1. Open a new terminal window and navigate to the **examples** directory (the parent directory of move_arm_joint) by running: `cd self/self-sdk-master/examples`
    2. Run: `scp -r move_arm_joint pi@{IPaddress}:~/self/self-sdk-master/examples`

    **For Windows users:** 

    1. Open Filezilla and connect to your Raspberry Pi. 
    2. In the **Host** field, specify your Raspberry Pi's IP address.
    3. In the **Username** field, specify your Raspberry Pi's username (**pi**).
    4. In the **Password** field, specify your Raspberry Pi's password (**raspberry**).
    5. In the **Port** field, specify **22**.  	
    6. Navigate to **self/self-sdk-master/examples** on the **Remote site** side of the screen.
    7.Navigate to the **self/self-sdk-master/examples** directory on the **Local site** side of the screen.
    8.Drag your **move_arm_joint** directory from the **Local site** to the **Remote site** to copy the directory across to your Raspberry Pi. You can monitor the progress of the transfer in the panel located at the bottom of the Filezilla screen.
2. Build Self on your Raspberry Pi with the following steps:

	1.	Navigate into the **self-sdk-master** directory on your Raspberry Pi: `cd self-sdk-master`
	
	2.	Mark the build script as executable by running: `chmod +x scripts/build_raspi.sh`
	
	3. Run: `scripts/clean.sh` 
	
	4. Run: `scripts/build_raspi.sh`

## 4. Configuring your `body.json` file
1. We will add a few extra parameters to the body.json:

    1. On your Raspberry Pi open your `body.json` file located at `self-sdk-master/bin/raspi/etc/profile/body.json`

    2. Locate the `m_Libs` variable, and change it to read: 
    
    	`"m_Libs":["move_joint_plugin", "workshop_five_plugin"]` 
    
    	**If there are any addional values here like "platfrom_linux" DELETE them all. You should only have 2 values under `m_libs`**

## 5. Go ahead and rebuild and rerun self:
1. Rebuild:

	1. Navigate into the **self-sdk-master** directory on your Raspberry Pi: `cd self-sdk-master`
	
	2. Mark the build script as executable by running: `chmod +x scripts/build_raspi.sh`

	3. Run: `scripts/clean.sh` 

	4. Run: `scripts/build_raspi.sh`

2. Rerun:

	1. Navigate to the **raspi** directory using: `cd /home/pi/self/self-sdk-master/bin/raspi`.

	2. Run: `./run_self.sh`


You have now added a gesture for moving a joint with INTU.  When you say, "Raise your right arm?" or "Lower your right arm" to the robot, the TJBot should move it arm. 


### Wait, wait, wait, but how does it work?
When Intu is asked "Raise your right arm?" Blackboard receives a [r_hand_raise] by looking at the configuration file located in self-sdk/tree/develop/docs/workshops-devcon/5/code-snippets/TJBotWave_Reference/. Under this folder you will find **two** raspi-joints.json files. First an internal graph sturcture will traverse for a phrase mathcing for "right arm" then the instance will decide that the r_hand_raise gesture is the matching the spoken input (See the file located at: self/self-sdk-master/bin/raspi/etc/shared/self_requests.json). Then under **skills/rasip-joints.json** you will see that r_hand_raise maps to a gesture with the same name, r_hand_raise. Under **gestures/rasip-joints.json** we will see concretely how we parameterize the arm movement. 

It is from the configuration file `raspi.anims`, in `self/self-sdk-master/bin/raspi/etc/gestures`. (More to come).


# Extra Extra Credit: Teaching your TJBot to wave
1. In this section we will explore how to make your TJBot go through and interaction like:
```
Human: "Wave to the crowd"
TJBot: "I do not know how to Wave"
Human: "Raise your left arm"
TJBot: [you see physical action perfored]
Human: "Lower your left arm"
TJBot: [you see physical action perfored]
Human: "That is how you wave"
TJBot: "I now know how to wave"
Human: "Wave to the crowd"
TJBot: [you see BOTH physical action perfored]
```
2. To do this all we need to do is add Alchemy into your registered services on the gateway and the update your body.json and config.json like you did above.

	1. The first thing is to go to [Bluemix](https://console.ng.bluemix.net/catalog/) and create an instance of Alchemy. Once you have done that grab the "apikey":
	 ![Getting the Alchemy API Key.](./FindingAlchemyOnBluemix.png?raw=true)

	2. The next step is to update your subscribed services on the [Intu Gateway](rg-gateway.mybluemix.net). Once you login use the left hand bar to navigate to MANAGE->Services like:
    	![Finding the service managment page.](./GoingToManageServices.png?raw=true)

	3. The final step is to click "Add Service" and fill it in with:

	```
	SERVICE NAME: AlchemyV1
	USER ID: Your_Alchemy_API_Key
	SERVICE ENDPOINT: http://gateway-a.watsonplatform.net/calls
	```
	**LEAVE THE PASSWORD FIELD BLANK**
	
	It will look like this when you are done:
	![Filling in the Alchemy API Key.](./FillInAlchemy.png?raw=true)
	    

3. Go ahead and rebuild and rerun self:
	1. Rebuild:

		1. Navigate into the **self-sdk-master** directory on your Raspberry Pi: `cd self-sdk-master`
		
		2. Mark the build script as executable by running: `chmod +x scripts/build_raspi.sh`
	
		3. Run: `scripts/clean.sh` 
	
		4. Run: `scripts/build_raspi.sh`

	2. Rerun:
	
		1. Navigate to the **raspi** directory using: `cd /home/pi/self/self-sdk-master/bin/raspi`.
	
		2. Run: `./run_self.sh`
	
4. Now try going through the below interactions with your TJBot. If all goes well you can now teach it more complex interactions. Congratulations on completing this workshop!

```
Human: "Wave to the crowd"
TJBot: "I do not know how to Wave"
Human: "Raise your right arm"
TJBot: [you see physical action perfored]
Human: "Lower your right arm"
TJBot: [you see physical action perfored]
Human: "That is how you wave"
TJBot: "I now know how to wave"
Human: "Wave to the crowd"
TJBot: [you see BOTH physical action perfored]
```

# Appendix: Instructions for Running Intu on a Raspberry Pi (Without a Pre-Imaged SD Card)
 
**Note:** Commands are assumed to be issued from **Terminal** on **Mac** or **PuTTY** on **Windows**. For **Windows** users, if you do not have **PuTTY** installed, you can download it using this [link](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). **Windows** users will also require a file management tool to copy files over a network between their local machine and the Raspberry Pi. You can use a stand-alone tool like **Filezilla**. **Filezilla** can be downloaded using this [link](https://filezilla-project.org/).

## 1. Set up the Wi-Fi connection for your Raspberry Pi

1.	Connect your Raspberry Pi to a power source, and connect an external keyboard, mouse and monitor to your Raspberry Pi.

2.	You should see a window open on your monitor. Click on the **Wifi networks (w)** icon at the top of the window, select your network (at DevCon, it will be **ROBOT_PED1**), and enter your password (**panda$123**).

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

9.	You should see a window open on your monitor. Click on the **Wifi networks (w)** icon at the top of the window, select your network (at DevCon, it will be **ROBOT_PED1**), and enter your password (**panda$123**).

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

## 3. Setting up your Raspberry Pi for Builds

**Note:** If any step below fails or errors, run: `sudo apt-get update`, then repeat the step.

1.	Open up a new browser window on your laptop and download [**Anaconda 4.2.0 For Linux Python 2.7 version**](https://www.continuum.io/downloads).

	**Make sure you download the correct version.** You need the **Linux** version regardless of the operating system that you have. Windows users may have to right click and select **Save as** to save the download locally.
		

2.	Copy Anaconda from your laptop over to the Raspberry Pi. You will be prompted for the username (**pi**) and/or password (**raspberry**) for the Raspberry Pi.

	**For Mac users:**
	
	1. Navigate to the directory where you downloaded Anaconda on your local machine. The file should be named: `Anaconda3-4.2.0-Linux-x86.sh`.
	
	2. Run: `scp Anaconda2-4.2.0-Linux-x86.sh pi@{pi's_IP_address}:/home/pi` 

	**For Windows users:**
	
	1. Open Filezilla and connect to your Raspberry Pi. 
    	1. In the **Host** field, specify your Raspberry Pi's IP address.
		2. In the **Username** field, specify your Raspberry Pi's username (**pi**).
		3. In the **Password** field, specify your Raspberry Pi's password (**raspberry**).
		4. In the **Port** field, specify **22**. 
	
	2. In the **Local site** side of the screen, navigate to the `Anaconda2-4.2.0-Linux-x86.sh` file.
	
	3.	In the **Remote site** side of the screen, navigate to the directory: **/home/pi**
	
	4.	Click on the file `Anaconda2-4.2.0-Linux-x86.sh` on the **Local site** side of the screen and drag it to the **Remote site** side of the screen. You can monitor the progress of the transfer in the panel located at the bottom of the Filezilla screen.

3.	Install Anaconda on your Raspberry Pi and set up the qiBuild.
	
	1. In a new Terminal/PuTTY window, SSH into your Raspberry Pi: `ssh pi@{ip_address}`. You will be prompted for the username (**pi**) and/or password (**raspberry**) for the Raspberry Pi.
	
	2.	Run: `bash Anaconda2-4.2.0-Linux-x86.sh`
	
	3. Follow the steps on the screen to install Anaconda. When you get to the license, keep hitting **Enter** to jump to the bottom. Type **yes** to approve the license.
	
	4.	Hit **Enter** to install Anaconda in the default location. **Note**: It may take a while for the progress to update, and if you get the following error, please ignore it. Proceed with the next step.

		```
Anaconda2-4.2.0-Linux-x86.sh: line 484: /home/pi/anaconda2/pkgs/python-3.5.2-0/bin/python: cannot execute binary file: Exec format error
ERROR:
cannot execute native linux-32 binary, output from 'uname -a' is:
Linux raspberrypi 4.4.21-v7+ #911 SMP Thu Sep 15 14:22:38 BST 2016 armv7l GNU/Linux

		```

	5. Once Anaconda has successfully installed, run: `sudo apt-get install python-pip cmake` 
	
		**Note:** If this fails, run `sudo apt-get update` and then rerun: `sudo apt-get install python-pip cmake`

	6.	Run: `sudo pip install qibuild`
 

4.	Install the wiringPi library on the Raspberry Pi.
	
	1. In a new Terminal/PuTTY window, SSH into your Raspberry Pi: `ssh pi@{ip_address}`. You will be prompted for the username (**pi**) and/or password (**raspberry**) for the Raspberry Pi.
	
	2.	Navigate to your Raspberry Pi's **home directory** by running: `cd /home/pi` 
	
	3.	Run: `git clone git://git.drogon.net/wiringPi`
	
	4.	Now navigate into the wiringPi directory by running: `cd wiringPi/`
	
	5.	Run: `./build`

	You should see a list of classes compiled and "All Done" at the end.
	
5. Finally, in a new Terminal/PuTTY window, SSH into your Raspberry Pi, navigate to its home directory by running: `cd /home/pi` and in here, create a new directory called **self** by running: `mkdir self`. This directory will be used in the steps below.

## 4. Download the Self SDK and build Self on your Raspberry Pi


### A. Download the Self SDK

1. [Download the Self SDK](https://github.com/watson-intu/self-sdk). Click on the **download icon** next to the default **master** branch selected.

2. Copy the zip file from your local machine across to the Raspberry Pi.

	**For Mac users:**

	1. In a Terminal window, navigate to the directory where you downloaded the zip file, and copy it across to the newly created **self** directory on the Raspberry Pi using: `scp self-sdk-master.zip pi@{IPaddress}:/home/pi/self/`
	
	**For Windows users:**
 
 1. Open Filezilla and connect to your Raspberry Pi.
  
    	1. In the **Host** field, specify your Raspberry Pi's IP address.
		
		2. In the **Username** field, specify your Raspberry Pi's username (**pi**).
		
		3. In the **Password** field, specify your Raspberry Pi's password (**raspberry**).
		
		4. In the **Port** field, specify **22**. 

	2. In the **Local site** side of the screen, navigate to the `self-sdk-master.zip` file.
	
	3.	In the **Remote site** side of the screen, navigate to the directory: **/home/pi/self**
	
	4.	Click on the file `self-sdk-master.zip` on the **Local site** side of the screen and drag it to the **Remote site** side of the screen. You can monitor the progress of the transfer in the panel located at the bottom of the Filezilla screen.

3. Unzip the `self-sdk-master.zip` file into the **self** directory of your Raspberry Pi. 
 	
 	1.	Navigate to the **self** directory on your Raspberry Pi. You can open a new Terminal/PuTTY window as before, SSH into your Raspberry Pi, and run: `cd /home/pi/self`. If your prompt reads: `pi@raspberrypi:~/self $`, this confirms that you are in the **self** directory.
	
	2.	Run the following command: `unzip self-sdk-master.zip`

4. Build Self on your Raspberry Pi with the following steps:

	1.	Navigate into the **self-sdk-master** directory on your Raspberry Pi: `cd self-sdk-master`
	
	2.	Mark the build script as executable by running: `chmod +x scripts/build_raspi.sh`
	
	3. Run: `scripts/clean.sh` 
	
	4. Run: `scripts/build_raspi.sh`

###You are now ready to proceed from **Section 3: Download the Self SDK onto your computer and add in the code for the LED gesture.** 

As you have already downloaded the `self-sdk-master.zip` file, your first step will be **Section 3.A.2:** Create a new directory named **intu** in your **home** directory
.

**Code Overview**

1. All classes in Intu inherit from the ISerializable interface. All objects can be deserialized from the Deserialize function and serialized from the Serialize function. All deserialization and serialization occurs from the body.json file found in the etc/profiles directory.

2. The flow of how this gesture gets executed has led up to working on the previous workshops. In Workshop 2, you have configured a Conversation service to have it respond to questions in a variety of ways. One thing that can be done in Conversation is to add an `[emote=show_laugh]` tag after a response. The text that gets returned from the Conversation service eventually makes its way to the SpeakingAgent. The SpeakingAgent will see the `[emote=show_laugh]` tag, and create an Emotion object and place that on the Blackboard. The agent that was developed in Workshop 3 subscribes to Emotion objects on start up. In the callback, the OnEmotion() function will execute an AnimateGesture skill.

3. All gestures inherit from the IGesture interface. Therefore, all gestures have a Start(), Stop(), Execute(), and Abort():
  
  * **Start():** This function is called from the GestureManager class. If the function returns false, then the GestureManager will not register the gesture.
  
  * **Stop():** This function is called when the GestureManager is stopped
  
  * **Execute():** The main implementation on how to carry out the execution of the gesture. If we look at the top of the cpp file, you will see two macros defined: REG_SERIALIZABLE and RTTI_IMPL. REG_SERIALIZABLE will serialize the object type to our system so we can get a handle on it using reflection, while RTTI_IMPL states that our implementation of the class WorkshopFiveGesture will override our base AnimateGesture class. The power of this allows us to have platform specific code to carry out execution of gestures while still keeping the core Intu platform agnostic. Therefore, when AnimateGesture is called to execute, our WorkshopFiveAnimation execute function will be called.
  
  * **Abort():** Will stop the execution of the gesture if the gesture is still in progress.



