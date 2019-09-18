---
title: Integrate DJIVideoParser Project into DJI Windows SDK Application
version: v0.3.2
date: 2019-09-17
github: https://github.com/DJI-Windows-SDK-Tutorials/Windows-FPVDemo
keywords: [Windows SDK, FPVDemo, basic tutorial]
---

*If you come across any mistakes or bugs in this tutorial, please let us know by sending emails to dev@dji.com. Please feel free to send us Github pull request and help us fix any issues.*

---

This tutorial is designed for you to gain a basic understanding of using DJI Windows SDK. It will implement the FPV view and basic control of DJI Phantom 4 Pro V2.0's camera.

You can download the tutorial's final sample project from this [Github Page](https://github.com/DJI-Windows-SDK-Tutorials/Windows-FPVDemo).
   
We use [DJI Phantom 4 Pro V2.0](https://www.dji.com/cn/phantom-4-pro-v2?site=brandsite&from=nav) as an example to make this demo.

## Importing DJIVideoParser Project

If you are not familiar with the process of integrating and activating DJI Windows SDK to your project, please check the Github source code and this tutorial: [Integrate WSDK into Application](https://developer.dji.com/windows-sdk/documentation/application-development-workflow/workflow-integrate.html) for details. Now we start to import the DJIVideoParser project.

* Firstly, copy DJIVideoParser project to the root folder of DJIWSDKDemo project. You can find DJIVideoParser project [here](https://github.com/dji-sdk/Windows-SDK/tree/master/Sample%20Code).
![VideoParserProject](../images/quick-start/WSDKCopyDJIVideoParser.png)
* Secondly, add DJIVideoParser project.
  * a. Open DJIWSDKDemo solution, right-click on the solution, select **Add->Existing Project**.
  * b. Locate to the folder where DJIVideoParser.vcxproj exists, select it and click **Open**.
* Thirdly, configure DJIVideoParser project.
  * a. Right-click on the DJIVideoParser project, select **Properties**.
  * b. Double check and follow the following settings: **Configuration, Platform, Target Platform Version** and **Output Directory**.
  ![VideoParserProject](../images/quick-start/WSDKDJIVideoParserConfig.png)
  * c. Right-click on the DJIVideoParser project, select "Build" to build the project so that you can use the API directly in the code after adding the related reference.
* Fourthly, add reference to DJIWSDKDemo.
  * a. Right-click on DJIWSDKDemo project, and select **Add Reference**, and then click **Projects**.
  * b. Click the checkbox of the DJIVideoParser project to add it.
  ![VideoParserProject](../images/quick-start/WSDKAddVideoParserReference.png)
  * Click **OK**.
* Finally, copy FFMpeg dlls to DJIWSDKDemo project.
  * a. Right-click on the DJIWSDKDemo project. Select **Add->Existing Item**.
  * b. Locate to the path of FFmpeg dll files(under the folder of /DJIVideoParser/ThirdParties/dlls).
  * c. Select all of them, and then click **Add**.
  ![VideoParserProject](../images/quick-start/WSDKAddFFMPEGDlls.png)

## Implementing the First Person View

  * **1**. Add FPV view. Double-click on the **MainPage.xaml** to open it. We can see there is a **Grid** declaration. Add an **SwapChainPanel** element named "swapChainPanel" inside it.

~~~csharp
    <Grid x:Name="MainGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SwapChainPanel x:Name="swapChainPanel"/>
    </Grid>
~~~

  * **2**. Add video data parser and container. Double-click on the **MainPage.xaml.cs** to open it. Add the following elements in the **MainPage** class file.

~~~csharp
//use videoParser to decode raw data.
private DJIVideoParser.Parser videoParser;
~~~

  * **3**. Add video raw data callback, decoded data callback and camera type callback. Add the following methods in the **MainPage** class file.

~~~csharp
//raw data
void OnVideoPush(VideoFeed sender, byte[] bytes)
{
    videoParser.PushVideoData(0, 0, bytes, bytes.Length);
}

//Decode data. Do nothing here. This function would return a bytes array with image data in RGBA format.
async void ReceiveDecodedData(byte[] data, int width, int height)
{
}

//We need to set the camera type of the aircraft to the DJIVideoParser. After setting camera type, DJIVideoParser would correct the distortion of the video automatically.
private void OnCameraTypeChanged(object sender, CameraTypeMsg? value)
{
    if(value != null)
    {
        switch (value.Value.value)
        {
            case CameraType.MAVIC_2_ZOOM:
                this.videoParser.SetCameraSensor(AircraftCameraType.Mavic2Zoom);
                break;
            case CameraType.MAVIC_2_PRO:
                this.videoParser.SetCameraSensor(AircraftCameraType.Mavic2Pro);
                break;
            default:
                this.videoParser.SetCameraSensor(AircraftCameraType.Others);
                break;
        }

    }
}
~~~

  * **4**. Add raw data and decoded data listener after the code of registering app successfully.

~~~csharp
//Callback of SDKRegistrationEvent
private async void Instance_SDKRegistrationEvent(SDKRegistrationState state, SDKError resultCode)
{
    if (resultCode == SDKError.NO_ERROR)
    {
        System.Diagnostics.Debug.WriteLine("Register app successfully.");

        //Must in UI Thread
        await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, async () =>
        {
            //Raw data and decoded data listener
            if (videoParser == null)
            {
                videoParser = new DJIVideoParser.Parser();
                videoParser.Initialize(delegate (byte[]data)
                {
                    //Note: This function must be called because we need DJI Windows SDK to help us to parse frame data.
                    return DJISDKManager.Instance.VideoFeeder.ParseAssitantDecodingInfo(0, data);
                });
                //Set the swapChainPanel to display and set the decoded data callback.
                videoParser.SetSurfaceAndVideoCallback(0, 0, swapChainPanel, ReceiveDecodedData);
                DJISDKManager.Instance.VideoFeeder.GetPrimaryVideoFeed(0).VideoDataUpdated += OnVideoPush;
            }
            //get the camera type and observe the CameraTypeChanged event.
            DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).CameraTypeChanged += OnCameraTypeChanged;
            var type = await DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).GetCameraTypeAsync();
            OnCameraTypeChanged(this, type.value);
        });
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("SDK register failed, the error is: ");
        System.Diagnostics.Debug.WriteLine(resultCode.ToString());
    }
}
~~~

  
## Enjoying the First Person View

Run the demo now and connect your PC to the Remote Controller of the aircraft. If everything goes well, you should be able to see the live video stream in the application, congratulations!

  ![fpv](../images/tutorials-and-samples/FPVDemo/WSDKFPVView.png)
  
## Shoot Photo And Record Video

### Camera Work Mode

Before we start to implement the features of shooting photo or recording video, we need to understand the work mode of DJI camera firstly. There are serval types of [camera work mode](https://developer.dji.com/api-reference/windows-api/Components/CameraHandler.html?search=camerawork&i=3&#value_cameraworkmode_inline). We can shoot photo and start recording video when the camera work modes are set to **SHOOT_PHOTO** and **RECORD_VIDEO**, respectively.

### Display and Switch Camera Mode

* Display Camera Mode

  When we set a work mode to camera, we can add camera work mode change event to constantly display the current camera mode. We can add a TextBlock named "ModeTB" to display the camera work mode.

~~~csharp
DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).CameraWorkModeChanged += async delegate (object sender, CameraWorkModeMsg? value)
{
    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, async () =>
    {
         if (value != null)
         {
             ModeTB.Text = value.Value.value.ToString();
         }
    });
};
~~~

* Switch camera mode

  Before shooting photo or recording video, we need to switch camera mode to a proper work mode. Add methods named `SetCameraWorkModeToShootPhoto_Click()`, `SetCameraModeToRecord_Click()` and `SetCameraWorkMode()` to implement camera mode settings and switching, the implementation of the methods are shown below:
  
~~~csharp

private async void SetCameraWorkModeToShootPhoto_Click(object sender, RoutedEventArgs e)
{
    SetCameraWorkMode(CameraWorkMode.SHOOT_PHOTO);
}

private void SetCameraModeToRecord_Click(object sender, RoutedEventArgs e)
{
    SetCameraWorkMode(CameraWorkMode.RECORD_VIDEO);
}

private async void SetCameraWorkMode(CameraWorkMode mode)
{
    if (DJISDKManager.Instance.ComponentManager != null)
    {
        CameraWorkModeMsg workMode = new CameraWorkModeMsg
        {
            value = mode,
        };
        var retCode = await DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).SetCameraWorkModeAsync(workMode);
        if (retCode != SDKError.NO_ERROR)
        {
            OutputTB.Text = "Set camera work mode to " + mode.ToString() + "failed, result code is " + retCode.ToString();
        }
    }
    else
    {
        OutputTB.Text = "The application hasn't been registered successfully yet.";
    }
}
~~~

### Shoot Photo and Record Video

Now we start to implement the features of shooting photo or recording video.

* Shoot Photo
  
  Add a button named "StartShootPhoto", and implement the `StartRecordVideo_Click()` method of it as shown below:
  
~~~csharp
private async void StartRecordVideo_Click(object sender, RoutedEventArgs e)
{
    if (DJISDKManager.Instance.ComponentManager != null)
    {
        var retCode = await DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).StartRecordAsync();
        if (retCode != SDKError.NO_ERROR)
        {
            OutputTB.Text = "Failed to record video, result code is " + retCode.ToString();
        }
        else
        {
            OutputTB.Text = "Record video successfully";
        }
    }
    else
    {
        OutputTB.Text = "The application hasn't been registered successfully yet.";
    }
}
~~~

* Get Video Record Time
	
	  When we record videos, we need to know the recording time of the video. The following code can help you get the record time constantly.
	   
~~~csharp
//Get updated video record time
DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).RecordingTimeChanged += async delegate (object sender, IntMsg? value)
{
    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, async () =>
                              {
                                  if (value != null)
                                  {
                                      RecordTimeTB.Text = value.Value.value.ToString();
                                  }
                              });
};
~~~

* Record Video
   	 
   	 Add a button named "StartRecordVideo" and "StopRecordVideo", and implement the `StartRecordVideo_Click()` and `StopRecordVideo_Click()` methods of it as shown below:
   	 
~~~csharp
private async void StartRecordVideo_Click(object sender, RoutedEventArgs e)
{
    if (DJISDKManager.Instance.ComponentManager != null)
    {
        var retCode = await DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).StartRecordAsync();
        if (retCode != SDKError.NO_ERROR)
        {
            OutputTB.Text = "Failed to record video, result code is " + retCode.ToString();
        }
        else
        {
            OutputTB.Text = "Start Recording video successfully";
        }
    }
    else
    {
        OutputTB.Text = "The application hasn't been registered successfully yet.";
    }
}

private async void StopRecordVideo_Click(object sender, RoutedEventArgs e)
{
    if (DJISDKManager.Instance.ComponentManager != null)
    {
        var retCode = await DJISDKManager.Instance.ComponentManager.GetCameraHandler(0, 0).StopRecordAsync();
        if (retCode != SDKError.NO_ERROR)
        {
            OutputTB.Text = "Failed to stop record video, result code is " + retCode.ToString();
        }
        else
        {
            OutputTB.Text = "Stop record video successfully";
        }
    }
    else
    {
        OutputTB.Text = "The application hasn't been registered successfully yet.";
    }
}
~~~   	 

* Display Result

![video](../images/quick-start/WSDFPVDemoRecord.png)

## Summary
   
   In this tutorial, you’ve learned how to use DJI Windows SDK to show the FPV View from the aircraft's camera and control the camera to shoot photo and record video. These are the most basic and common features in a typical drone app. However, if you want to create a drone app which is more fancy, you still have a long way to go. More advanced features should be implemented, including previewing the photo and video in the SD Card, showing the OSD data of the aircraft and so on. Hope you enjoy this tutorial, and stay tuned for our next one!
   
