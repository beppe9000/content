---
layout: sample
title: IoT Onboarding
description: Use Onboarding to connect a device to a Wi-Fi network using AllJoyn and a device's SoftAP.
keyword: intermediate, alljoyn, onboarding, windows 10 iot core
permalink: /en-US/Samples/IotOnboarding.htm
samplelink: https://github.com/ms-iot/samples/tree/develop/IotOnboarding
lang: en-US
---

# IotOnboarding Source Code

Source code for the IoT Onboarding sample is available by downloading all IoT samples from [here](https://github.com/ms-iot/samples). 

## Prerequisites

1. An IoT device with Wi-Fi capability. (e.g. RPi3, RPi2 with Wi-Fi adapter, etc).
2. An available Wi-Fi network for onboarding your IoT device.  Wi-Fi network may provide Open Authentication or may be secured with WPA2-PSK authentication.  Substitute a Personal Hotspot from a cell phone if desired.
3. A laptop with Wi-Fi capability running Windows 10.  Substitute with another Windows 10 IoT Device if desired.
4. Visual Studio 2015 or newer running on laptop or desktop.

### Step 1: IoT Device Setup
**Note:** It is recommended to start with a clean O/S install of your IoT Device.  Your IoT Device must not have been connected to a Wi-Fi network.  Remove all Wi-Fi Network Profiles from your Iot Device if not starting from a clean O/S install.
1. Install a clean O/S to your IoT Device.   If using IoT Dashboard, deselect the "Wi-Fi Network Connection" checkbox when preparing your SD Card.
2. If your IoT Device needs an external Wi-Fi adapter, attach it now.
3. Boot your IoT Device with the clean O/S install.
4. For Windows 10 IoT builds 10.0.14393 or earlier, the IoT Onboarding sample must be replaced.  
**Note:** These steps are not required for newer Windows 10 IoT Builds.

    1. Connect your Iot Device to a wired LAN connection shared with your development system.
    2. Copy or clone samples from [here](https://github.com/ms-iot/samples) to your development system.
    3. Build the IotOnboarding Solution (c:\samples\IotOnboarding\IotOnboarding.sln) for your Iot Device's platform (e.g. ARM, x86, x64).
    4. Deploy the IotOnboarding Solution to your IoT Device.
    5. Disconnect your IoT Device from the wired LAN connection.

### Step 2:  Wi-Fi Network Setup
1.  Ensure your Wi-Fi Router is on and configured to allow a network connection.

### Step 3:  Laptop Setup
1. Download and unzip the Windows Universal Samples source from here  [here](https://github.com/Microsoft/Windows-universal-samples).
2. Within the Windows Universal Sample, find the AllJoyn Consumer Experiences solution (\samples\AllJoyn\ConsumerExperiences\cs\AllJoynConsumerExperiences.sln) and open with Visual Studio.  
**Note:** This sample will be used to onboard your IoT Device, however an alternate AllJoyn Onboarding Consumer may be used if desired.
3. Build the AllJoyn Consumer Experiences Sample to run on your laptop. (For example, select Release/x64, Release/x86 or Release/Arm as necessary)
4. If not already, disconnect your laptop from all Wi-Fi and wired networks. 

### Step 4: Onboard your IoT Device 
1. Run the AllJoyn Consumer Experiences Sample on your laptop.
2. From the menu on the left, Select Scenario "2) Onboarding Consumer".
3. Select "Physical Device".
4. Press the "Scan" button.
5. Select a laptop Wi-Fi Adapter from the drop-down list.  After selection, another new drop-down should appear.
6. Select the Soft-AP for your IoT Device from the new drop-down list (e.g. AJ_YourDeviceName_XXXXXXXXXXXX) and press Connect.  
**Note:**  The the X's represent the MAC address of your device's Wi-Fi Adapter.
7.	Enter the password for your IoT Device's Soft-AP and press Connect again.  
**Note:** The default password is 'p@ssw0rd' without the quotes.  Learn more about this below.
8. The AllJoyn Consumer Experiences application will connect with your device's Soft-AP and will then connect to your device's AllJoyn Onboarding Producer.  When completed successfully, the AllJoyn Consumer Experiences app queries your IoT Device for the list of Wi-Fi networks that are visible to your IoT Device.  The list will be shown in the SSID dropdown.
9.	To Onboard your IoT Device to a Wi-Fi network,  select the Wi-Fi network's SSID from the SSID drop-down menu.
10.  Enter a password for that network (if necessary).
11. Press the Onboard button.  
12. If the Wi-Fi network credentials are valid, your IoT device will connect to your Wi-Fi network.

### IotOnboarding Customization
The IotOnboarding code may be modified as needed for your use, but there are several basic settings that may be customized through the default configuration file "config.xml".  Note that for experimentation, some of these values may be configured through your IoT Device's Web Interface when your IoT Device is connected to a wired LAN.

|Setting                                                            |Description|
|------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|*AllJoyn Onboarding Settings*                      | |
|Enabled                                                           | Enables or Disables AllJoyn Onboarding|
|DefaultDescription                                         |Device description shared through the AllJoyn About Data |
|DefaultManufacturer                                      |Device manufacturer shared through the AllJoyn About Data|
|ModelNumber                                                |Device model number shared through the AllJoyn About Data|
|Psk                                                                   |Specifies the AllJoyn Onboarding Producer Authentication PSK.  When this value is empty, the AllJoyn Onboarding Producer uses ECDHE_NULL authentication, otherwise ECDHE_PSK authentication is used.|
| *SoftAP Settings*                                           | |
| Enabled                                                          |Enables or Disables the device's Soft AP (**Note:** if AllJoynOnboarding:Enabled is true, this value is ignored and assumed to be true)|
| Password                                                        |The Soft-AP password.  This value must conform to the WPA2-PSK requirements (It must be between 8 and 63 printable ASCII characters and is not optional)|
| Ssid                                                                 |The Soft-AP SSID.  This value will be prefixed with AJ_ if AllJoyn Onboarding is enabled.  The value is suffixed with the Wi-Fi Adapter's MAC Address.|

### Additional Information
#### Multi-Level Authentication
There are two levels of authentication for Iot Onboarding.  The first level establishes connectivity with the SoftAP.  Once connected to the Soft-AP, the second level permits connectivity with the AllJoyn Producer.  To reduce the number of authentication requests, the second level is disabled in the config.xml file. (Refer to the [AllJoynOnboarding:Psk setting](#IotOnboarding-Customization).)

#### Soft-AP Password Broadcast
The Soft-AP created by the IotOnboarding application utilizes WPA2-PSK authentication.  Currently it is not possible to create a Soft-AP with Open Authentication.  To simulate Open Authentication, the password used for IoT Onboarding is shared through the Soft-AP's Information Elements so that supporting AllJoyn Consumer applications may connect to an IoT Device without prompting for a password.  This functionality may be disabled through a code change in "OnboardingAccessPoint.cs".

#### Soft-AP and Wi-Fi Profiles
The Soft-AP and AllJoyn Onboarding settings are ignored when a Wi-Fi profile is detected.  In theory, once a device has a Wi-Fi profile configured, there is no longer a need to Onboard the device.  This behavior can be changed by modifying the control logic in OnboardingService.cs.  For example, a hardware switch could be polled that re-enables the Soft-AP when a button is pressed.  To change the logic, replace the code that sets the "_state" variable to "OnboardingState.ConfiguredValidated" when a Wi-Fi profile is found.

