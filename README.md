---
description: BlendTriee.cs
---

# joystick + slider

**Before:** we recorded the animations, set as trigger and used 9 buttons to call each of them at one time

![](<.gitbook/assets/image (2).png>) ![](<.gitbook/assets/image (7).png>)

**Updated:** we use joy stick to control patient's eye movement and slider to control sitting position

![](<.gitbook/assets/image (1) (1).png>) ![](<.gitbook/assets/image (6).png>)

![](<.gitbook/assets/image (5) (1).png>) ![](<.gitbook/assets/image (9).png>)

**This is how to works:**

This script is attached to the character model. When the character model is instantiate in a new scene, it tries to find the joystick and slider in the scene and get the corresponding inputs to change the position of the eyes and eyelids as well as the character and the chair.

```csharp
 GameObject root = GameObject.Find("Canvas");
        foreach (Transform child in root.transform)
        {
            if(child.name == "MoveSlider")
            {
                PatientSlider = child.gameObject;
            }
            else if (child.name == "MoveJoystick")
            {
                joystick = child.gameObject;
                rotateJoy = joystick.GetComponent<EasyJoystick>();
            }

 c       }​
```

```csharp
 Vector3 input = rotateJoy.MoveInput();
        if (input.x < 0)
        {
            right = 0;
            left = -input.x * 100;
        }
        else if (input.x > 0)
        {
            left = 0;
            right = input.x * 100;
        }
        else
        {
            left = right = 0;
        }
        if (input.y < 0)
        {
            down = -input.y * 100;
            up = 0;
        }
        else if (input.y > 0)
        {
            down = 0;
            up = input.y * 100;
        }
        else
        {
            up = down = 0;
        }
```

For the eyelids, we directly use the values of the blendershapes that come with the model.

![](<.gitbook/assets/image (3).png>)

```
        skin.SetBlendShapeWeight(22, right);
        skin.SetBlendShapeWeight(23, right);
        skin.SetBlendShapeWeight(24, left);
        skin.SetBlendShapeWeight(25, left);
        skin.SetBlendShapeWeight(26, up);
        skin.SetBlendShapeWeight(27, up);
        skin.SetBlendShapeWeight(28, down);
        skin.SetBlendShapeWeight(29, down);
```

and for the eyes, we set their rotation because we made the changes ourselves previously.

```csharp
        EyeLtoR = (input.x + 1) / (1 + 1) * (25 + 15) - 15;
        EyeUptoDown = (input.y + 1) / (1 + 1) * (-170 + 196) - 196;
        eyeLeft.localEulerAngles = new Vector3(EyeLtoR, 0, EyeUptoDown);
        eyeRight.localEulerAngles = new Vector3(EyeLtoR, 0, EyeUptoDown);
```

The figure and the seat are done with the animation we recorded. The value that slider passes will affect the height of the seat.

![](<.gitbook/assets/image (4) (1).png>)

```csharp
 this.GetComponent<Animator>().SetFloat("motionTime", PatientSlider.GetComponent<Slider>().value);
```

For joystick I exported a package. By commenting out the following lines, the joystick can remain in any position instead of returning to the center point.

![EasyJoystick](<.gitbook/assets/image (2) (1).png>)

In addition, the MoveInput is only x and y in our case.

![EasyJoystick](<.gitbook/assets/image (8).png>)
