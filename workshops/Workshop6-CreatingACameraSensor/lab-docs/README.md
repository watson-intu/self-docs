# Workshop 6: Creating a camera sensor.

In this workshop, you create a camera sensor. Sensors take in data from the outside world to process. The camera sensor will aim to take in visual data from the environment, ready to be processed on Intu. 

**Before you begin:** You must have a **Mac**, and you must have completed [Workshop 1 --  Saying Hello!](../../../workshops/Workshop1-SayingHello/lab-docs/README.md). You will notice that Intu and Self are used interchangeably. Self is the technical name for Intu.

Complete the following tasks:

1. [Understanding some Intu terminology](#understanding-some-intu-terminology)
2. [Building the Intu SDK](#building-the-intu-sdk)
3. [Creating a camera sensor](#creating-a-camera-sensor)
4. [Configuring your Intu instance to include the camera sensor](#configuring-your-Intu-instance-to-include-the-camera-sensor)


## 1. <a name="understanding-some-intu-terminology">Understanding some Intu terminology</a>

Before you create a camera sensor, become familiar with the following terminology:

  * **Blackboard**: The central message broker on which all of the agents post data and listen for incoming data.
  
  * **Publish**: To push data onto the blackboard under a particular topic. 
  
  * **Subscribe**: Subscribing to a topic on the blackboard means to listen to the blackboard and wait for any other agents to post to the blackboard under a particular topic.
  
  * **Topic**: A channel to which Agents publish and subscribe.

## 2. <a name="building-the-intu-sdk">Building the Intu SDK</a>


**Before you begin**:

1. Create a new directory named **intu** in a directory of your choosing.
2. In your `intu` directory, [download or clone the Intu SDK](../../../installation/compiling.md/#files). 


### A. Preparing for OS X

You will need to have **CMake** and **qiBuild** installed on your Mac to run this workshop. For instructions on how to install these, [click here](../../../installation/compiling.md/#setup).
   
### B. Building the Intu SDK for OS X

Build the **Intu SDK**.  [Click here](../../../installation/compiling.md/#osx) for instructions on how to do so.

## 3. <a name="creating-a-camera-sensor">Creating a camera sensor</a>

### A. Preparing your directories and files for the camera sensor

1. If you do not have it installed already, download the trial version of the [CLion C++ IDE](https://www.jetbrains.com/clion/download/).

2. In **CLion**, select Open -> home directory -> intu -> self-sdk-master and click **OK**. 

	Note that a window may appear prompting you to open your project in a New Window or This Window. Select **New Window**. At the bottom of your CLion window, in the Problems tab, you will see the following Error, which you do not need to worry about:
	
	```
	Error: By not providing "FindSELF.cmake" in CMAKE_MODULE_PATH this project has asked CMake to find a package configuration file provided by "SELF", but CMake did not find one.
Could not find a package configuration file provided by "SELF" with any of the following names:
  SELFConfig.cmake   self-config.cmake
Add the installation prefix of "SELF" to CMAKE_PREFIX_PATH or set "SELF_DIR" to a directory containing one of the above files.  If "SELF" provides a separate development package or SDK, be sure it has been installed.
	```

	2i. Inside the CLion **self-sdk-master project**, right-click **examples**, select **New**, and select **Directory**. Type in **workshop_six** for the new directory name, and click **OK**.
 
	2ii. Right-click the `CMakeLists.txt` file in the **examples** directory, and click **Copy**. (If you are unsure of the directory you are in, look in the top-left navigation bar.)
  
	2iii. Right-click the **workshop_six** directory, and click **Paste**. This file helps to build the plugin for the camera sensor.

	2iv. Return to the **examples** directory, open the `CMakeLists.txt` file, and add the following line: `add_subdirectory(workshop_six)` at the end. Your file contains the following three lines:

  ```
    include_directories(".")

    add_subdirectory(sensor)
    add_subdirectory(workshop_six)
  ```
	2v. Open the `CMakeLists.txt` file in the **workshop_six** directory, and overwrite its content with this code:

 	```
	include_directories(.)

	file(GLOB_RECURSE SELF_CPP RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "*.cpp")
	qi_create_lib(workshop_six_plugin SHARED ${SELF_CPP})
	qi_use_lib(workshop_six_plugin self wdc OPENCV2_CORE OPENCV2_HIGHGUI)
	qi_stage_lib(workshop_six_plugin)
  	```

3. Create a new directory called **sensors** in the **workshop_six** directory.

4. Right click on the newly created **sensors** directory and select **New** ==> **C/C++ Source File**.  Name this file **WorkshopSixSensor**.  Select the checkbox for *Create an associated header*. Click ok.

5. In the **WorkshopSixSensor.cpp** file paste the following code:

	```
	/**
	* Copyright 2016 IBM Corp. All Rights Reserved.
	*
	* Licensed under the Apache License, Version 2.0 (the "License");
	* you may not use this file except in compliance with the License.
	* You may obtain a copy of the License at
	*
	*      http://www.apache.org/licenses/LICENSE-2.0
	*
	* Unless required by applicable law or agreed to in writing, software
	* distributed under the License is distributed on an "AS IS" BASIS,
	* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	* See the License for the specific language governing permissions and
	* limitations under the License.
	*
	*/
	
	#include "WorkshopSixSensor.h"
	
	REG_SERIALIZABLE(WorkshopSixSensor);
	RTTI_IMPL(WorkshopSixSensor, Camera);
	
	#ifndef _WIN32
	REG_OVERRIDE_SERIALIZABLE(Camera, WorkshopSixSensor);
	#endif
	
	//  OnStart() will create and subscribe a camera object to the Blackboard on a background thread.
	bool WorkshopSixSensor::OnStart()
	{
	    Log::Debug("WorkshopSixSensor", "Starting Camera!");
	    m_StopThread = false;
	    m_ThreadStopped = false;
	    return true;
	}
	
	//  OnStop() when called will kill any camera thread that is running ensuring that the process is stopped.
	bool WorkshopSixSensor::OnStop()
	{
	    Log::Debug("WorkshopSixSensor", "Stopping MacCamera!");
	    m_StopThread = true;
	    while (!m_ThreadStopped)
	        tthread::this_thread::yield();
	    return true;
	}
	
	//  CaptureVideo() first checks that the thread is still live. If the thread is, then the frame object is then encoded
	//  into the memory buffer as a JPEG. This then sends the buffered data.
	void WorkshopSixSensor::CaptureVideo(void * arg)
	{
	
	}
	
	//  OnPaused() will increment the variable Paused, each time it is called.
	void WorkshopSixSensor::OnPause()
	{
	    m_Paused++;
	}
	
	//  OnResume() Will decrease the variable Paused each time it is called.
	void WorkshopSixSensor::OnResume()
	{
	    m_Paused--;
	}
	
	//  SendingData will check to see if the frame has been paused. This function should call only call the SendData()
	//  If variable m_Paused is greater than zero.
	void WorkshopSixSensor::SendingData(VideoData * a_pData)
	{
	
	}
	```


6. In the **WorkshopSixSensor.h** file, paste the following code:

	```
	/**
	* Copyright 2016 IBM Corp. All Rights Reserved.
	*
	* Licensed under the Apache License, Version 2.0 (the "License");
	* you may not use this file except in compliance with the License.
	* You may obtain a copy of the License at
	*
	*      http://www.apache.org/licenses/LICENSE-2.0
	*
	* Unless required by applicable law or agreed to in writing, software
	* distributed under the License is distributed on an "AS IS" BASIS,
	* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	* See the License for the specific language governing permissions and
	* limitations under the License.
	*
	*/
	
	#ifndef SELF_WORKSHOP_SIX_SENSOR_H
	#define SELF_WORKSHOP_SIX_SENSOR_H
	
	#include "sensors/Camera.h"
	#include "SelfInstance.h"
	#include "utils/TimerPool.h"
	
	#ifndef _WIN32
	#include <iostream>
	#include <opencv2/highgui/highgui.hpp>
	#include <opencv2/imgproc/imgproc.hpp>
	#include <opencv2/core/core.hpp>
	#endif
	
	//! Workshop Six Sensor
	class WorkshopSixSensor : public Camera
	{
	public:
	    RTTI_DECL();
	
	    //! Construction
	    WorkshopSixSensor() : m_StopThread(false), m_ThreadStopped(false), m_Paused(0)
	    {}
	
	    //! ISensor interface
	    virtual const char * GetSensorName()
	    {
	        return "Camera";
	    }
	
	    //! ISensor interface
	    virtual bool OnStart();
	    virtual bool OnStop();
	    virtual void OnPause();
	    virtual void OnResume();
	
	    void				SendingData(VideoData * a_pData);
	
	private:
	
	    //! Data
	    volatile bool		m_StopThread;
	    volatile bool		m_ThreadStopped;
	    volatile int 		m_Paused;
	#ifndef _WIN32
	    cv::VideoCapture    m_Capture;
	#endif
	
	    void                CaptureVideo(void * arg);
	};
	
	#endif //SELF_WORKSHOP_
	```


### B. Building out the OnStart(), CaptureVideo() and SendingData() functions for the camera sensor

Open the `WorkshopSixSensor.cpp` file, which contains the following functions that enable the camera sensor you'll create:

  * **OnStart()**: Initializes the camera sensor. The OnStart() function is called by the SensorManager class. Because OnStart occurs on the main thread, we want to do as little as possible processing and do the core logic on a background thread. Here we instantiate the VideoCapture object from opencv and spawn a background thread to capture the images. Using our Delegate class, we can call functions in C++ on different threads.
  
  * **OnStop()**: Stops the camera sensor. After the sensor is called, it should wait for any processing to occur and release any objects in memory.
  
  * **OnPause()**: Pauses the camera sensor. The pause function increments an m_Pause integer. The reason this is not a boolean is because other places throughout core Intu can pause a sensor, and we need to keep track how many times pause is called before we know we can resume. When Pause is called, no data is sent to the extractor that has subscribed to its data.
  
  * **OnResume()**: Resumes the sensor. The sensor can then start sending data to the extractor that is subscribed to it.
  
  * **CaptureVideo()**: Using opencv, we take raw buffers from the camera and encode them in JPEG format. This will continually happen based on the frames per second that is declared from the m_FramesPerSecond variable, and will stop when the OnStop() function is called.
 
  * **SendingData()**: Checks whether the camera is paused. If it's not paused, data is sent to all extractors that have subscribed to it.

The OnStop, OnPause and OnResume functions are already completely built out.

In the next step, you will build out the **OnStart**, **CaptureVideo** and **SendingData** functions using the example code provided.

1. The new **WorkshopSixSensor.cpp** code you just added will need to be filled out with some code snippets listed below.

2. For the **OnStart()** function, copy the code directly below **//Code for OnStart()**. Now paste this inside the function body **{}** of **OnStart()** in `WorkshopSixSensor.cpp` inside your **sensors** directory, overwriting all the code already inside the function body **{}**. The code which you need is displayed below for completeness; however, it is **not** recommended for you to copy it from here due to formatting issues.


  ```
	Log::Debug("WorkshopSixSensor", "Starting Camera!");
	    	m_StopThread = false;
	    	m_ThreadStopped = false;
	#ifndef _WIN32
	    m_Capture.open(0);
	
	    if( !m_Capture.isOpened() )
	    {
	        Log::Error("MacCamera", "Could not open Mac Camera - closing sensor");
	        return false;
	    }
	    Log::Debug("WorkshopSixSensor", "Video Capture is now opened!");
	#endif
	    ThreadPool::Instance()->InvokeOnThread<void *>(DELEGATE(WorkshopSixSensor, CaptureVideo, void *, this), 0);
	    return true;
    ```

3. For the **CaptureVideo()** function, copy the code directly below **//Code for CaptureVideo()**. Now paste this inside the function body **{}** of **CaptureVideo()** in `WorkshopSixSensor.cpp`. The code which you need is displayed below for completeness; however, it is **not** recommended for you to copy it from here due to formatting issues.


  
  ```
	 #ifndef _WIN32
    while(!m_StopThread)
    {
        cv::Mat frame;
        m_Capture.read(frame);
        std::vector<unsigned char> outputVector;
        if ( cv::imencode(".jpg", frame, outputVector) && m_Paused <= 0 )
            ThreadPool::Instance()->InvokeOnMain<VideoData *>( DELEGATE( WorkshopSixSensor, SendingData, VideoData *, this ), new VideoData(outputVector));
        else
            Log::Error( "WorkshopSixSensor", "Failed to imencode()" );
        tthread::this_thread::sleep_for(tthread::chrono::milliseconds(1000 / m_fFramesPerSec));
    }
	#endif
  ```
  
4. For the **SendingData()** function, copy the code directly below **//Code for SendingData()**. Now paste this inside the function body **{}** of **SendingData()** in `WorkshopSixSensor.cpp`. The code which you need is displayed below for completeness; however, it is **not** recommended for you to copy it from here due to formatting issues.

  
  ```
	if (m_Paused <= 0)
        SendData(a_pData);
  ```


5. Now that you have updated the code, you will have to once again build the **Intu SDK**. [Click here](../../../installation/compiling.md/#osx) for instructions on how to do so.

**Congratulations!** You just built all the functions required for the camera sensor. 

In the next task, you will update the `body.json` file also located in the **intu/self-sdk-master/bin/mac/etc/profile** directory to include the new plugin so that Intu can use it.


## 4. <a name="configuring-your-Intu-instance-to-include-the-camera-sensor">Configuring your Intu instance to include the camera sensor</a>

### A. Retrieving the credentials for your Organization in the Intu Gateway

1. [Log in to the Intu Gateway](https://rg-gateway.mybluemix.net/). 

2. Click on **VIEW CREDENTIALS** in the left hand navigation bar.

3. Select your Organization and Group in the top Filter by menu, and click on the **Get Credentials** box. Click the **Copy** button.

4. Navigate to the following directories:
	
	* For **OS X**:  **wlabs_self-sdk-master/bin/mac**.
	* For **Windows**: **Visual Studio**, in the **Solution Explorer**, go to **sdk -> bin**.

5. If there is a `config.json` file, delete its current contents and paste the credentials you obtained into this file.  If there is no `config.json` present, add a new file in this directory and name it `config.json`.  Paste your credentials obtained from the gateway into this file.

### B. Configuring your `body.json` file

1. Open your `body.json` file. This will be in **self-sdk-master/bin/mac/etc/profile**.
	
2. Locate the `m_Libs` variable. The variable should look like `"m_Libs" : [ "platform_mac" ],`

3. Add **workshop****_six****_plugin** to the end of the `m_Libs` variable for your platform, **as shown below**:
  `"m_Libs" : [ "platform_mac", "workshop_six_plugin"],`

4. Locate `m_Sensors` inside the `body.json`. Add the following

	```
	{
       "Type_" : "Camera",
       "m_SensorId" : "8f385c2a-ecb0-3bfb-32af-3c54ec18db5c",
       "m_fFramesPerSec" : 10
    },
	```

5. Verify that it looks something like:

	```
	"m_Sensors" : [
	{
		"Type_" : "Camera",
		"m_SensorId" : "8f385c2a-ecb0-3bfb-32af-3c54ec18db5c",
		"m_fFramesPerSec" : 10
	},
	{....	   
	```

6. Save your changes.

### C. Building Intu

1. Return to your most recent Terminal window, where you should already be in the **mac** directory. Otherwise, open a **new** Terminal window and navigate to this directory by running: `cd intu/self-sdk-master/bin/mac`

2. Run Intu. For instructions on how to do so, [click here](../../../installation/running.md/#intu-sdk).

Now Intu is running, you should see your Mac camera turn on. Now Intu will be able to capture vision through your Mac camera.

### D. Challenge: Add a camera sensor to a Raspberry Pi

The process you have just undertaken was adding a camera into your Intu instance on your Mac, by extending the self-sdk. The challenge is to now add a camera instance for the Raspberry Pi. (Hint: do workshop 5 first and think how you can alter the process and think how how could get the code for the camera on the Raspberry Pi instead of the LED gesture. 

**Pro Tips:** 

1. In your Terminal/PuTTY window, SSH into your Raspberry Pi, and run: `sudo modprobe bcm2835-v4l2` to ensure that the PiCam data is passed through. You need to do this every time you open the terminal

2. Enable the camera on your Raspberry Pi. 
	1. Open the raspi-config tool from your SSH session to the Raspberry Pi by running: `sudo raspi-config`.
	2. Select `Enable camera` and hit **Enter**. 
	3. Then go to **Finish** and you'll be prompted to reboot.

## Reminder: Update your services on the Gateway

You need to update your services on the Intu Gateway within 30 days of creating your organization.  For more details on how to do so, [click here](../../../installation/configuring.md).
