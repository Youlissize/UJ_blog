---
title: Gliding Bird Locomotion Technique
description: My implementation to fly like a bird in VR
date : 0018-01-01

draft: false
tags: [homework] [labs]
---

---
#### In Theory :

The main goal was to feel like this :

![Eagle](../static/eagleBis.jpg)


# Interactions

These are the 4 main interaction you can have :
**-** Going Up/Down : Point Up/Down controllers 
**-** Roll : Just move your arms
**-** Dive : Fold your arms
**-** Slown down when messing up : When your wings are not aligned with your velocity
 
![Interactions](../static/interactions.png)


#### In Practice :

# Controls

First of all we need a way to control our bird. The natural way is of course to control the wings with our controllers.
You can control individually wings orientation and length using your controller. 

![Wing](../static/wingbis.jpg)

For the body, it's fixed under the position of your headset, and it's orientation is automatically computed so that it is always aligned with your velocity.

The orientation of the camera is totally free.


# Physics

In order to implement such a motion technique, I used a physics-based system.
So I started to add a rigidBody element to my main actor that would be sensitive to gravity.

I First try to try to make a lift force that depend on your velocity and the orientation and length of your wings, 
but I quickly understand that a realistic simulation of wings will be hard to implement. 
So then I removed the gravity so by default you won't fall. I will add gravity myself later.

Then I try to find to right axis I needed to put in the 'AddForce' function of Unity to have the result I wanted.

Here are the axis I ended with:

![Axes](../static/axes.jpg)

The 'wings up' and 'wings down' axis are the most fondamental. That the direction of the force we need to add to go up or down. 
They are relative to your arms location, so when your arms are inclined, that allow you to turn. 
The fact that they're orthogonal to the velocity make this interaction independant of your speed.
So only with this, you were already free to go everywhere in the map, but always with a constant speed, and without any gravity.

So the next step was to add this gravity I removed earlier. To do this I add and addaptative force of the 'Down' axis.
The intensity of this force depend only on your wings orientation. So when wings are perfectly horizontal there's no gravity at all,
and gravity is maximum with wings orthogonal to the ground. 
This method allow you to fly at any speed without falling, which seems to me a good way to let inexperimented players have some fun without falling down everytime.

Then I needed a way to force player to have smooth movements in the air, and not making some brutal turn.
Si I added the constraint to have always you're wings orientation matching your velocity (within a certain threshold).
So when the scalar product of your wing orientation and your velocity is fewer than 0.9, I add a force of the 'backward' axis.
This is done independantly with the 2 wings.
Since your velocity is displayed using the bird body orientation, it's possible to align your controllers in this direction. 
But after some tests I figured that it's not that obvious, so I decided to add a visual warning to help player to keep the right orientation.

![Warning](../static/redscreen.png)

At this point the locomotion technique was fun, but the problem was that you never can gain some speed, only slow down...
To give player a way to accelerate I added the possibility to dive. Physically the dive add force on the 'forward' axis,
 and remove the lift, so gravity is fully applied on the rigidBody. 
To dive, you need to fold your arms, so I just trigger it when your controllers are close enough (less than 1 meter).
As for the velocity matching constraint, it wasn't obvious to see it.
So I added some speed lines, as in mangas, to clearly show that you are gaining some speed.

![SpeedLines](../static/speed.png)

Another small rectification to make the locomotion less punitive was to add a constant forward acceleration, 
so that when you are not making any mistake you will naturally gain some speed.



---

Here is a demo of the final version of this locomotion technique

{{< youtube PXFEgOI2-D0 >}}


And the link to the APK : https://www.mediafire.com/file/ny0b2ey14ic7xxp/BirdParkour.apk/file
















