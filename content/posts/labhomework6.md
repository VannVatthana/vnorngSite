---
title : 'Lab Homework 6 : Final Project'
date : 2024-01-22
draft : false
---

This is the final project of the course IGD301. The scenario for this project is as follow: the user is in a supermarket scene where they have to select objects with a selection technique. Here, I am going to talk about the implementation of the selection technique that I call **Sloppy Robin**. So how did I proceeded with this technique?

### Interaction
The concept of **Sloppy Robin** is quite simple, there is one bow with the arrow attached to the string in the scene. The user pull the trigger of the left controller to grab the body of the bow and aim, and the right controller to pull the string. When the right controller's trigger is released, the arrow is shot. When it hit the target object, the object is selected, and the arrow comes back to its original position (attached to bow string). Since the arrow takes into account the physics components (Gravity and Kinematics), the user cannot aim directly at the wanted target, they might need to aim a little higher, especially when the target object is far. 

### Bow and Arrow assets
I need a bow and an arrow prefabs for this project. It is possible to use a long stick for the bow gameobject and another one for the arrow, but I think that the visual appeal might add to the experience of Sloppy Robin. Therefore I get found the assets from this [github repository](https://github.com/Fist-Full-of-Shrimp/Bow-and-Arrow-Assets), which is great since it already has the necessary components for scripting.

### Bow Grab
The given **Bow** prefab has 4 children: **PlankBow** (3D model of the prefab), **Start** (rest position of the string), **End** (fully pulled position of the string), **Notch** (where the arrow is put on). 
To be able to grab the bow, I need to put a collider on where the bow is needed to be grabbed. 
![Bow Collider](/images/labhomework6/bow_collider.png "Collider Box of the Bow object")
I created a script called **BowGrab.cs** for the left controller to grab and move around the bow. The code is very simple, it only needs to detect whether the controller collides with the bow, and pick it up when the trigger is pressed, and when trigger is released, the bow is floating in one place without moving.
```csharp
using UnityEngine;

public class BowGrab : MonoBehaviour
{
    public OVRInput.Controller controller;
    private float triggerValue;
    [SerializeField] private bool isInCollider;
    [SerializeField] private bool isSelected;

    private GameObject selectedObj;
    void FixedUpdate()
    {
        triggerValue = OVRInput.Get(OVRInput.Axis1D.PrimaryIndexTrigger, controller);
        
        if(isInCollider)
        { 
            if(!isSelected && triggerValue > 0.95f)
            { 
                isSelected = true;
                selectedObj.transform.parent = this.transform;
            }
            else if (isSelected && triggerValue < 0.95f)
            {
                isSelected = false;
                selectedObj.transform.parent = null; 
            }
        }
    }
    private void OnTriggerEnter(Collider other)
    { 
        
        if (other.gameObject.name == "Bow")
        { 
            isInCollider = true;
            selectedObj = other.gameObject;

        }
    }
    private void OnTriggerExit(Collider other)
    {
        if (other.gameObject.name == "Bow")
        {
            isInCollider = false;
            selectedObj = null;
        }
    }
}
```
### Bow string Animation
The bow prefab does not have a string, so what I do is create a LineRenderer and set it as a child of the **Bow**. I created a script called **PullString.cs**, and attached it to the **String** object. This script lets the right controller to move the notch and update the **String** renderer according to how much it is pulled. 
![String](/images/labhomework6/string_rest_pull.png "String in rest position (left) and when it is pulled (right)")
The maximum distance that the notch can be pulled is the distance from **Start** point to **End** point. When the right trigger is released, the notch moves back to its original position, and the pull amount is sent to **MyTechnique** so that it can calculate the force to send the arrow flying. Here is the code of **PullString**

``` csharp
using System;
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit;

public class PullString : XRBaseInteractable
{   
    public static event Action<float> PullActionReleased;
   
    public Transform start, end;
    public GameObject notch;
    public float pullAmount { get; private set; } = 0.0f;

    private LineRenderer _lineRenderer;
    private IXRSelectInteractor pullingInteractor = null;

    protected override void Awake()
    {
        base.Awake();
        _lineRenderer = GetComponent<LineRenderer>();
    }

    public void SetPullInteractor(SelectEnterEventArgs args)
    {
        pullingInteractor = args.interactorObject;
    }

    public void Release()
    {
        PullActionReleased?.Invoke(pullAmount);
        pullingInteractor = null;
        pullAmount = 0f;
        notch.transform.localPosition = new Vector3(notch.transform.localPosition.x, notch.transform.localPosition.y, 0f);
        UpdateString(); // Bow String, not char 
    }

    public override void ProcessInteractable(XRInteractionUpdateOrder.UpdatePhase updatePhase)
    {
        base.ProcessInteractable(updatePhase);
        if(updatePhase == XRInteractionUpdateOrder.UpdatePhase.Dynamic)
        {
            Vector3 pullPos = pullingInteractor.transform.position;
            pullAmount = CalculatePull(pullPos);
            UpdateString();
        }
    }
    float CalculatePull(Vector3 pullPosition)
    {
        Vector3 pullDirection = pullPosition - start.position;
        Vector3 targetDirection = end.position - start.position;
        float maxLength = targetDirection.magnitude;

        targetDirection.Normalize();
        float pullValue = Vector3.Dot(pullDirection, targetDirection) / maxLength;
        return Mathf.Clamp(pullValue, 0, 1);
    }
    void UpdateString()
    {
        Vector3 linePosition = Vector3.forward * Mathf.Lerp(start.transform.localPosition.z, end.transform.localPosition.z, pullAmount);
        notch.transform.localPosition = new Vector3(notch.transform.localPosition.x, notch.transform.localPosition.y, linePosition.z + .0f);
        _lineRenderer.SetPosition(1, linePosition);
        _lineRenderer.SetPosition(1, linePosition);
    }
}
```
### Arrow Behavior
The most important part of the implementation is the arrow because it is the object that is used as the selector of supermarket items. All the physics components of the **Arrow** object are scripted in **MyTechnique**. 

At the start of the game, **Arrow** is the child of **Notch**, and the physics components of the rigidbody (gravity and kinematics) are not activated yet. So, it moves according to the **Bow**.  When the **String** is released, the **Arrow** detaches from the **Notch**, the gravity and kinematics are activated, and the force to send the arrow flying are calculated. 
```csharp
private void Release(float value)
{
    // Detatch from arrow
    if(arrow.transform.parent == notch.transform)
        arrow.transform.parent = null;
    _inAir = true;
    SetPhysics(true);
    GameObject arrow1 = arrow;
    Vector3 force = arrow1.transform.forward * value * speed;
    _rigidbody.AddForce(force, ForceMode.Impulse);

    StartCoroutine(RotateWithVelocity());

    _lastPosition = arrow.transform.position;
}
```
When the arrow hit an object, it sets 
`currentSelectedObject = hitInfo.collider.gameobject`, then the gravity and kinematics of the **Arrow** are set to false. In case that the arrow does not hit anything for 5s, both rigidbody components are also set to false. At that moment, the **Arrow** stops moving.
```csharp
private void CheckCollision()
{
    if (Physics.Linecast(_lastPosition,arrow.transform.position,out RaycastHit hitInfo))
    {
        if (hitInfo.transform.gameObject.layer != 6)
        {
            _rigidbody.interpolation = RigidbodyInterpolation.None;
            currentSelectedObject = hitInfo.collider.gameObject;
            Stop();
        }
    }
    else
    {
        StartCoroutine(DeactivatePhysics(5f));
    }
}
```
In the FixedUpdate(), when the **Arrow** stops moving, it is set back as a child of **Notch**, and it moves back to its original position on the notch.
```csharp
if (_inAir)
{
    CheckCollision();
    _lastPosition = arrow.transform.position;
}
else
{
    arrow.transform.SetParent(notch.transform);
    arrow.transform.SetLocalPositionAndRotation(Vector3.zero, Quaternion.Euler(Vector3.zero)) ;
}
```
### Locomotion
Since I did not have enough time to implement the selection of a group of objects before selecting the target one, the selection with arrow is quite difficult when it is far. So I wrote a function so that the user can move along the aisle of the supermarket using *A* and *B* button of the controller.
```csharp
void MoveAround()  // Move around 
{
    if (OVRInput.Get(OVRInput.Button.Two))  // Press B to move forward
        cameraRig.position = cameraRig.position + new Vector3(-3f,0,0);
    if (OVRInput.Get(OVRInput.Button.One))  // Press A to move backward
        cameraRig.position = cameraRig.position + new Vector3(3f, 0, 0);

}

```

You can find my implementation here : [https://github.com/VannVatthana/SloppyRobinVR](https://github.com/VannVatthana/SloppyRobinVR).

Here is what the final result looks like:
<!--show vdo-->
{{<youtube x1sgBtCkWCY>}}
As expected, the user needs some time to be familiar with how the arrow travels. However, it is a fun way to select objects.

