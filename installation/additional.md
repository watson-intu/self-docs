# Additional requirements for Intu
If you plan on using a Mac to run or build Intu, you will be required to install a few things:

1. [Homebrew](#homebrew)
2. [Anaconda](#anaconda) (Python)

## <a name="homebrew">Homebrew</a>

### Check if Homebrew is installed
1. Verify you have **Homebrew** installed by running `brew --version` in your terminal
2. If a version number is displayed, **Homebrew** is installed and you can skip the **Install Homebrew** section

### Install Homebrew
1. Install Homebrew by completing the following steps:
   1. Open a new terminal window, and run the command:
   ```
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```
   
   	2. The following message is displayed:
   ```
   Press RETURN to continue or any other key to abort
   ```
   
   3. Press **Return** or **Enter**. A prompt for your laptop's password is displayed.
   4. Specify your password, and press **Return** or **Enter**. If you have **macOS Sierra**, the following message is displayed:
   
   		```
   HEAD is now at 89fd34b Merge pull request #1368 from MikeMcQuaid/build-options-file
   Error: /usr/local/Cellar is not writable. You should change the
   ownership and permissions of /usr/local/Cellar back to your
   user account:
   sudo chown -R $(whoami) /usr/local/Cellar
   Failed during: /usr/local/bin/brew update --force
   		```
   
   		Run: `sudo chown -R $(whoami) /usr/local/Cellar`
   4. Now repeat step 1 by running: 
    ```
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```


## <a name="anaconda">Anaconda</a>

### Check to see if you already have Anaconda installed
1. In your terminal, run `python --version`
2. If the version displayed matches `Python 2.x.x :: Anaconda 4.x.x (x86_64)`, you have Anaconda installed (Note: the x's can be any number)
3. If not, download Anaconda following the instructions below

### Download Anaconda
1. Download **Anaconda 4.2.0 Python 2.7 version** by using the **Graphical Installer**. It is required to correctly configure pip.
   1. Open a new browser window and [download Anaconda 4.2.0 Python 2.7 version](https://www.continuum.io/downloads).
   2. Click the solid blue GRAPHICAL INSTALLER button for Python 2.7 Version. It should be 403 MB. The .pkg file downloads.
   3. After the file is downloaded, double-click it, and follow the prompts to install Anaconda.
   4. Open a **new** Terminal window and make sure your version of Python has been successfully updated by running the command: `python --version` 
 	
 		You should see: `Python 2.7.12 :: Anaconda 4.2.0 (x86_64)`