---
title: Controller Input with Unity
permalink: controller-input-with-unity.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

You can use the [Unity development tools](http://unity3d.com/unity) to create apps and games for Amazon Fire TV devices as you would any Android device.

Although we do not provide a Unity plugin for Fire TV development, there are packages in the Unity Asset Store to enable game controller support. In particular, [InControl by Gallant Games](http://www.gallantgames.com/incontrol) is one with which our developers have had great success. InControl is a cross-platform input manager for Unity3D that standardizes control mappings for a variety of common controllers.

You can also use the Unity input manager to configure controller input for your game. Use the tables below to map the buttons on the Amazon Fire TV remotes and game controllers with the Unity input manager buttons and axes.

{% include note.html content="The input references in this document apply to Unity 4.3.x and higher, but are subject to change with future Unity releases." %}

* TOC
{:toc}

## Remote Input

Use these values in Unity to map the buttons on both the Amazon Fire TV Remote and Voice Remote. See <a href="http://docs.unity3d.com/ScriptReference/KeyCode.html">KeyCode</a> for more details about Unity KeyCode values.

{% include image.html title="Unite remote" file="firetv/getting_started/images/remote-unity" type="png" %}

<table class="grid">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
  <thead>
    <tr>
      <th>Button</th>
      <th>Unity Input Manager Value</th>
      <th>Unity KeyCode Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>one (system event)</td>
      <td>none (system event)</td>
    </tr>
    <tr>
      <td>Back</td>
      <td>none (not supported)</td>
      <td><code>KeyCode.Escape</code></td>
    </tr>
    <tr>
      <td>Menu</td>
      <td>none (not supported)</td>
      <td><code>KeyCode.Menu</code></td>
    </tr>
    <tr>
      <td>Microphone (Search)</td>
      <td>none (system event)</td>
      <td>none (system event)</td>
    </tr>
    <tr>
      <td>Select (D-Pad Center)</td>
      <td>joystick button 0</td>
      <td><code>KeyCode.JoystickButton0</code></td>
    </tr>
    <tr>
      <td>Left (D-Pad)</td>
      <td>5th Axis</td>
      <td><code>KeyCode.LeftArrow</code></td>
    </tr>
    <tr>
      <td>Right (D-Pad)</td>
      <td>5th Axis</td>
      <td><code>KeyCode.RightArrow</code></td>
    </tr>
    <tr>
      <td>Up (D-Pad)</td>
      <td>6th Axis</td>
      <td><code>KeyCode.UpArrow</code></td>
    </tr>
    <tr>
      <td>Down (D-Pad)</td>
      <td>6th Axis</td>
      <td><code>KeyCode.DownArrow</code></td>
    </tr>
    <tr>
      <td>Play/Pause</td>
      <td>none (not supported)</td>
      <td>none (not supported)</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td>none (not supported)</td>
      <td>none (not supported)</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td>none (not supported)</td>
      <td>none (not supported)</td>
    </tr>
  </tbody>
</table>


## Game Controller Input

Use these values in Unity to map the buttons on the Amazon Fire Game Controller. See <a href="http://docs.unity3d.com/ScriptReference/KeyCode.html">KeyCode</a> for more details about Unity KeyCode values.

{% include image.html title="Game Controller Unity" file="firetv/getting_started/images/gamecontrollr-unity" type="png" %}

{% include image.html title="Game Controller Unity" file="firetv/getting_started/images/gamecontrollr-unity-second-view" type="png" %}

<table class="grid">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
  <thead>
    <tr>
      <th>Game Controller Button</th>
      <th>Unity Input Manager Value</th>
      <th>Unity KeyCode Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>none (system event)</td>
      <td>none (system event)</td>
    </tr>
    <tr>
      <td>Back</td>
      <td>none (system event)</td>
      <td><code>KeyCode.Escape</code></td>
    </tr>
    <tr>
      <td>Menu</td>
      <td>none (system event)</td>
      <td><code>KeyCode.Menu</code></td>
    </tr>
    <tr>
      <td>GameCircle</td>
      <td>none (system event)</td>
      <td>none (system event)</td>
    </tr>
    <tr>
      <td>A</td>
      <td>joystick button 0</td>
      <td><code>KeyCode.JoystickButton0</code></td>
    </tr>
    <tr>
      <td>B</td>
      <td>joystick button 1</td>
      <td><code>KeyCode.JoystickButton1</code></td>
    </tr>
    <tr>
      <td>X</td>
      <td>joystick button 2</td>
      <td><code>KeyCode.JoystickButton2</code></td>
    </tr>
    <tr>
      <td>Y</td>
      <td>joystick button 3</td>
      <td><code>KeyCode.JoystickButton3</code></td>
    </tr>
    <tr>
      <td>Left (D-Pad)</td>
      <td>5th Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Right (D-Pad)</td>
      <td>5th Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Up (D-Pad)</td>
      <td>6th Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Down (D-Pad)</td>
      <td>6th Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Left Stick (Left/Right)</td>
      <td>X Axis 1st Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Left Stick (Up/Down)</td>
      <td>Y Axis 2nd Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Left Stick Press</td>
      <td>joystick button 8</td>
      <td><code>KeyCode.JoystickButton8</code></td>
    </tr>
    <tr>
      <td>Right Stick (Left/Right)</td>
      <td>3rd Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Right Stick (Up/Down)</td>
      <td>4th Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Right Stick Press</td>
      <td>joystick button 9</td>
      <td><code>KeyCode.JoystickButton9</code></td>
    </tr>
    <tr>
      <td>Play/Pause</td>
      <td>none (not supported)</td>
      <td>none (not supported)</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td>none (not supported)</td>
      <td>none (not supported)</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td>none (not supported)</td>
      <td>none (not supported)</td>
    </tr>
    <tr>
      <td>Left Trigger (L2)</td>
      <td>13th Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Left Shoulder (L1)</td>
      <td>joystick button 4</td>
      <td><code>KeyCode.LeftShift KeyCode.JoystickButton4</code></td>
    </tr>
    <tr>
      <td>Right Trigger (R2)</td>
      <td>12th Axis</td>
      <td>none</td>
    </tr>
    <tr>
      <td>Right Shoulder (R1)</td>
      <td>joystick button 5</td>
      <td><code>KeyCode.RightShift KeyCode.JoystickButton5</code></td>
    </tr>
  </tbody>
</table>


## Controller Names

Controller names are available in Unity with the [`Input.GetJoystickNames()`](http://docs.unity3d.com/ScriptReference/Input.GetJoystickNames.html) method. Use these values for each controller:

*   Remote: `"Amazon Fire TV Remote"`
*   Voice Remote: `"Amazon Fire TV Remote"`
*   Game controller: `"Amazon Fire Game Controller"`

{% include links.html %}
