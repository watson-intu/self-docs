# Workshop 7: Installing, configuring, and running the Intu Avatar

In this workshop, you install, configure, and run the Intu Avatar. You can use the avatar when you do not have a visual embodiment for Intu. Additionally, the avatar is useful in noisy environments because you can interact with it by using the keyboard instead of the microphone.

**Before you begin:** You must have a Mac or Windows laptop, and you must have completed Workshop 1: Say Hello!. You will notice that Intu and Self are used interchangeably. Self is the technical name for Intu.

Complete the following tasks:

1. [Installing the Intu Avatar and configuring it to your instance](#installing-the-intu-avatar-and-configuring-it-to-your-instance)
2. [Running the Intu Avatar](#running-the-intu-avatar)

## Installing the Intu Avatar and configuring it to your instance

1. Return to the Intu Gateway, and click **Downloads** on the left side of the page.

2. Download the Intu Avatar and extract the files into a working directory.

3. On the Gateway page, click **View Credentials** on the left side of the page.

4. In the **Filter By** fields, select your Organization and Group, and click **Get Credentials**. In the next steps, you replace the credentials in the avatar configuration file with your own credentials.

5. Open the avatar configuration file.
  * If you're using a Mac, Ctrl+click `intu_avatar` and select **Show Package Contents**. Open **Contents** -> **Resources** -> **Data** -> **StreamingAssets** ->, and open `Config.json` in your favorite text editor.
  * If you're using Windows, open **Self-Avatar-Win64** -> **intu_****avatar_Data** -> **StreamingAssets**, and open `Config.json` in your favorite text editor.

6. Locate the `SelfID` key, and delete only its value. **Important**: Do not delete the set of quotation marks.

7. Return to your credentials in the Gateway, and copy the value of `m_BearerToken`.

8. Return to the `Config.json` file in your text editor, and paste the value of `m_BearerToken` over the value of the `BearerToken` key.

9. Return to your credentials in the Gateway, and copy the value of `m_GroupId`.

10. Return to the `Config.json` file in your text editor, and paste the value of `m_GroupId` over the value of the `GroupID` key.

11. Return to your credentials in the Gateway, and copy the value of `m_OrgId`.

12. Return to the `Config.json` file in your text editor, and paste the value of `m_OrgId` over the value of the `OrgID` key.

13. Save the changes you made in the `Config.json` file.
  
## Running the Intu Avatar
1. Confirm Intu is running on your machine. If Intu is not running, run **Intu Manager**.

 	1. Navigate to the directory where you installed the Intu Manager from Workshop 1 and run the application. (If you're using a **Mac**, Ctrl+click on the Intu Manager and select Open. If you're using **Windows**, double-click on the Intu Manager to run it.) 
 	2. If a security warning is displayed, accept the risk and open the file.
 	3. Make sure the **Windowed** checkbox is selected, accept the other default values, and click **Play!**. 
 	4. When prompted to login, enter your Bluemix credentials and continue.
 	5. Your Organization and Group should be pre-selected in the dropdown menu and your Intu instance should appear with a Green dot indicating that it is running.

2. Navigate back to the directory where you extracted the Intu Avatar, and Ctrl+click on the `intu_avatar` application. If a security warning is displayed, click **Ok** to continue run the application.

3. Select the **Windowed** option, accept the other default values, and click **Play!**.

4. Test Intu by saying hello into the microphone or by typing hello in the **Input Message Here** field. You can also talk to your Avatar by using the **Conversation** box as you learned in Workshop 1.

NOTE: In case the avatar fails to run correctly, below is a workaround

1. Make sure the Intu instance is not running. Keep the configuration values **blank** which were added in steps 5-13 (You can keep the quotes).

2. Start Intu and see that the topics are listed in 127.0.0.1:9443/info

3. Start the Intu Avatar.


