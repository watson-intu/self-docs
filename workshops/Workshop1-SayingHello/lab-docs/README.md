# Workshop 1: Saying Hello!

In this workshop, you install Intu on your laptop and get it to say hello to you.

**Before you begin:** 

* You must have a Mac or Windows laptop.
* Turn on your laptop's microphone.

Complete the following tasks:

1. [Gaining access to the Intu Gateway](#gaining-access-to-the-watson-intu-gateway)
2. [Downloading Intu](#downloading-intu)
3. [Installing Intu](#installing-intu)
4. [Saying hello to Intu](#saying-hello-to-intu)

## 1. <a name="gaining-access-to-the-watson-intu-gateway">Gaining access to the Intu Gateway</a>

1. Go to the [Intu Gateway](https://rg-gateway.mybluemix.net/).
2. Click **Log In** and specify your IBM Bluemix credentials. If you don't have IBM Bluemix account, create one at [IBM Bluemix](https://console.ng.bluemix.net/) then return to the Gateway and login.
3. In the **Organization** field, specify the name of the organization that you represent.
4. Click **Submit**. After your request has been submitted you will be logged in and receive a confirmation email.
5. The Intu Downloads page is displayed.

## 2. <a name="downloading-intu">Downloading Intu</a>

1. On the Intu Downloads page, click on the topmost download arrow for **Download Intu Launcher**. **Intu-Tooling-Launcher-OSX64.zip** or **Intu-Tooling-Launcher-Win64.zip** will start to download, depending on your machine.

2. Extract the Intu-Tooling-Launcher-OSX64 or Intu-Tooling-Launcher-Win64 (it may do so automatically). 


## 3. <a name="installing-intu">Installing Intu</a>

1. Open your directory used for Downloads, where you will find the **Intu Manager**. Open Intu Manager.
	
	**For Mac users**, right-click on the Intu Manager and select **Open**.
	
	**For Windows users**, double-click on the Intu Manager to open it. 
	
	If a security warning is displayed, accept the risk and open the file.

    It will download and extract the latest version of **Intu Manager** to your local computer, then it will run **Intu Manager**. 

2. Select the **Windowed** checkbox, accept the other default values, and click **Play!**. If a security warning is displayed, accept the risk. The Intu Manager page is now displayed. 

3. Wait until you see a browser appear inside the application. At that point, log into Bluemix through said browser, and you will be automatically logged into Intu Manager.

4. The Intu Manager's main screen will start. Click **Install**.

5. A page displays options for where you can choose to install Intu. For this workshop, select **Local Machine**, and click **Next**. A page displays your Organizations and Groups in the dropdown menus. Default values are already selected. If you have multiple Organizations, Groups or both and want to use them, manually select them from the menus.

6. Click **Install**. Installing Intu takes a few minutes. During the installation process, if you see one or more security prompts, make sure to allow access.

7. After Intu installation is **Done!** and **100%** installed, if your microphone is on, say "hi,” or “how are you?", and hear Intu return a greeting. You will see a message "Intu install complete, loading Monitor in..." You can wait for the Intu Manager to load automatically or click **Intu Monitor**.

8. The Intu Manager window is displayed, your Organization and Group selection can be seen in the top left. Use the drop down menus to select the Organization and Group you used for the installation. 

9. When an Organization and Group are selected. A **red** dot will display while your Intu Manager tries to establish a connection. When the connection is made, the status icon of the device you installed Intu on turns **green**. 

10. Click on your device. A representation of a brain is displayed, and you will also see a **Menu** on the bottom left of the window.

Now that Intu is installed successfully, explore how you can test conversations in speech and text with Intu in the next task.

## 4. <a name="saying-hello-to-intu">Saying hello to Intu</a>

1. In the Intu Manager, click **Menu**, and then click **Conversation** so that you can see what happens when you talk to Intu. Ensure that the status icon of the device you have installed Intu on is green. If it is red, [submit an issue](https://github.com/watson-intu/self-sdk/issues).

2. Test Intu through speech and text:
 * Say "hello" into your microphone. Intu speaks a greeting.
 * In the empty field on the right side of the window, type "hello," and click **Ask**. Intu returns a text greeting in the window.

## Challenge: Configuring the Intu Avatar for your instance

To complete this challenge, do the tasks in [Workshop7 -- Installing, Configuring, and Running the Intu Avatar](../../../workshops/Workshop7-InstallingConfiguringAndRunningTheIntuAvatar/lab-docs/README.md).

## Reminder: Update your services on the Gateway

You need to update your services on the Intu Gateway within 30 days of creating your organization.  For more details on how to do so, [click here](../../../installation/configuring.md).

