---
title : 'Lab Homework 5 : Selection Pitch'
date : 2024-01-08
draft : false
---

In [Class Homework 4](https://vannvatthana.github.io/vnorngSite/posts/classhomework4/), I proposed three selection technique ideas what might be really fun to implement, but they are either too simple for the scope of the course or difficult and not suitable to implement for Oculus
headset. So, I would like to propose a new selection technique, which I would call it **Sloppy Robin**.

![Market](/images/labhomework5/super_market.PNG)

## Sloppy Robin
**Sloppy Robin** is a technique that makes use the scenario of shooting an arrows in real life. Unlike the raycasting, the projectile path of an arrow is a curve line, so the player has to aim a little or far above the target object that they want to select. 

![Drawing](/images/labhomework5/sloppy_robin.PNG)

When the arrow hit the target object, the player has two options: they can either bring the object back to hand (Option 1 from the illustration) or teleport to the position of the their arrow (Option 2). In case that the arrow does not fall on any object, the user can still use the teleport option to go wherever the arrow is. Depending on how good the user is at shooting, they can select even the occluded objects that are hiding from their view. Hence, it can solve the occlusion problem at some level.

- **Reach : Semi-infinite.** Users can select an object only as far as how far they can shoot an arrow 

- **Cardinality : Single.** Each arrow shot can select one object at a time.
- **Progressive Refinement : None**

