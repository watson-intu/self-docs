# Workshop 4: Customizing Intu to call your phone

In this workshop, you will configure Intu's telephony plan to have Intu call your phone by using the Nexmo Cloud Communications API. 

A plan is a set of preconditions, a set of actions, and a set of postconditions to address a given goal. You'll learn more about the importance of plans and how they work later in this workshop.

**Before you begin:** You must have a Mac or Windows laptop, and you must have completed Workshop 1: Say Hello!. You will notice that Intu and Self are used interchangeably. Self is the technical name for Intu.

Complete the following tasks:

1. [Signing up for a Nexmo account](#signing-up-for-a-nexmo-account)
2. [Getting your Nexmo credentials](#getting-your-nexmo-credentials)
3. [Adding the telephony service to Intu](#adding-the-telephony-service-to-Intu)
4. [Understanding plans](#understanding-plans)
5. [Modifying plans to have the telephony service call your phone number](#modifying-plans-to-have-the-telephony-service-call-your-phone-number)

## 1. <a name="signing-up-for-a-nexmo-account">Signing up for a Nexmo account</a>

1. [Sign up for a Nexmo account](https://dashboard.nexmo.com/sign-up)
  
## 2. <a name="getting-your-nexmo-credentials">Getting your Nexmo credentials</a>

1. On the [Nexmo dashboard](https://dashboard.nexmo.com/), click the top right drop down menu (the one with your name next to it) to expand the account menu.

2. Click **Settings**.

3. Copy the values of the **API key** and **API Secret** parameters. You will need these for configuration on the Intu Gateway.

4. You will need to pay for minutes on the Nexmo service. This will allow your account to allow to make calls using the API key/secret you copied above.

## 3. <a name="adding-the-telephony-service-to-Intu">Adding the telephony service to Intu</a>

1. Log in to the [Intu Gateway](https://rg-gateway.mybluemix.net/).
2. Click **MANAGE** on the left side of the page, and click **Services**.
3. In the **Filter By** fields, select your Organization and Group.
4. Click **+ Add Service**.
5. Specify your Nexmo credentials.
  1. In the **SERVICE NAME** field, specify **TelephonyV1**.
  2. In the **USER ID** field, paste the **API key** value from Nexmo.
  3. In the **PASSWORD** field, paste the **API Secret** value from Nexmo.
  4. In the **SERVICE ENDPOINT** field, specify: **ws://nexmo-watson.mybluemix.net/ws-embodiment**
6. Click **SAVE**.
7. If you currently have Intu running, stop it to enable the Telephony service to automatically provision a US phone number for your device.

## 4. <a name="understanding-plans">Understanding plans</a>

Plans are the primary driving factor in how Intu can accomplish goals. For example, you can preconfigure a plan that tells Intu how to act in a certain situation without any code changes.

Plans contain two major components:

  * **Preconditions**: A set of conditions that must be met for a particular plan to begin execution.
  
  * **Actions**: After all preconditions are met, a series of actions are taken to to complete a plan. Intu supports two types of actions: CreateAction, which creates a blackboard object and places it on the blackboard, and UseSkillAction, which tells Intu to execute a specified skill.

All plans are loaded when Intu starts. When a goal object is placed on the blackboard, the GoalAgent finds the best possible plan, executes the plan, and establishes whether that goal was completed successfully (i.e., the plan finished with no action failures) or failed (i.e., no plan was found to carry out execution).

## 5. <a name="modifying-plans-to-have-the-telephony-service-call-your-phone-number">Modifying plans to have the Telephony service call your phone number</a>

1. Locate the **plans** directory where you will be modifying your plan. If you used the Intu INSTALLER (from the Intu Gateway) to install Intu in Workshop 1, the path is:

	* **For Mac users:** /Applications/IBM/Self/latest/etc/shared/plans
	
	* **For Windows users:** C:\Users\username\AppData\LocalLow\IBM\Self\INTU_VERSION\etc\shared\plans
	
	*Note for Windows Users:* replace *INTU_VERSION* with the latest version of Intu you installed. You can look in the *Self* directory to see which one is the newest.

2.  Open the `default.json` file in the **plans** directory using your favorite text editor (if on Mac, refrain from using TextEdit as it will format text and cause the plan not to be found. On Windows, it is preferable to use Notepad/Wordpad and on Mac it is preferable to use Sublime or nano/vim).

3. Browse through the different plans and notice how plans can have different preconditions based on the data that is represented.
For example, look at the first plan called `"dialog_answer"`. It contains a set of preconditions (the key is `"m_PreConditions"`) that must be answered for that plan to execute. The first parameter, the array with the key `"m_Params"`, in that precondition states that the data being analyzed must have the format `"{"answer" : {"response" : ["some value"] }, }"`, where the array in response must not be equal to null, while the second precondition states the response array must not have a key of `"id"` in the response array.

4. Search for a plan called `"outgoing_call"`, and change the value of `"m_ToNumber"` in the second action to your phone number (Be sure to include the country code first, e.g. **15551231234**). Now, look at the actions for this plan. The first action will have Intu `"Dialing"`, while the second action will carry out the execution to call the number specified.

5. Run Intu. For instructions on how to do so [click here](../../../installation/running.md)

6. Ask Intu "Can you call me?". When your phone rings, answer it. Have a conversation with Intu. Say "Tell me a joke". You should hear a joke. You can continue to talk or hang up.

## Reminder: Update your services on the Gateway

You need to update your services on the Intu Gateway within 30 days of creating your organization.  For more details on how to do so, [click here](../../../installation/configuring.md).
