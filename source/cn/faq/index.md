---
title: DJI Windows SDK FAQ
date: 2018-11-27
keywords: [FAQ, stackOverFlow]
---

## Table of Contents

**Getting Started**

* [How can I become a DJI Developer?](#how-can-i-become-a-dji-developer)
* [Where are the DJI Windows SDK Resources?](#where-are-the-dji-windows-sdk-resources)
* [Are there any tutorials for the DJI Windows SDK?](#are-there-any-tutorials-for-the-dji-windows-sdk)
* [Where can I get the Windows SDK API Reference?](#where-can-i-get-the-windows-sdk-api-reference)
* [If I have questions, where can I get help?](#if-i-have-questions-where-can-i-get-help)
* [How can I import and activate the DJI Windows SDK in my own project?](#how-can-i-import-and-activate-the-dji-windows-sdk-in-my-own-project)

**Product Related**

* [Can I access SD card data without an SD card reader?](#can-i-access-sd-card-data-without-an-sd-card-reader)
* [How can I update my DJI Aircraft or OSMO's firmware?](#how-can-i-update-my-dji-aircraft-or-osmo-s-firmware)
* [How can I Unlock Travel Mode for Inspire 1?](#how-can-i-unlock-travel-mode-for-inspire-1)
* [How do I link the Remote Controller to an aircraft?](#how-do-i-link-the-remote-controller-to-an-aircraft)
* [Do DJI products use WGS84 or NAD83 as the coordinate system?](#do-dji-products-use-wgs84-or-nad83-as-the-coordinate-system)

## Getting Started

### How can I become a DJI Developer?

Becoming a DJI developer is easy. Please see [here](../application-development-workflow/workflow-register.html) for details.

### Where are the DJI Windows SDK Resources?

All <a href="http://developer.dji.com/windows-sdk/documentation" target="_blank"> documentation </a> can be found on the DJI developer website.

The SDK can be downloaded from the <a href="https://developer.dji.com/windows-sdk/downloads" target="_blank"> website </a>. 

All sample code referenced in the tutorials is available on <a href="https://github.com/dji-sdk/Windows-SDK" target="_blank"> Github </a>.

### Are there any tutorials for the DJI Windows SDK?

Tutorials of Windows SDK are available here [Windows SDK Tutorials](../tutorials/index.html).

### Where can I get the Windows SDK API Reference?

- <a href="https://developer.dji.com/api-reference/windows-api/index.html" target="_blank">Windows API Reference</a>

### If I have questions, where can I get help?

You can use the following methods to get help:

- StackOverFlow 

  Post questions in StackOverFlow with DJI SDK tag: <a href="http://stackoverflow.com/questions/tagged/dji-sdk" target="_blank">dji-sdk</a>
  
- Send Email

  If you prefer email, please send to <dev@dji.com> for help.

### How can I import and activate the DJI Windows SDK in my own project?

The following link of the documentation can help:

- [Integrate WSDK into Application](../application-development-workflow/workflow-integrate.html)

## Product Related

### Can I access SD card data without an SD card reader?

Yes, you can use a USB cable to connect between DJI Products and your computer to access the SD card data directly. Each product's manual illustrates the location of the USB port on the product.

### How can I update my DJI Aircraft or OSMO's firmware?
  
Each product's web page at <a href="http://www.dji.com" target="_blank">www.dji.com</a> has a firmware installation manual in the **Downloads** section.

### How can I Unlock Travel Mode for Inspire 1?

The aircraft is in Travel Mode during delivery. In this mode, the landing gear is half way between the retracted (up) and deployed (down) state. The landing gear needs to be deployed to attach a camera. Follow these steps to deploy the landing gear (put the aircraft in Landing Mode):

1. Place the Inspire on a flat surface clear of any obstructions (the propellors do not need to be installed).
2. Insert the battery into the battery compartment.
3. Power on the Remote Controller and the Inspire.
4. Toggle the Transformation Switch up and down at least four times.
5. The aircraft body will slowly raise as the landing gear deploys.
6. Power off the aircraft.

 ![](../../images/faq/unlockTravelMode_en.png)

> **Note:** 
>- If you have purchased the dual remote controller version, you must use the Master remote controller to deactivate Travel Mode. 
>- Be sure to remove the camera from the aircraft before switch from Landing Mode to Travel Mode. 
>- The ultrasonic sensor underneath the aircraft is used to determine when the landing gear is at the right position. Smooth reflective surfaces will work better than rough, sound-absorbing surfaces (e.g. carpet). 
 
### Do DJI products use WGS84 or NAD83 as the coordinate system?

DJI products use **WGS84** as the coordinate system.

