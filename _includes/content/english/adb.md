You can use Android Debug Bridge (ADB) to connect your development computer to an Amazon Fire TV device or stick for installing, testing, and debugging your apps.

Before you use ADB to connect to Fire TV or Fire TV stick, complete the following sections:

{% include note.html content="[Android Debug Bridge](https://developer.android.com/studio/command-line/adb.html) is provided by the Android Open Source Project, not by Amazon." %}

* TOC
{:toc}

## Step 1. Enable Debugging on Amazon Fire TV {#turnondebugging}

You must enable both ADB and debugging on your Fire TV device before you can connect to it:

1.  From the main screen of your Fire TV, select **Settings**.
2.  Select **Device** > **Developer Options**.
3.  Turn on **ADB Debugging**.
4.  (Optional) If you plan to connect your computer to your Fire TV device using a USB cable, turn on **USB Debugging**.
    
    {% include note.html content="When USB Debugging is enabled, the USB port is unavailable for other uses such as external storage or input devices. To re-enable the USB port, turn off USB debugging." %}
    
5.  Turn on **Apps from Unknown Sources**.

## Step 2. Set Up Android Debug Bridge {#setupadb}

Android Debug Bridge (ADB) is a command-line utility for running and managing Android apps on your device or emulator. ADB is available when you install Android Studio, but Windows users will need to install a special USB driver.

### Mac OS X

No action is required for ADB to work on Mac OS X.

### Windows

If you're on Windows and want to connect your computer to Fire TV through a USB cable, you need to install a special USB driver to connect your computer to a Fire TV device through ADB. The driver supports all the Fire TV platforms. To install the driver:

1.  [Download the USB file](https://s3.amazonaws.com/android-sdk-manager/redist/fire_devices_usb_driver.zip) and extract the zip file's contents.
2.  Double-click the **FireDevices_Drivers**.
3.  Complete the installation dialog boxes as prompted.

{% include note.html content="The USB driver is only certified through Windows 8.1. If you're on Windows 10, you will need to explicitly accept that you are installing from an \"un-certified source.\"" %}

## Step 3. Add Android Debug Bridge to Your Path {#addadbpath}

You need to add ADB to your PATH so you can more easily execute `adb` commands. (Your PATH is an environment variable used to specify the location of the program's executable. If you don't add ADB to your PATH, running `adb` commands will require you to browse to the `<Android SDK>/platform-tools` directory to run `adb`.)

### Mac OS X

To add ADB to your PATH on Mac:

1.  Get the path to your Android SDK platform-tools directory:

    1.  Open Android Studio and click the **SDK Manager** button {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_androidsdkmanagericon" type="png" %}.
    
        The location to your Android SDK appears near the top next to **Android SDK Location**. For example: `/Users/<your username>/Library/Android/sdk` 
    
        If this is your first time opening Android Studio, there isn't an SDK Manager button. Instead, at the Welcome to Android Studio prompt, click **Configure > SDK Manager** and provide the location to the Android SDK.
    
    2.  Copy the path to the SDK and paste it somewhere convenient, such as a text editor.
    3.  Add **/platform-tools** to the end of the path you copied in the previous step. ("platform-tools" is the directory containing the `adb` executable.)
    4.  Copy the full path to your clipboard.
    
2.  Use the following command to add adb to your **.bash_profile**, replacing `/Users/<your username>/Library/Android/sdk/platform-tools/` with your path to your Android SDK.  

    ```bash
    echo 'export PATH=$PATH:/Users/<your username>/Library/Android/sdk/platform-tools/' >> ~/.bash_profile
    ```
    
    Your .bash_profile file is usually in your user directory, which you can find by typing `cd ~` (change to your user directory). Then type `ls -a` (list all) to show all files, including hidden ones. 
       
    If the file isn't there, simply create one. You can then type `open .bash_profile` to see the paths listed. One of the lines should be something like this: `export PATH=$PATH:/Users/<your username>/Library/Android/sdk/platform-tools/`.

3.  Restart any terminal sessions, and then type **adb**. If you successfully added ADB to your path, you will see ADB help info rather than "command not found."


### Windows

To add ADB to your PATH on Windows:

1.  Get the path to your Android SDK platform-tools directory:

    1.  Open Android Studio and click the **SDK Manager** button {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_androidsdkmanagericon" type="png" %}.

        The location to your Android SDK appears near the top next to **Android SDK Location**. For example: `C:\Users\<your user name>\AppData\Local\Android\Sdk\platform-tools`

        If this is your first time opening Android Studio, there isn't an SDK Manager button. Instead, at the Welcome to Android Studio prompt, click **Configure > SDK Manager** and provide the location to the Android SDK.

    2.  Copy the path to the SDK and paste it somewhere convenient, such as a text editor.
    3.  Add **/platform-tools** to the end of the path you copied in the previous step. ("platform-tools" is the directory containing the `adb` executable.)
    4.  Copy the full path to your clipboard.
    
2.  Click **Start** and type **view advanced system settings** in the search box.
3.  Click **View advanced system settings**.
4.  When the System Settings dialog opens, click the **Environment Variables** button.
5.  Under *System Variables* (the lower pane), select **Path** and click **Edit**.
6.  Do one of the following:
    
    * On *Windows 7 or 8*, move your cursor to the farthest position on the right, type `;` and then press **Ctrl+V** to insert the path to your SDK that you copied earlier. It may look like this: `;C:\Users\<your user name>\AppData\Local\Android\Sdk\platform-tools`. Click **OK** on each of the three open dialog boxes to close them.
    * *On Windows 10*, click the **New** button and add this location.  
    
8.  Restart any terminal sessions, and then type **adb**. If you successfully added ADB to your path, you will see ADB help info rather than "command not found."

## Step 4: Options for Connecting ADB {#connectingadboptions}

You can use ADB to connect the Fire TV or Fire TV stick to your computer in two ways:

*   [Connect ADB Through the Network](#networkconnect). With this option, you connect using either a wired Ethernet or WiFi network connection. Both your computer and the Fire TV device must be on the same network for a network ADB connection to work.
*   [Connect ADB Through USB](#usbconnect). With this option, you use an A-to-A USB cable to establish a direct USB connection.

{% include note.html content="The following instructions apply to Generation 2 devices that have a more updated user 
interface. If you have a generation 1 device, the menu locations differ slightly." %} 

### Connect ADB Through the Network {#networkconnect}

You need the IP address of your Fire TV device on your network to connect ADB to it.

1.  If you haven't already done so, connect your Fire TV device to a network (the same network that your computer is on). To do this, from the Fire TV home screen, go to **Settings > Network** and select a network.
2.  From the Fire TV home screen, select **Settings**.
3.  Go to **Device > About > Network**. Make a note of the IP address listed on this screen.
    
    {% include tip.html content="Copy this IP address onto a convenient and visible place if you plan to connect regularly via the network." %}
    
4.  Open a terminal window.
    
    On a Mac, you can open Terminal by pressing **Cmd + spacebar** and then typing **Terminal.** On Windows, you open the Command Prompt usually by typing **cmd** in your program search. (The exact steps vary based on your Windows version.)
    
5.  Make sure your Fire TV device and your computer are on the same network. You can use either a wifi network or a wired network.
6.  Run the following commands, where `<ipaddress>` is the IP address of the Fire TV device noted in the previous section:

    ```
    adb kill-server
    adb start-server
    adb connect <ipaddress>
    ```

    {% include note.html content="Make sure you added ADB to your PATH, as described in [Add Android Debug Bridge to Your Path](#addadbpath). Otherwise you will need to `cd` (change directories) to the platform-tools directory first and use`./adb` on a Mac or `adb` on Windows to run `adb` commands." %}

    If the connection was successful, ADB responds with the message:

    ```
    connected to <ipaddress>:5555
    ```

7.  Verify that the Fire TV device appears in the list of devices:

    ```
    adb devices
    ```

    ADB responds with the message:

    ```
    List of devices attached
    <ipaddress>:5555  device
    ````

If the serial number does not appear after running `adb devices`, [update ADB](#updateadb) and then repeat the steps here.

{% include tip.html content="You don't always need to kill and start the server with ADB. Usually you can just run the `adb connect <ipaddress>` command." %}

### Connect ADB Through USB {#usbconnect}

To connect your computer to Fire TV through USB, you need an A-to-A USB cable. Note that you must have a Fire TV, not a Fire TV Stick, because only Fire TV (the box) has the USB cable port.

1.  If you're on Windows, install the USB driver as described in [Set Up Android Debug Bridge](#setupadb).
2.  Connect your Fire TV to a USB port on your computer.
2.  Run the following commands:

    ```
    adb kill-server
    adb start-server
    adb devices
    ```

After the last command, ADB responds with the following message, where `<serialno>` is the serial number of the device:

```bash
List of devices attached
<serialno> device
```

If the serial number does not appear after running `adb devices`, [update ADB](#updateadb) and then repeat the steps here.

After ADB connects your computer to your Fire TV device, when you open Android Studio and click the **Run App** button, you'll be prompted with a dialog box like this:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_aftsoptions" type="png" max-width="60%" border="true" %}

In this example, "Amazon AFTS" refers to Fire TV (Generation 2).

## Update ADB {#updateadb}

If your device isn't listed when you type `adb devices`, try updating ADB:

1.  Open a terminal session and navigate to your Android SDK **tools** directory.

    In Android Studio, you can see the installation directory by going to **Tools > Android > SDK Manager** or by clicking the **SDK Manager** button {% include inline_image.html file="firetv/getting_started/images/androidsdkmanagericon" type="png" %}. The directory is listed next to **Android SDK Location**.

2.  In your Terminal or Command Prompt, change directories (`cd`) to your Android SDK directory, and then `cd` one level down to **tools**. (The tools directory is contained within the SDK directory.)

2.  Run one of the following commands to update ADB:

    **Mac**:

    ```
    ./android update adb
    ```

    **Windows**:

    ```
    android update adb
    ```

    You receive a message that says ADB has been updated.

## Run Your App

After you have connected your computer to your Fire TV device through ADB, you can build and run your app on the Fire TV device. In Android Studio, click the **Run App** button {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_runappbutton" type="png" %}.

## Troubleshooting

 If you receive a message such as the following:

 ```
 unable to connect to 192.168.0.6:5555: Operation timed out
 ```

or

```
error: device offline
```

try any of the doing the following:

* Make sure both Fire TV and your computer are using the same network and router.
* When connecting wireless with `adb connect <ipaddress>`, make sure you're typing the IP address correctly, with all the required dots `.`
* Close Android Studio and any other emulators or USB cable connections.
* Kill (`adb kill-server`) and restart (`adb start-server`) the server.
* Restart Fire TV (Settings > Device [or System] > Restart).
* Restart your router.
* [Update ADB](#updateadb).
* Search online for the error message you're seeing.
