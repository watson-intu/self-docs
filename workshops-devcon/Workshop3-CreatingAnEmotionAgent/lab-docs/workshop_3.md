# Workshop 3: Creating an emotion agent

In this workshop, you create an emotion agent. Agents make decisions about how Intu operates and responds. The emotion agent uses the Tone Analyzer service on Bluemix, which analyzes a person's tone and determines whether that tone is positive or negative.

**Before you begin:** You must have a Mac or Windows laptop, and you must have completed Workshop 1: Say Hello!. You will notice that Intu and Self are used interchangeably. Self is the technical name for Intu.

**Note for Windows users:** You will need to have [**Visual Studio  Community 2015**](https://www.microsoft.com/en-us/download/details.aspx?id=48146) installed. This will take some time to download.

Complete the following tasks:

1. [Setting up the Tone Analyzer service](#setting-up-the-tone-analyzer-service)
2. [Understanding some Intu terminology](#understanding-some-intu-terminology)
3. [Building the Self SDK](#building-the-intu-sdk)
4. [Creating an emotion agent](#creating-an-emotion-agent)
5. [Configuring Intu to include your emotion agent](#configuring-intu-to-include-your-emotion-agent)

## <a name="setting-up-the-tone-analyzer-service">1. Setting up the Tone Analyzer service </a>

### A. Creating your own instance of the Tone Analyzer service

1. [Log in to Bluemix](https://console.ng.bluemix.net/) if you are not already logged in.

2. On the Bluemix dashboard, click **Catalog** in the top-right navigation bar.

3. In the All Categories menu on the left, under Services, click **Watson**.

4. Click the **Tone Analyzer** tile.
  1. Keep all the default values, and click **Create**.
  2. Click the **Service Credentials** tab.
  3. Click **View Credentials** for the new Tone Analyzer service instance you just created. 
  4. Copy these credentials (everything inside **{ }**) and paste them into a new text file in your favourite text editor. You will need these credentials to configure your Tone Analyzer service in the section below.

### B. Configure Intu to use your Tone Analyzer service instance

1. Open a new browser window and [log in to the Intu Gateway](https://rg-gateway.mybluemix.net/).

2. Click on **MANAGE** on the left hand side navigation bar, and select **Services**. Select your Organization and Group in the top Filter by menu

3. Click the **Add Service** box.
  
4. In the **SERVICE NAME** field, specify **ToneAnalyzerV1**.

5. In the **USER ID** field, copy in the username you pasted into your text file in the above.

6. In the **PASSWORD** field, copy in the password.

7. In the **SERVICE ENDPOINT** field, copy in the url.

8. Click the **Add** button at the bottom right of the window.

## 2. <a name="understanding-some-intu-terminology">Understanding some Intu terminology</a>

Before you create an emotion agent, become familiar with the following terminology:

  * **Blackboard**: The central message broker on which all the agents post data and listen for incoming data.
  
  * **Publish**: To push data onto the Blackboard under a particular topic. 
  
  * **Subscribe**: Subscribing to a topic on the Blackboard means to listen to the Blackboard and wait for any other agents to post to the Blackboard under a particular topic.
  
  * **Topic**: A channel to which Agents publish and subscribe.

## 3. <a name="building-the-intu-sdk">Building the Self SDK</a>

See [Installing and building the Self SDK](https://github.com/watson-intu/self-sdk#compiling-intu-for-various-platforms) before you continue in this workshop.


## 4. <a name="creating-an-emotion-agent">Creating an emotion agent</a>

### A. Preparing your directories and files for the emotion agent

1. Create a directory called **workshop_three** in the **~/intu/self-sdk-master/examples/** directory. 

2. Create a file called **WorkshopThreeAgent.cpp** in this directory, and paste the following code:

	```
	
	#include "WorkshopThreeAgent.h"
	#include "SelfInstance.h"
	#include "blackboard/BlackBoard.h"
	#include "blackboard/Goal.h"
	#include "skills/SkillManager.h"
	
	REG_SERIALIZABLE( WorkshopThreeAgent );
	RTTI_IMPL( WorkshopThreeAgent, IAgent);
	
	// Serialize will build a JSON object from the object data created by the Emotion Agent.
	void WorkshopThreeAgent::Serialize(Json::Value & json)
	{
	    IAgent::Serialize( json );
	
	    SerializeVector("m_NegativeTones", m_NegativeTones, json);
	    SerializeVector("m_PositiveTones", m_PositiveTones, json);
	
	    json["m_EmotionalState"] = m_EmotionalState;
	    json["m_EmotionTime"] = m_EmotionTime;
	}
	
	//  Read in JSON data and create a readable object for functions to read from.
	void WorkshopThreeAgent::Deserialize(const Json::Value & json)
	{
	    IAgent::Deserialize( json );
	
	    DeserializeVector("m_NegativeTones", json, m_NegativeTones);
	    DeserializeVector("m_PositiveTones", json, m_PositiveTones);
	
	    if( json.isMember( "m_EmotionalState" ) )
	        m_EmotionalState = json["m_EmotionalState"].asFloat();
	    if (json.isMember("m_EmotionTime"))
	        m_EmotionTime = json["m_EmotionTime"].asFloat();
	
	    if (m_NegativeTones.size() == 0)
	        m_NegativeTones.push_back("extraversion");
	    if (m_PositiveTones.size() == 0)
	        m_PositiveTones.push_back("joy");
	}
	
	//  OnStart() will subscribe the Emotion Agent to start listening for updates from the Blackboard and start listening.
	bool WorkshopThreeAgent::OnStart()
	{
	    SelfInstance * pInstance = SelfInstance::GetInstance();
	    assert( pInstance != NULL );
	    BlackBoard * pBlackboard = pInstance->GetBlackBoard();
	    assert( pBlackboard != NULL );
	
	    pBlackboard->SubscribeToType("LearningIntent",
	                                 DELEGATE(WorkshopThreeAgent, OnLearningIntent, const ThingEvent &, this), TE_ADDED);
	    pBlackboard->SubscribeToType("Text",
	                                 DELEGATE(WorkshopThreeAgent, OnText, const ThingEvent &, this), TE_ADDED);
	
	    m_spEmotionTimer = TimerPool::Instance()->StartTimer(VOID_DELEGATE(WorkshopThreeAgent, OnEmotionCheck, this), m_EmotionTime, true, true);
	    return true;
	}
	
	//  OnStop() will unsubscribe the Emotion Agent, stopping it from listening for updates posted to the Blackboard.
	//  This will ensure processes are won't be running in the background.
	bool WorkshopThreeAgent::OnStop()
	{
	    SelfInstance::GetInstance()->GetBlackBoard()->UnsubscribeFromType( "LearningInten", this);
	    SelfInstance::GetInstance()->GetBlackBoard()->UnsubscribeFromType( "Text", this );
	    return true;
	}
	
	//  OnText() waits for a text repsponse Tone Analyser instance
	void WorkshopThreeAgent::OnText(const ThingEvent & a_ThingEvent)
	{
	
	}
	
	//  OnLearningIntent() waits for a response waits for a target response, and depending on feedback being positive or net
	//  and emotional state being constrained between zero and one, increments the emotional state. The updated emotional state
	//  is then published to the Blackboard.
	void WorkshopThreeAgent::OnLearningIntent(const ThingEvent & a_ThingEvent)
	{
	
	}
	
	//  OnTone() waits for the response from the Tone Analyser...
	void WorkshopThreeAgent::OnTone(DocumentTones * a_Callback)
	{
	
	}
	
	//  Every 30 seconds OnEmotionCheck() will either increase or decrease the EmotionalState value to ensure that over time
	//  EmotionalState resolve back to a value of 0.5 over time.
	void WorkshopThreeAgent::OnEmotionCheck()
	{
	    if (m_EmotionalState > 0.5)
	        m_EmotionalState -= 0.1f;
	    else
	        m_EmotionalState += 0.1f;
	
	    PublishEmotionalState();
	}
	
	//  PublishEmotionalState() will publish a value of EmotionalState to the Blackboard. Agents listening for EmotionalState
	//  will be updated when a new EmotionalState is published.
	void WorkshopThreeAgent::PublishEmotionalState()
	{
	    Json::Value json;
	    json["m_EmotionalState"] = m_EmotionalState;
	
	    SelfInstance::GetInstance()->GetBlackBoard()->AddThing(IThing::SP(
	            new IThing(TT_PERCEPTION, "EmotionalState", json, 3600.0f)));
	}
	```
3. Create a file called **WorkshopThreeAgent.h** in the same directory, and paste the following code:

	```
	#ifndef SELF_WORKSHOP_THREE_AGENT_AGENT_H
	#define SELF_WORKSHOP_THREE_AGENT_AGENT_H
	
	#include "agent/IAgent.h"
	#include "blackboard/Text.h"
	#include "blackboard/LearningIntent.h"
	#include "services/WeatherInsights.h"
	#include "services/ToneAnalyzer/ToneAnalyzer.h"
	#include "sensors/SensorManager.h"
	#include "sensors/MoodData.h"
	#include "SelfLib.h"
	
	class SkillInstance;
	
	//! This agent handles emotions
	
	class WorkshopThreeAgent : public IAgent
	{
	public:
	    RTTI_DECL();
	
	    WorkshopThreeAgent() : m_EmotionalState( 0.5f ), m_EmotionTime ( 30.0f )
	    {}
	
	    //! ISerializable interface
	    virtual void Serialize(Json::Value & json);
	    virtual void Deserialize(const Json::Value & json);
	
	    //! IAgent interface
	    virtual bool OnStart();
	    virtual bool OnStop();
	
	private:
	    //! Data
	    std::vector<std::string>	m_NegativeTones;
	    std::vector<std::string>	m_PositiveTones;
	    float						m_EmotionalState;
	    float						m_EmotionTime;
	    TimerPool::ITimer::SP		m_spEmotionTimer;
	
	    //! Event Handlers
	    void 		OnEmotion(const ThingEvent & a_ThingEvent);
	    void		OnText(const ThingEvent & a_ThingEvent);
	    void		OnLearningIntent(const ThingEvent & a_ThingEvent);
	    void		OnTone(DocumentTones * a_Callback);
	
	    void		OnEnableEmotion();
	    void		OnEmotionCheck();
	    void		PublishEmotionalState();
	};
	
	#endif // SELF_WORKSHOP_THREE_AGENT_AGENT_H
	```
	
**For OS X users:**

1. If you do not have it installed already, download the trial version of the [CLion C++ IDE](https://www.jetbrains.com/clion/download/).

2. In **CLion**, select **Open** -> **your home directory** -> **intu** -> **self-sdk-master** and click **OK**. 

	Note that a window may appear prompting you to open your project in a New Window or This Window. Select **New Window**. At the bottom of your CLion window, in the Problems tab, you will see the following Error, which you do not need to worry about:

	Error: By not providing "FindSELF.cmake" in CMAKE_MODULE_PATH this project has asked CMake to find a package configuration file provided by "SELF", but CMake did not find one.
Could not find a package configuration file provided by "SELF" with any of the following names:
  SELFConfig.cmake   self-config.cmake
Add the installation prefix of "SELF" to CMAKE_PREFIX_PATH or set "SELF_DIR" to a directory containing one of the above files.  If "SELF" provides a separate development package or SDK, be sure it has been installed.

	2i. Right-click the `CMakeLists.txt` file in the **examples** directory, and click **Copy**. (If you are unsure of the directory you are in, look in the top-left navigation bar.)
  
	2ii. Right-click the **workshop_three** directory, and click **Paste**. This file helps to build the plugin for the emotion agent.

	2iii. Return to the **examples** directory, open the `CMakeLists.txt` file, and add the following line: `add_subdirectory(workshop_three)` at the end. Your file contains the following three lines:

  ```
    include_directories(".")

    add_subdirectory(sensor)
    add_subdirectory(workshop_three)
  ```
	2iv. Open the `CMakeLists.txt` file in the **workshop_three** directory, and overwrite its content with this code:

  ```
    include_directories(.)

    file(GLOB_RECURSE SELF_CPP RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "*.cpp")
    qi_create_lib(workshop_three_plugin SHARED ${SELF_CPP})
    qi_use_lib(workshop_three_plugin self wdc)
    qi_stage_lib(workshop_three_plugin)
  ```

**For Windows users:**

2. In **Visual Studio** open the solution located at **intu/self-sdk-master/vs2015/self-sdk.sln**. Right click on the **examples** folder and select **Add->New Project**. Select **Visual C++/Win32** in the tree to the left, select **Win32 Project** and set the name field to `workshop_three_plugin`, finally click **OK**. 

3. Click **Next**, select **Application Type as DLL**, uncheck **Precompiled header and Security Development Lifecycle (SDL) checks** and check **Empty Project** under **Additional options**.

4. Click **Finish**.

6. Inside the **Solution Explorer** window, right click **workshop_three****_plugin**, and select **Add -> Existing Item** then select the files you added above **WorkshopThreeAgent.cpp** and **WorkshopThreeAgent.h**.

10. Right-click the **workshop_three_plugin** project, open **Properties**, and make the following changes:
	1. Select Configuration at the top left is set to **All Configurations**.
	2. Change the value of **General -> Character Set** to **Use Multi-Byte Character Set**.
	3. Go to **C/C++ -> General** **-> Additional Include Directories** **->**, and add: 
		```
		..\..\examples\workshop_three;..\..\include\self;..\..\include\wdc;..\..\lib\boost_1_60_0;..\..\lib;%(AdditionalIncludeDirectories)
		```
	4. Go to **C/C++ -> Precompiled Headers -> Precompile Header File**, and delete the value. Make sure it's blank.
	5. Go to **Linker -> General -> Additional Library Directories ->**, and add: 
		```
		../../lib/$(Configuration);../../lib/boost_1_60_0/stage/lib/
		```
	6. Replace the value of **Linker -> Input -> Additional Dependencies** with the following value: 
		```
		jsoncpp.lib;self.lib;wdc.lib;%(AdditionalDependencies)
		```
	7. Go to **Build Events -> Post-Build Event -> Command Line**, and add: 
	```
	copy /Y "$(TargetPath)" "$(ProjectDir)..\..\bin\$(Configuration)"
	```

11. Right-click the **workshop_three_plugin** project, select **Build Dependencies->Project Dependecies**. Check the checkbox beside **self-sdk** to make our new project dependent on the self-sdk project.

### B. Building out the OnText, OnTone and OnLearningIntent functions for your emotion agent

Open the `WorkshopThreeAgent.cpp` file, which contains the following functions that enable the emotion agent you'll create:

  * **Serialize()**: All classes in Intu inherit from our base interface ISerializable. We have the ability to serialize any object back into json format, which usually will get stored in the body.json (found in same directory as self_instance).

  * **Deserialize()**: All classes in Intu inherit from our base interface ISerializable. We have the ability to deserialize any object from json into memory, which is usually from the body.json (found in the same directory as self_instance).

  * **OnStart()**: Starts the agent. The OnStart() function is called from the AgentSociety, which will start and stop all agents on start up and shut down. Since the OnStart()
  function is called on the main thread, we want to do as little processing as possible. For agents, we want to subscribe to objects that are placed on the Blackboard, start
  timers, or potentially instantiate a service. To subscribe to a Blackboard object, look at the BlackBoard.h file for SubscribeToType function. There we can specify the type of the object we want to subscribe to, our callback information, and a macro stating what type of event we are interested in. For example, TE_ADDED specifies to let the agent know when the Blackboard object has been added to the Blackboard, while TE_STATE specifies to let the agent know when the Blackboard object state has changed (i.e., from processing to finished).
  
  * **OnStop()**: Stops the agent. The OnStop() function is called from the AgentSociety, which will start and stop all agents on start up and shut down. The OnStop() function is called on the main thread, so it is important that this call blocks and waits for all lingering processes to finish in the agent. Here, we typically will unsubscribe from Blackboard objects and make sure to reset any timers that may be currently active.
  
  * **OnEmotion()**: Is our callback function when an emotion object was placed on the Blackboard. All Blackboard objects inherit from the IThing interface. That means when we receive a ThingEvent, we grab the IThing object and dynamically cast it to the object we believe we have. Always make sure to check the object is not null before proceeding with your logic.
  
  * **OnText()**: Is our callback function when a Text object is placed on the Blackboard. Text objects typically are created from the TextExtractor class, which subscribes to Audio sensors, takes in raw audio, sends it to the STT service, and instantiates a Text object with the final transcription. Here, we are interested in text objects so we can determine the tone of the user. Once a text object is received, then we want to send the transcript to the Tone Analyzer service.
  
  * **OnLearningIntent()**: This is our callback for when we receive a learning intent. A learning intent could be many things, but for this particular case we are only interested in positive and negative feedback. An example of each could be "Good Job!"/"Bad Job!" respectively. Initially, the EmotionalState is 0.5 and must be 0 - 1. Each time the agent receives a piece of positive or negative feedback, the OnLearningIntent() function increases for positive feedback or decreases for negative feedback the EmotionalState variable score by 0.1. 
  
  * **OnTone()**: This is our callback for when we receive data from the Tone Analyzer service. Depending on the response, we iterate through pre-determined vectors of positive and negative moods. If there is a match with a positive mood (i.e., "Joy"), then we will increment the emotional state, while if there is a negative tone (i.e., "Disgust") then we will decrement the emotional state.
  
  * **OnEmotionCheck()**: Restores the EmotionalState to a basal level of 0.5. For every 30 seconds when the EmotionalState is less than 0.5, the OnEmotionCheck() increases by 0.1. Similarly, for every 30 seconds when the EmotionalState is more than 0.5, the OnEmotionCheck() decreases by 0.1. This ensures that the EmotionalState will trend back to neutral over time.

  * **PublishEmotionalState()**: Formats the current EmotionalState value, formats it into the json value, and adds it to the Blackboard.

  
The Serialize, Deserialize, OnStart, OnStop, OnEmotion, OnLearningIntent, OnEmotionCheck, and PublishEmotionalState functions are already completely built out.

In the next step, you will build out the **OnText**, **OnTone** and **OnLearningIntent** functions using the example code provided.

1. In the following section, you will see code snippets that you need to copy and paste into the **WorkshopThreeAgent.cpp** file that you created earlier.  The instructions will tell you in which function specificially the snippets need to be pasted.

2. For the **OnText()** function, copy the code directly below:
  
  ```
  Text::SP spText = DynamicCast<Text>(a_ThingEvent.GetIThing());
    if (spText)
    {
        ToneAnalyzer * tone = SelfInstance::GetInstance()->FindService<ToneAnalyzer>();
        if (tone != NULL)
        {
            tone->GetTone(spText->GetText(), DELEGATE(WorkshopThreeAgent, OnTone, DocumentTones *, this));
        }
    }
  ```
            
3. For the **OnTone()** function, copy the code directly below:
  
  	```
     if (a_Callback != NULL)
    {
        double topScore = 0.0;
        Tone tone;
        for (size_t i = 0; i < a_Callback->m_ToneCategories.size(); ++i)
        {
            for (size_t j = 0; j < a_Callback->m_ToneCategories[i].m_Tones.size(); ++j)
            {
                Tone someTone = a_Callback->m_ToneCategories[i].m_Tones[j];
                if (someTone.m_Score > topScore)
                {
                    topScore = someTone.m_Score;
                    tone = someTone;
                }
            }
        }
        Log::Debug("WorkshopThreeAgent", "Found top tone as: %s", tone.m_ToneName.c_str());
        bool toneFound = false;
        for (size_t i = 0; i < m_PositiveTones.size(); ++i)
        {
            if (tone.m_ToneId == m_PositiveTones[i])
            {
                toneFound = true;
                if (m_EmotionalState < 1.0f)
                    m_EmotionalState += 0.1f;
            }
        }

        if (!toneFound)
        {
            for (size_t i = 0; i < m_NegativeTones.size(); ++i)
            {
                if (tone.m_ToneId == m_NegativeTones[i])
                {
                    if (m_EmotionalState > 0.0f)
                        m_EmotionalState -= 0.1f;
                }
            }
        }

        PublishEmotionalState();
    }
  ```
  
4. For the **OnLearningIntent()** function, copy the code directly below:

	```
	LearningIntent::SP spLearningIntent = DynamicCast<LearningIntent>(a_ThingEvent.GetIThing());
    if (spLearningIntent && spLearningIntent->GetVerb().compare("feedback") == 0)
    {
        if (spLearningIntent->GetTarget().compare("positive_feedback") == 0)
        {
            if(m_EmotionalState < 1.0)
                m_EmotionalState += 0.1f;
        }
        else
        {
            if (m_EmotionalState > 0.0)
                m_EmotionalState -= 0.1f;
        }

        PublishEmotionalState();
    }
  
	```
  
First, this code iterates over the response to find the emotion that has the highest probability. Then, it checks whether the emotion is positive, and, if it is, the EmotionalState variable is incremented by 0.1. The EmotionalState variable cannot exceed one. If the highest probability tone is negative, the EmotionalState variable is decreased by 0.1. The EmotionalState variable cannot be less than zero.


## 5. <a name="configuring-intu-to-include-your-emotion-agent">Configuring your Intu instance to include the emotion agent</a>

### A. Retrieving the credentials for your Organization in the Intu Gateway
1. [Log in to the Intu Gateway](https://rg-gateway.mybluemix.net/). 

2. Click on **VIEW CREDENTIALS** in the left hand navigation bar.

3. Select your Organization and Group in the top Filter by menu, and click on the **Get Credentials** box.
4. Create a `config.json` file in case it isn't present and paste the credentials obtained from the gateway in step 3 above.
	
	* For **OS X**,  in **~intu/self-sdk-master/bin/mac**.
	* For **Windows**, in **intu/self-sdk-master/bin/Debug**. 
		* **NOTE:** Debug may be called Release depending on the selected configuration.
	
### B. Configuring your `body.json` file

1. Open your `body.json` file. 

	* For **OS X**, this will be in **intu/self-sdk-master/bin/mac/etc/profile**.
	* For **Windows**, this will be in **intu/self-sdk-master/bin/Debug/etc/profile**.
	
2. Locate the `m_Libs` variable and add **workshop_three_plugin** to the `m_Libs` variable, **as shown below**:
   ```
	"m_Libs" : [ "workshop_three_plugin"],
	```

4. Locate `EmotionAgent` in the `body.json` file, and notice the `m_NegativeTones` and `m_PositiveTones` strings. To understand the tone of the input, these strings are compared to OnTone().

5. Change `EmotionAgent` to `WorkshopThreeAgent` or the name you gave your class. As the instructions used `WorkshopThreeAgent`, the `"Type_"` field becomes `"Type_" : "WorkshopThreeAgent"`.
6. Save your changes.

### 3. Building and Running Intu

**For OS X users:**

1. Open a **new** Terminal window or using an exisiting window and build with the following commands:

	```
	cd ~/intu/self-sdk-master
	scripts/build_mac.sh
	```

2. Execute the following commands to actually run Intu:
	```
	cd ~/intu/self-sdk-master/bin/mac
	export LD_LIBRARY_PATH=./
	./self_instance
	```
	
**For Windows users:**

1. From the Visual Studio Menu, select **Build -> Build Solution**.

2. Once built, you only need to do the following steps one time:
	1. Right-click on **workshop_three_plugin** and select **Set as startup project**. 
	2. Right-click on **workshop_three_plugin** and select **Properties**.
		1. Select **Debugging** tab and browse the **Command** field to the **self_instance.exe** located in the **self-sdk-master/bin/Debug/** directory. 
		2. Select the **Working Directory** and browse to the **self-sdk-master/bin/Debug** directory.
	
3. Run Intu by clicking **Local Windows Debugger** at the top of Visual Studio.

Now that you have added an Emotion Agent, Intu will start to adapt to you. First, ask Intu “How are you?”, and listen to the response. Now feed Intu some positive emotion statements like "Good job!”, and then ask “How are you?” again. Intu should now give a "happier" response than the one it gave before. Try the same thing for some negative emotion statements. Say “Wrong answer" a number of times and then ask “How are you?”. Intu should respond with a "sadder" response.

## Reminder: Update services within 30 days of registering on the Gateway
Your instance of Intu is preconfigured with the following Watson services: Conversation, Weather Company Data, Speech to Text, and Text to Speech. The preconfiguration is enabled for 30 days. If you want to test Intu after 30 days, you must create your own instances of these services and configure Intu to use them.

### A. Creating instances of Watson services
To use Intu, you need operational instances of the following services in Bluemix: Conversation, Weather Company Data, Speech to Text, and Text to Speech.

**Pro tip:** As you complete this task, you'll receive credentials for each service instance, and you'll need these credentials later. Open a new file in your favourite text editor and create a section for each service so that you can temporarily store its credentials.

1. On the Bluemix dashboard, click **Catalog** in the top right navigation bar.

2. In the All Categories menu on the left, under Services, click on **Watson**.

3. Create an instance of the Conversation service.
  1. Click the **Conversation** tile.
  2. Keep all the default values, and click **Create**.
  3. Click the **Service Credentials** tab.
  4. Click **View Credentials** for the new Conversation service instance.
  5. Copy the values for your password and username and paste them into a new text file in your favourite text editor.
  6. Click the **<--Watson** breadcrumb near the top left (directly above your Conversation service name). The list of your service instances is displayed.
  7. Add the next service instance by clicking the **Create Watson** **+** button. The Watson service catalog is displayed.

4. Create instances of the Weather Company Data, Speech to Text, and Text to Speech services by repeating the same substeps 1 - 7 that you completed to create the Conversation service instance.

### B. Configuring Intu to use your service instances

To configure Intu to use your instances of these Watson services, log in to the [Intu Gateway](https://rg-gateway.mybluemix.net/) and complete the following steps:

1. Click on **MANAGE** on the left hand side navigation bar, and select **Services**. 

2. Select your Organization and Group in the top Filter by menu, if not already selected.

3. For your instances of the Weather Company Data, Speech to Text, and Text to Speech services, click **Edit**, and specify the user ID and password (saved in your text file in the previous section **Creating instances of Watson services**), and click **Save**.

4. To configure your instance of **Conversation**, navigate to **DOWNLOADS** on the left of your Intu Gateway browser page, download the **Intu Starter Kit**, and follow the instructions in the `readme.txt` file. Alternatively, go to the instructions for **Workshop 2**, and follow the steps in: **1. Setting up the Conversation service**.
 
**Important:** Do not change the service endpoint for your services unless you are an enterprise user.
