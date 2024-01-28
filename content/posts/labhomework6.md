---
title : 'Lab Homework 6 : Final Project'
date : 2024-01-22
draft : true
---

This is the final project of the course IGD301. The scenario for this project is as follow: the user is in a supermarket scene where they have to select objects with a selection technique. Here, I am going to talk about the implementation of the selection technique that I call **Sloppy Robin**. The idea is simple: I have a bow that creates an arrow at a time, and when I shot the arrow and it hit an object, that object is selected. So how did I proceeded with this technique?

### Bow and Arrow Prefab
Since I am not good at creating 3D models, I need to find the already made assets online to save time. The assets that I get is from [Website](), which is great since it already has the necessary components for scripting.
### Bow and String Animation
The bow prefab does not have a string, so what I do is create a LineRenderer and set it as a child of the bow. 
### Arrow Behavior
The most important part of the implementation is the arrow because the movement has to take into account the physics components (gravity, kinematic), and it has to send the information of the object that it hit. 
![arrowBehavior]()


