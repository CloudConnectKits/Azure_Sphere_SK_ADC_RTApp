
# Sample: Inter-core communication ADC

This sample demonstrates how to exchange messages between applications running on the high-level and real-time capable cores.

Once per second the high-level application sends a message "Read-ADC-%d", where %d is an incrementing counter to the real-time capable application. The real-time capable application prints the received data, reads the ADC channel 0 (light sensor) and sends it back to the high-level application.

The high-level application uses the following Azure Sphere libraries and includes [beta APIs](https://docs.microsoft.com/azure-sphere/app-development/use-beta):

|Library   |Purpose  |
|---------|---------|
|application.h |Communicates with and controls real-time capable applications |
|log.h |Displays messages in the Visual Studio Device Output window during debugging |

The real-time capable features used in the sample are in Beta.


To use this sample, clone the repository locally if you haven't already done so:

```
git clone https://github.com/Azure/azure-sphere-samples.git
```

## Prerequisites

1. [Avnet Azure Sphere MT3620 Starter Kit](http://cloudconnectkits.org/product/azure-sphere-starter-kit).
1. A breakout board and USB-to-serial adapter (for example, [FTDI Friend](https://www.digikey.com/catalog/en/partgroup/ftdi-friend/60311)) to connect the real-time capable core UART to a USB port on your PC. 
1. A terminal emulator (such as Telnet or [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/.)) to display the output.


## To build and run the sample

**Prep your device**

1. Ensure that your Azure Sphere device is connected to your PC, and your PC is connected to the internet.
1. Even if you've performed this set up previously, ensure that you have Azure Sphere SDK version 19.05 or above. In an Azure Sphere Developer Command Prompt, run **azsphere show-version** to check. Download and install the [latest SDK](https://aka.ms/AzureSphereSDKDownload) as needed.
1. Right-click the Azure Sphere Developer Command Prompt shortcut and select **More&gt;Run as administrator**.
1. At the command prompt, issue the following command:
   ```sh
   azsphere dev prep-debug --EnableRTCoreDebugging
   ```

   This command must be run as administrator when you enable real-time capable core debugging because it installs USB drivers for the debugger.
1. Close the window after the command completes because administrator privilege is no longer required.  
    **Note:** As a best practice, you should always use the lowest privilege that can accomplish a task.

**Set up hardware to display output**

To set up the hardware to display output from the Hello World sample, follow these steps.
1. Connect GND on the breakout adapter to Header 3, pin 2 (GND) on the MT3620 RDB.
1. Connect RX on the breakout adapter to Header 3, pin 6 (real-time core TX) on the MT3620 RDB.
1. Attach the breakout adapter to a USB port on your PC.
1. Determine which COM port the adapter uses on the PC.
    1. Start Device Manager. 
    1. Select **View&gt;Devices by container**, 
    1. Look for your adapter and note the number of the assigned COM port.
1. On the PC, start the terminal emulator and open a serial terminal with the following settings: 115200-8-N-1 and the COM port assigned to your adapter .

**Build and deploy the real-time capable application**
  
1. Start Visual Studio. From the **File** menu, select **Open&gt;CMake...** and navigate to the folder that contains the sample.
1. Select the file CMakeLists.txt and then click **Open**
1. From the **CMake** menu (if present), select **Build All**. If the menu is not present, open Solution Explorer, right-click the CMakeLists.txt file, and select **Build**. This step automatically performs the manual packaging steps. The output location of the Azure Sphere application appears in the Output window.
1. From the **Select Startup Item** menu, on the tool bar, select **GDB Debugger (RTCore)**.
1. Press F5 to start the application with debugging.

**Build and deploy the high level application**
1. Start another instance of Visual Studio.
1. Open Project/Solution and navigate to the folder that contains the sample (../AvnetStarterKitOLED)
1. Select AvnetStarterKitOLED.sln and click **Open**
1. From the Build menu select **Build Solution**
1. Press F5 to start the application with debugging.

The high-level application output will be displayed in the Output window in Visual Studio.
```
Remote debugging from host 192.168.35.1
High-level intercore application.
Sends data to, and receives data from the real-time core.
Sending: Read-ADC-0
Received 4 bytes: Light intensity: 316.171539 Lux
Sending: Read-ADC-1
Received 4 bytes: Light intensity: 313.829498 Lux
Sending: Read-ADC-2
```

The real-time core application output will be sent to the serial terminal for display.
```
IntercoreCommsADC_RTApp_MT3620_BareMetal
App built on: Jun  7 2019, 17:33:13
Received message of 30bytes:
  Component Id (16 bytes): 685f13af-25a5-40b2-8dd8-8cbc253ecbd8
  Reserved (4 bytes): 00280003
  Payload (10 bytes as hex): 52:65:61:64:2d:41:44:43:2d:30
  Payload (10 bytes as text): Read-ADC-0
ADC channel 0: 0.149 V
Received message of 30bytes:
  Component Id (16 bytes): 685f13af-25a5-40b2-8dd8-8cbc253ecbd8
  Reserved (4 bytes): 00000032
  Payload (10 bytes as hex): 52:65:61:64:2d:41:44:43:2d:31
  Payload (10 bytes as text): Read-ADC-1
ADC channel 0: 0.151 V
Received message of 30bytes:
  Component Id (16 bytes): 685f13af-25a5-40b2-8dd8-8cbc253ecbd8
  Reserved (4 bytes): 00000032
  Payload (10 bytes as hex): 52:65:61:64:2d:41:44:43:2d:32
  Payload (10 bytes as text): Read-ADC-2
ADC channel 0: 0.148 V
```
## License
For details on license, see LICENSE.txt in this directory.

## Code of Conduct
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).