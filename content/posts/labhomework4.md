---
title : 'Lab Homework 4 : Roll-A-Ball in VR'
date : 2023-12-04
draft : false
---

In **Lab Homework 3**, I made my first project **Roll-A-Ball** in Unity. This time, I will convert that project into a version that we can play in VR headsets. 

You can find this project file on my Github [https://github.com/VannVatthana/roll_a_ball_vr](https://github.com/VannVatthana/roll_a_ball_vr).

## Roll-a-Ball VR
### Set up the project
As usual, start by create a new 3D project (have a look at [Lab Homework2](https://vannvatthana.github.io/vnorngSite/posts/labhomework2/) in case you forget). Since Oculus headset runs with Android, we need to build the project for Android device as well. Go to **File -> Build Settings -> Android -> Switch Platform**. Normally you should be able to keep the default setting of the build.

To get the access to the VR functionalities of Oculus in Unity, you can import the [Oculus Integration](https://assetstore.unity.com/packages/tools/integration/oculus-integration-deprecated-82022) to Unity. 

It might takes sometimes to import the assets. Once it finished, you will see the Oculus folder in your Assets folder. You also need to import the package called **Oculus XR Plugin**. Go to **Window -> Package Manager** and find this plugin in **Packages: Unity Registry**. In the **Project Setting -> XR Plug-In Management**, check the Oculus for both the Android and PC. 

Now the project is ready to build for Oculus Headset.

### Export assets from the previous project
The assets from the [previous lab](https://vannvatthana.github.io/vnorngSite/posts/labhomework3/) is needed to build the VR version. Open the Roll-a-Ball project, go to **Assets -> Export Package**, then click on **Export**. The extension of the package is *.unitypackage*. 

Now, we go back to the project Roll-a-Ball VR to continue the project.

### Create a new Scene
Create an Empty Object and call it Environment. Create a huge plane as the floor of the scene, and a cube as the table on the floor, then put both of them as children of Environment.

For this project, you do not need the Main Camera (remove it from the scene). Instead, use the OVRCameraRig from **Assets -> Oculus -> VR -> Prefabs**. If you want to save time from trying to find the location of each file, you can also type the name of that file on the Project Panel search bar. On the OVRManager script attached to it, set the Tracking to Floor Level and the Color Gamut to Quest.

After adding the OVRCameraRig to the scene, you need OVRControllerPrefab for both HandAnchors. For left controller, set the controller prefab as a child of LeftControllerAnchor, and then select L Touch on the script. Do the same thing for the right controller. At this point, you should be able to look around at the scene and see your controllers moved according to your hands.

###  Import Roll-a-ball assets to the VR project and add to the scene
In the VR project, go to **Assets -> Import Package -> Custom Package**, then find the *.unitypackage* file that you saved a moment ago, and click **Import**. 

To avoid any errors from the input system, comment/remove the code lines for the input system. In my case, when I imported the Oculus Integration assets, there are some files that I needed to remove so that the project could launch correctly. That might delete the **PlayerController.cs** from the Oculus folder, so I did not need to rename my roll-a-ball's **PlayerController**. 

Now add the **Player** (the ball), **Walls**, **PickUps** and **Ground**, then put them in an empty gameobject **roll-a-ball**. I also create two new **TextMeshPro** for counting score and show the win text and put them to world space. With everything added, the scene should look like this in **Scene** mode: 
![assemble](/images/labhomework4/scene_mode.png)
### Adding Colliders and Layers
Create two layers **selection** for roll-a-ball gameobject (the parent), and **roll-a-ball** for all the children(**Player**, **Walls**, **Ground** and **PickUps**). That can ensure that the colliders of the parent and the children do not affect each other. Add a **Box Collider** to **roll-a-game** board, and set it to fit the size of the **Ground**.

Set **Left/RightHandAnchor** to **selection** layer too. Feach anchor, add a **SphereCollider** and adjust the radius to the same size of the controller prefab. Now the setup of the scene is finished and it is time to write a script for grabbing the roll-a-ball gameboard.

### Script for grabbing roll-a-ball GameObject
We need a script for grabbing the roll-a-ball board. Create a C# script (I named it **MySelct.cs** because of a typo) and attach it to Left and RightHandAnchor. Here are the variables needed.
```csharp
public OVRInput.Controller controller;
private float triggerValue;
[SerializeField] private bool isInCollider;
[SerializeField] private bool isSelected;
private GameObject selectedObj;
```
This is the code for handling the condition of roll-a-ball gameobject. When the board is selected/grabbed, the board moves and rotates according to the movement of the controller. When the trigger is released, the game board is dropped back on the table. 

```csharp
triggerValue = OVRInput.Get(OVRInput.Axis1D.PrimaryIndexTrigger, controller);

if(isInCollider)
{
    // roll-a-ball not selected 
    if (!isSelected && triggerValue > 0.95f)
    {
        isSelected = true;
        selectedObj.transform.parent = this.transform;
        Rigidbody rb = selectedObj.GetComponent<Rigidbody>();
        rb.isKinematic = true;
        rb.useGravity = false;
        rb.velocity = Vector3.zero;
        rb.angularVelocity = Vector3.zero;
    }
    // roll-a-ball is selected
    else if (isSelected && triggerValue <0.95f)
    {
        isSelected = false;
        selectedObj.transform.parent = null;
        Rigidbody rb = selectedObj.GetComponent<Rigidbody>();
        rb.isKinematic = false;
        rb.useGravity = true;
        rb.velocity = OVRInput.GetLocalControllerVelocity(controller);
        rb.angularVelocity = OVRInput.GetLocalControllerAngularVelocity(controller);
    }
}
```
Here is the code for conrolling whether the controller collides with roll-a-ball object and if the trigger button is pressed or not.
```csharp
void OnTriggerEnter(Collider other)
{
    if(other.gameObject.name == "roll-a-ball")
    {
        isInCollider = true;
        selectedObj = other.gameObject;
    }
}

void OnTriggerExit(Collider other)
{
    if (other.gameObject.name == "roll-a-ball")
    {
        isInCollider = false;
        selectedObj = null;
    }
}
```
This is what the whole script looks like:
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MySelct : MonoBehaviour
{
    public OVRInput.Controller controller;
    private float triggerValue;
    [SerializeField] private bool isInCollider;
    [SerializeField] private bool isSelected;
    private GameObject selectedObj;

    // Update is called once per frame
    void Update()
    {
        // Here we called the IndexTrigger value from controller,
        // so the Primary will map to right hand when the inspectoris RTouch in Unity.
        triggerValue = OVRInput.Get(OVRInput.Axis1D.PrimaryIndexTrigger, controller);

        if(isInCollider)
        {
            // not selected and pull the trigger
            if (!isSelected && triggerValue > 0.95f)
            {
                isSelected = true;
                selectedObj.transform.parent = this.transform;
                Rigidbody rb = selectedObj.GetComponent<Rigidbody>();
                rb.isKinematic = true;
                rb.useGravity = false;
                rb.velocity = Vector3.zero;
                rb.angularVelocity = Vector3.zero;
            }
            else if (isSelected && triggerValue <0.95f)
            {
                isSelected = false;
                selectedObj.transform.parent = null;
                Rigidbody rb = selectedObj.GetComponent<Rigidbody>();
                rb.isKinematic = false;
                rb.useGravity = true;
                rb.velocity = OVRInput.GetLocalControllerVelocity(controller);
                rb.angularVelocity = OVRInput.GetLocalControllerAngularVelocity(controller);
            }
        }
    }

    void OnTriggerEnter(Collider other)
    {
        if(other.gameObject.name == "roll-a-ball")
        {
            isInCollider = true;
            selectedObj = other.gameObject;
        }
    }

    void OnTriggerExit(Collider other)
    {
        if (other.gameObject.name == "roll-a-ball")
        {
            isInCollider = false;
            selectedObj = null;
        }
    }
}
```
Now, you can grab and move roll-a-ball gameboard around. What is left to do is to build an Android apk and install it on the your Oculus Quest.

### Installation of APK using SideQuest
SideQuest is an application that allows you to install any third party apks to your VR headset. Make sure that you have download SideQuest and set up an account. For the Oculus Quest headset, do not forget to connect to Oculus app on your computer via the cable, and set the headset to developper mode. 

Finally, open SideQuest, click on the Install icon, and find the apk that you have built for roll-a-ball VR. If everything goes well, you it should be like in this video:
{{<youtube 23MJ3lOFERQ>}}

Well done, you have made the first VR project for Oculus Quest!