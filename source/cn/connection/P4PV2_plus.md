---
title: Phantom 4 Pro+ V2.0 Connection Guidance
date: 2019-01-16
keywords: [Windows SDK, Phantom 4 Pro+ V2.0]
---

*If you come across any mistakes or bugs in this tutorial, please let us know by sending emails to dev@dji.com. Please feel free to send us Github pull request and help us fix any issues.*

---

## Preparation

1. Phantom 4 Pro V2.0 aircraft and remote controller **with screen**. Make sure they are both **in newest firmware version**. Use **DJI Go App** on mobile phone to check if your devices need updating.
2. Windows SDK 0.2.0 or later.
3. Micro-USB cable.

## DJI Assistant 2

Make sure that you have installed DJI Assistant 2 and have enabled all the driver intallation while installing DJI Assistant 2.

With DJI Assistant 2 and connected Phantom 4 Pro+ V2.0, check **Device Manager** of your computer. If there is a yellow warning icon in the DJI item inside **Ports (COM & LPT)**, you need to install or re-install DJI Assistant 2 and make sure you have enabled all the driver intallation while installing.

## WinUSB Driver

1. Install open-source software [zadig.exe](https://zadig.akeo.ie/) and open it.
2. Connect your Phantom 4 Pro+ V2.0 remote controller.
3. Selete device named **GL300K** and install its WinUSB driver.


## USB Authority Configuration in UWP

Configure serial port authority in order to connect the application to the remote controller. Right-click on Package.appxmanifest, select View Code, add the following code in the Capabilities element.

```XML

<DeviceCapability Name="usb">
    <Device Id="vidpid:2ca3 1006">
        <Function Type="name:vendorSpecific" />
    </Device>
</DeviceCapability>

<DeviceCapability Name="serialcommunication">
    <Device Id="vidpid:2ca3 1006">
        <Function Type="name:serialPort" />
    </Device>
</DeviceCapability>
```

## Key Points of Connecting

1. Make sure that before connecting using your cable, your P4P V2.0 remote controller is **off**.
2. Connect the remote controller to your computer using **Micro-USB cable**.
3. Turn on your remote controller.
4. Make sure you have not open the DJI software like DJI Assistant 2. DJI Windows SDK Applications are not able to communicate with the aircrafts at the same time.
