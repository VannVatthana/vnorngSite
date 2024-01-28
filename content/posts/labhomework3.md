---
title : 'Lab Homework 3 : Unity Roll-A-Ball'
date : 2023-11-27
draft : false
---

In this lab, we are going to build my first Unity project **Roll-A-Ball**. The step-by-step tutorial of this project can be found at [Unity's official website](https://learn.unity.com/project/roll-a-ball?uv=2020.2), make sure to choose the right version according to the one that you installed. 

You can also find my project file from [https://github.com/VannVatthana/roll_a_ball_v2020](https://github.com/VannVatthana/roll_a_ball_v2020)

## Roll-A-Ball project
Assume that an empty 3D project is created (please look at [Lab Homework2](https://vannvatthana.github.io/vnorngSite/posts/labhomework2/)), 
![Empty](/images/labhomework2/emptyEnv.png)
There are 4 types of objects that are needed to be created for this project:
- A Ball that acts as a player that roll around and collect the pickups
- Some Pickups for the ball to collect to gain score
- Ground so that the ball does not fall because of gravity
- Walls as a boundary 

First and foremost, create a ground for the environment: **GameObject -> 3D Object -> Plane**, then, at the hierarchy window, rename the Plane object to **Ground** (You can also create a GameObject by pressing right click : Go to Hierarchy Window, **Right Click -> 3D Object -> Plane**).
![Ground](/images/labhomework3/create_plane.png) 
You have a plane that looks like this:
![planeWhite](/images/labhomework3/plane_white.png)
Create a material for the ground to change modify its color: Go to Project Tab, then **Right Click -> Create -> Material**
![CreateMat](/images/labhomework3/create_material.png)
Click on the material, then go to its inspector to and change the color according to your preference.
![ChangeCol](/images/labhomework3/change_color.png)
Then change drag the and drop the material on the ground. Now create the some materials for the colors of the ball, pickups and walls too.
![DragDrop](/images/labhomework3/drag_drop.png)

Create 4 walls, following the same step as the ground : **GameObject -> 3D Object -> Cube**, and resize the scale at the spector so that it fits with the ground. Then create an empty object, rename it to **Walls**,  then put the 4 wall as its children. Add a box collider to each wall (child, not the parent!)
![Walls](/images/labhomework3/walls.png) 

For the pickups, create one of them : **GameObject -> 3D Object -> Cube**, then drag to the Project Window to crate a Prefab. On the inspector, change the rotation and scale of the pickup. For its behavior, create a C# script called **Rotator.cs** (on Project Window, **Right Click -> Create -> C# Script**) and attach it to the prefab.
![Prefab](/images/labhomework3/prefab.png)

Here is the code of **Rotator.cs**. It only needs the behavior of spinning around itself.
![Rotator](/images/labhomework3/rotatorcs.png)
Do not forget to add a tag for pickups so that it can be picked by the ball. At the inspector, Go to **Tag -> Add Tag** to create a tag called **pickup**, and assign it to the pickups.
![Tag](/images/labhomework3/tag.png)

Finally, create a ball : **GameObject -> 3D Object -> Sphere**, then create a script called **PlayerController.cs**  and attach it to the ball. The script contrains the code for making the ball move using the four arrow keys.
![CodePlayer](/images/labhomework3/playercode.png)

### Test run
At this stage, the project is nearly completed. I need to test play the project to find out if there are any bugs that cause the game to not work properly. In my case, there were no bugs occured as I followed directly the tutorial, so the test run was successful. The result should be similar to this.

### Build
To build an apk for any platform, go to **File > Build Setting**, then choose **PC, Mac & Linux Standalone**. Keep the default settings of the build (**Target Platform** -> **Windows** and **Architecture** -> **x86.64**).
![Build](/images/labhomework3/buildplatform.png)
In case there are more than one scene in the project, make sure to check on the one to build, and uncheck the rest. In my case, I have only one scene, so there is nothing to worry about. Finally, click on **Build** or **Build and Run**.
![Scenechoice](/images/labhomework3/choosescene.png)