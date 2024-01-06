---
title: 'Class homework 3: Selection techniques'
date: 2023-12-11
draft: false
---

## The Flexible Pointer
The Flexible Pointer overcomes the limitations of conventional pointing methods by adopting a virtual flexible pointer that can bend and curve around obstacles. Therefore, it allows users to point objects that are partially or fully occluded by other objects.

![Flexible Ptr](/images/classhomework3/flex_ptr.png "Flexible Pointer bended around an object")

- **Reach:** Infinite \\
 Users can point at objects anywhere in the virtual environment using Flexible Pointer. This is because the virtual pointer can be bent or flexed around objects, so there is no limit to how far it can reach.
- **Cardinality:** Single \\
The Flexible Pointer can only be used to select one object at a time as the user needs to focus on a single object in order to use the technique effectively.

- **Progressive Refinement::** Continuous \\
The user can continuously refine their selection by moving the virtual pointer closer to the object they want to select. This makes it easy for users to select objects with precision.

![Flexible Params](/images/classhomework3/parameters_flexible_pointer.png "Control parameters for the Flexible Pointer")

[comment]: <link: https://uist.hosting.acm.org/uist2005/images/poster_examples/pointer.pdf>

**Reference:** [A. Olwal, S. Feiner (1998) - The Flexible Pointer: An Interaction Technique for  Selection in Augmented and Virtual Reality](https://uist.hosting.acm.org/uist2005/images/poster_examples/pointer.pdf)

## LPTPT : Locked Dwell Time-based Point and Tap Gesture
LPTPT is a natural selection technique that combines pointing and tapping gestures using a locked dwell time. It is a versatile technique that can be used to select objects, navigate through menus, and interact with virtual environments.

The LPTPT technique typically involves the following steps:
1. The user points at an object in the virtual environment. 
2. The user holds their finger or hand on the object for a specified dwell time.
3. If the dwell time is met, a tap gesture is registered and the object is selected.

- **Reach:** Short \\
The user must be relatively close to the object they want to select. This is because the user needs to hold their finger or hand on the object for a specified dwell time.

- **Cardinality:** Single \\
LPTPT can only be used to select one object at a time. This is because the user needs to focus on a single object in order to use the technique effectively.

- **Progressive refinement:** Discrete \\
The user must complete the entire selection gesture before the object is selected. This can be a disadvantage for tasks that require fine-grained control over the selection process.
[comment]: <link: https://dl.acm.org/doi/abs/10.1145/3385959.3422701?casa_token=ZVQD7PHzqwwAAAAA:GtfjYGnps3H5KGDXGDAKMalBiU2d_ReGdT3N_YCtWiGPXChnzkcGEFYI7M4_GhJwAcpYdSAdMwr4>

**Reference:** [S.Bhowmick, A.Panigrahi, P.Borah - Investigating the Effectiveness of Locked Dwell Time-based Point and Tap Gesture for Selection of Nail-sized Object in Dense Virtual Environment](https://dl.acm.org/doi/pdf/10.1145/3385959.3422701)

## AMAZE : A Multi-finger Approach to Zoom in Dense Environments
AMAZE is a multi-finger continuous zoom-based technique for selecting small and distant objects in dense virtual environments. It combines a pinch gesture with pointing to provide accurate and efficient object selection.

The AMAZE technique typically involves the following steps:
1. The user performs a pinch gesture with their fingers to zoom in on the  virtual environment. 
2. As the user zooms in, the hit area of the pointer increases, allowing the user to select distant and small objects more easily.<br>
3. Once the user has zoomed in enough to bring the target object within their reach, they can use a pointing gesture to select the object.

- **Reach:** Short-medium \\
AMAZE user needs to be relatively close to the object they want to select, but not as close as with a purely pointing-based technique. This is because the pinch gesture allows the user to bring the object closer without having to physically move their hand.

- **Cardinality:** Single \\
AMAZE can only be used to select one object at a time.

- **Progressive refinement:** Continuous
AMAZE is a continuous selection technique, meaning that the user can continuously refine their selection by zooming in and out until they are satisfied with the target object. This makes it easy to select objects with precision, even in dense environments.

**Reference:** [Design and evaluation of AMAZE: A multi-finger approach to select small and distant objects in dense virtual environments](https://www.sciencedirect.com/science/article/pii/S0141938223001725?ref=pdf_download&fr=RR-2&rr=836ffe752b4279c1)
