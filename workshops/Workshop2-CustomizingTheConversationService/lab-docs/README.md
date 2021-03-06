# Workshop 2: Customizing the Conversation Service

In this workshop, you customize the greeting in the dialog from the Conversation service workspace and extend the conversation by adding new intents, new entities, and new dialog.

**Before you begin:** You must have a Mac or Windows laptop, and you must have completed [Workshop 1 --  Say Hello!](../../../workshops/Workshop1-SayingHello/lab-docs/README.md).

Complete the following tasks:

1. [Setting up the Conversation service](#setting-up-the-conversation-service)
2. [Customizing the greeting](#customizing-the-greeting)
3. [Adding a new conversation flow with a new intent, new entities, and new dialog](#adding-a-new-conversation-flow-with-a-new-intent,-new-entities,-and-new-dialog)
4. [Configuring Intu to use your new Conversation instance](#configuring-Intu-to-use-your-new-Conversation-instance) 

## 1. <a name="setting-up-the-conversation-service">Setting up the Conversation service</a>

You must create your own instance of the Conversation service in Bluemix, configure Intu to use this service instance, and import a Conversation workspace.

### A. Creating your own instance of the Conversation service

First you will need to create your own instance of the Convesation service on Bluemix.  For step-by-step instructions on how to create and configure the Conversation service, [click here](../../../installation/configuring.md/#conversation).

### B. Importing the example "Intu-Dialog" workspace into the Conversation service

To complete this task, you must download the **Intu Starter Kit**, which contains a complete Conversation service workspace **Intu-Dialog** that you need for this workshop.

**Important:** Should you wish to use your own **custom** Conversation workspace, please skip down and follow the instructions in **Challenge: Using your own custom Conversation workspace with Intu** below.

1. Open a new browser window and [log in to the Intu Gateway](https://rg-gateway.mybluemix.net/).

1. Click **DOWNLOADS** on the left hand side navigation bar.

2. Click on the big download arrow for **Download Intu Starter Kit**. Extract the contents of the zip file into a working directory and take note of this so that you know where to find it in step 7 below. **Note:** The directory that you select for this starter kit does not matter; it contains only the `intu-workspace-full.json` file required in step 7 below and a `readme.txt` file. 

3. Return to the Conversation service window in Bluemix (left open in your browser in step 4 above), and select **Manage**.

4. Click **Launch tool**. You are directed to the Watson Conversation page. Click **Log in with IBM ID** if you see this window. Note: If you encounter an error saying *Insufficient Privileges*, reopen Conversation in a different web browser.
 
5. **Do not create a new workspace.** Click the **Import** icon next to the green **Create +** button.

6. Click **Choose a file**, navigate to the directory where you extracted the Intu Starter Kit in step 2, select the `intu-workspace-full.json` file, and click **Import**. Keep the default settings so that you import everything. The workspace titled **Intu-Dialog** is imported and is displayed on the Workspaces page.


## 2. <a name="customizing-the-greeting">Customizing the greeting</a>

**Before you begin:** Turn on the microphone on your device.

1. On the Intu-Dialog workspace, click **Get started**.

2. Click **Dialog** in the navigation bar at the top.

3. Expand this branch of the dialog tree by clicking through the following dialog nodes: **#dialog** -> **@greeting** -> **@greeting:hello**. You may need to scroll down as you're clicking through. Note: Nodes may have the name "Untitled Node" as you are clicking on them. The node name is an optional field.

4. After clicking on the **@greeting:hello** node there are currently four responses for when a user says "hello". You can edit, remove, and add to these. For now, on a new line, after "why hello there[emote=wave_anim]" add "Hello Workshop Two participant!" directly below it.

5. Click the **Conversation bubble** icon on the top right banner to open a **Try it out** panel where you can test your new response.

6. Test your response.
  1. In the **Enter something to test your bot** field at the bottom of the **Try it out** panel, type Hello, and press **Enter**. 
  2. Submit hello a few times until your new response is returned. 
  
  		**Note:** Conversation may take a few minutes to train, as indicated by the pop-up message: **Watson is training on your recent changes** in purple.

7. Close the **Try it out** box by clicking on the **X** icon (directly above the  **Clear** icon).
  
## 3. <a name="adding-a-new-conversation-flow-with-a-new-intent,-new-entities,-and-new-dialog">Adding a new conversation flow with a new intent, new entities, and new dialog</a>

### A. Create a new intent

1. Click on **Intents** on the top navigation bar of the Conversation browser window you're currently in.

2. Click the green **Create new** button on the top left to create a new intent.

3. The hashtag (#) identifies intents. After the # for intent name, type **dialog_capitals**.

4. Click **Create** on the top right. The intent **#dialog_capitals** is at the top of your intents list.

5. Click **#question** in your intents list, and search for **capital**. (You can use **Ctrl + F** or **Cmd + F**). Four entries for "What is the capital of..." should be displayed.

6. Select the checkbox beside each of the four entries.

7. Scroll to the top of **#question**, and select **Move to...**.

8. In the **# Intent name** box that is displayed, start typing **dialog_capitals**, and select it when it appears in the dropdown list. **Note:** The move of the four entries from **#question** to **#dialog_capitals** may take a minute or more to update, and you may need to refresh your page. 

9. Scroll to the top of your intents list and click on **#dialog_capitals**, where you should see the four entries that you moved.

10. Click **#dialog_capitals**, and select **Add a new user example…**.

11. Type **What is the capital of Australia**, and press **Enter**. This entry is added to the list.

12. Repeat steps 8 and 9 for **What is the capital of Canada** and **What is the capital of New Zealand**.

### B. Create new entities

  1. Click **Entities** on the top navigation bar, and click **Create new**.
  
  2. Type the new entity **countries**, and click **Create**. The entity **@countries** is displayed in your Entities list.
  3. Click the @countries entity, and select **Add a new value**.
  4. Type **Australia**, and press **Enter**.
  5. Repeat steps 3 and 4 to add two new values for **Canada** and **New Zealand**.

### C. Create new dialog

When configuring the dialog flow, keep in mind as you click/navigate through the dialog flow, displayed in a tree view, an edit box will pop up on the right side of the screen. The tree view is what shows the summary of dialogs and the way elements relate to each other. As you click on the elements of the tree view, a box pops up on the right in which the condition and response can be configured.

1. Click **Dialog** on the top navigation bar, and click on any of the existing branches in grey in the tree view, e.g. **#dialog**. You will see a gray **+** displayed at the **bottom** of the box that has expanded.
  
2. Click on this bottom gray **+** and in the new edit box pops up on the right, type **#dialog_capitals**.
  
3. Click within the **#dialog_capitals** in the tree view. Now, click the gray **+** icon on the right side of the box. 
  
4. In the new edit box that pops up on the right, under **Enter new condition**, type **@countries:Australia**.

5. Click on the **#dialog_capitals** node box in the tree view. Next click on the **Jump to...** icon (the square icon with an arrow located to the right of the trashcan icon)

6. Immediately click on the **@countries:Australia** box in the tree view, then click on **Go to condition** which has just popped up.

7.  In the **@countries:Australia** edit box on the right, type in the response: **The capital of Australia is Canberra.**

8.	Click on the **@countries:Australia** where you just typed in the response: The capital of Australia is Canberra. The box will become outlined in green. Click on the gray **+** icon on the bottom of the box. 

9.	In the new box that appears, under **Enter a condition**, type in: **@countries:Canada,** and for the response: **The capital of Canada is Ottawa.**

10.	Repeat for **@countries:New Zealand**, where the response will be **The capital of New Zealand is Wellington.**

11. Click the **Conversation bubble** icon on the top right to open a **Try it out** panel where you can test your new response.

12.	Test your response.

	1. In the **Enter something to test your bot** field at the bottom of the **Try it out** panel, type: **What is the capital of Australia?** You should see the response: **The capital of Australia is Canberra.** Note that Conversation may take a few minutes to train, as indicated by the pop-up message: **Watson is training on your recent changes** in purple.
	2. Try asking for the capital of Canada and New Zealand.

## 4. <a name="configuring-Intu-to-use-your-new-Conversation-instance">Configuring Intu to use your new Conversation instance</a>

Your installation of Intu is preconfigured to use the default Conversation service. To configure Intu to use your new Conversation instance which you just modified, complete the following steps:

1.	After testing your changes in the previous step above, click on the three horizontal bars icon in the top left corner. Next click "Back to workspaces".

2.	This returns you to your Workspaces page, where you will see the Intu-Dialog Workspace box. Click on the **three vertical dots** in the top right hand corner of this box. Select **View details**.   

3.	Copy the Workspace ID.

4.	Return to the [Intu Gateway](https://rg-gateway.mybluemix.net/).

5.	Click on **MANAGE** on the left hand side navigation bar, and select **Services**. Select your Organization and Group in the top Filter by menu, if not already selected.

6.	For your instance of **ConversationV1** click the **Edit** button (pencil icon). Now paste your Workspace ID into the **self_dialog** parameter. 

	**Important:** As you are using the **Intu-Dialog** workspace, you do not 	need to worry about the **self_domain** parameter here. You can just leave 	**self_domain** with its default value, or delete it if you wish. If you 	were using your own, custom Conversation workspace (see **Challenge: Using 	your own custom Conversation workspace with Intu**), the **self_domain** 	parameter is where you would paste your Workspace ID. Please note that you have entered the workspace ID in the right place.
	
	In addition, do not change the service endpoint unless you are an enterprise user. 	Click **Save**.

7.	If Intu is already **running** (from Workshop 1), ask it a question, e.g. **What is the capital of Australia?**

8. If Intu is **not running, run Intu Manager**. For instructions on how to do so, [click here](../../../installation/running.md/#intu-through-manager).

## Challenge: Using your own custom Conversation workspace with Intu

1.  Make sure Intu is not running and navigate to your Conversation Service's dashboard of workspaces.

2.  If you are importing a workspace, simply import it the same way you imported the Starter Kit Workspace json.  If you are creating a brand new workspace, click the **Create** button. NOTE: When you import or create your new workspace, make sure it is present in the same Bluemix Conversation Service.

3. If you are creating your own brand new workspace, add *Intents*, *Entities*, and a *Dialog flow* in the same way you were instructed in this workshop.  Be as creative as you would like with this workspace.

4.  Get the Workspace Id -- Click on the three horizontal lines in the top left corner, click *Back to workspaces*. Once you see all your workspaces, click on the **three dots** in the top right hand corner of the newly created Workspace box. Select **View details**.

5.  Copy the Workspace ID.

6.  Return to the [Intu Gateway](https://rg-gateway.mybluemix.net/). 

7. Click on Manage => Services => Choose the appropriate Organization and Group => edit ConversationV1. If the **self_domain** key is still present, edit the value to point to your new Conversation workspace.  If the **self_domain** key is absent, => Click on the + sign. For the key field make it **self_domain** and for the value field make it your Workspace ID.

8. Click save.

9. Start Intu. Your new Conversation workspace will be picked up. Test it by asking questions specific to that Conversation workspace.



## Reminder: Update your services on the Gateway
You need to update your services on the Intu Gateway within 30 days of creating your organization.  For more details on how to do so, [click here](../../../installation/configuring.md).
