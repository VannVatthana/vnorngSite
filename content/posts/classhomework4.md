---
title: 'Class Homework 4: Selecting Objects in a Supermarket'
date: 2023-12-17T16:14:46+01:00
draft: false
---
## Grabbing and Teleportation
**Goal:** To make the shopping experience in visual supermarket to be intuitive and accessible. 

**How it works:** Users use the teleportation technique to move to differents locations of the supermarket. Once arriving in the proximity to wanted items, they can physically reach out and grab those items using VR controllers.

![VR Teleportation](/images/classhomework4/teleport_vr.webp "VR Teleportation")

![VR Grabbing](/images/classhomework4/vrGrab.png "Grabbing Object (arm-reach)")

**Evaluation:** 
- The success of this technique is evaluated based on speed and easiness of the navigation by teleportation, and the precision of the grabbing interaction with objects. 
- Metrics will include teleportation time, successful grabbing accuracy, and user feedback on the overall fluidity and naturalness of the selection process.

For the occluded object, it can be improved by putting both hands like the photo below, then all the object between the hands are selected. User can grab all of them at once.
![Grabbunch1](/images/classhomework4/grabbunch1.jpg)
![Grabbunch2](/images/classhomework4/grabBunch.PNG)
## Gaze-based selection 
**Goal:** hands-free method of selecting items in the virtual supermarket by using gaze-based interactions. The goal is to enhance user convenience and provide a novel and engaging way to navigate and choose items.

**How it works:** Users navigate through the virtual supermarket by looking at items. Gaze-based selection is implemented using eye-tracking technology. Users focus their gaze on an item for a designated duration to trigger its selection. Additional information about the item is revealed as users maintain their gaze.
To select the object, user needs to slightly raise the right eyebrow, then the eyetracker will register the movement of the eyebrow and proceed to select that object.

![Gaze-based](/images/classhomework4/raycast_gaze.png "Gaze-based interaction")

**Evaluation:** 
- Success will be evaluated based on the ease and intuitiveness of using gaze-based interactions for item selection. 
- Metrics will include the time taken to select items, the accuracy of gaze-based selections, and user feedback on the comfort and enjoyment of the gaze-based interaction.

## Voice control

**Goal:** Interact with the virtual supermarket using natural language voice commands, providing a hands-free and conversational shopping experience. The goal is to make shopping in VR efficient, accessible, and enjoyable.


**How it works:** Users navigate the virtual supermarket by verbally instructing a virtual shopping assistant. They can say commands like "Find Milk," and the assistant responds by highlighting and providing information about the relevant items with the quantity. Users confirm their selections vocally.

![Voice](/images/classhomework4/voicectrl.jpg "User says **Find Milk** (Left), then the whole list of available Milk with quantities appears (Middle), User says **Choose Milk1** to take one of the first milk from inventory (Right)")

**Evaluation:** 
- Success will be measured by the accuracy of voice recognition, the efficiency of navigating the virtual supermarket using voice commands, and user satisfaction with the natural language interaction.
- Metrics include the time taken to complete the shopping task and user feedback on the convenience and enjoyment of the voice-controlled experience.


