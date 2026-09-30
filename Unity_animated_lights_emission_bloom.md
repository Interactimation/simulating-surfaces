# Animating Unity Lights, Emissive Materials, and Bloom

## What this guide covers

Use Unity **Animation Clips** to create flickering or pulsing light:

- a Unity Light's **intensity**
- a Unity Light's **color**
- an emissive material's visible brightness and color
- Bloom, using a clip to control a Global Volume's **Weight**

This guide assumes a standard **Universal Render Pipeline (URP)** project.

## Is this viable with Animation Clips?

Yes, for a small number of lamps, screens, neon signs, torches, or magical objects.

- A Unity **Light** component works very well with Animation Clips.
- A **URP/Lit** material's emission color can also be keyed through an Animation Clip.
- For a flickering object, give it its **own material asset**. Do not expect one shared material to behave independently on several different objects.
- Use real-time Unity lights for lighting that must change while the project runs. A baked light does not provide changing run-time illumination.
- An emissive material makes its own surface look bright; it does **not** reliably light nearby objects by itself. Pair it with a Unity Point Light or Spot Light when the glow should illuminate the scene.

For a large number of independently flickering lights, a small script is usually more efficient than a separate clip and material for every light. For ordinary classroom scenes, clips are a clear and appropriate method.

## 1. Build one lamp or glowing-object hierarchy

For an object that should visibly glow and illuminate nearby objects, organize it like this:

```text
Lamp_01
  Bulb_Mesh
  Light_Emitter
```

1. Create an empty GameObject named `Lamp_01`.
2. Place the visible glowing mesh under it and name it `Bulb_Mesh`.
3. Add a Point Light or Spot Light as another child and name it `Light_Emitter`.
4. Position the Light near the visible bulb, tube, torch flame, or screen.
5. Select `Light_Emitter`.
6. In the Inspector, set its **Mode** to **Realtime**.
7. Give the light a starting **Intensity**, **Color**, and **Range**.
8. If you do not need moving shadows, turn shadows off for this light. Flickering real-time shadow lights can be expensive.

Using one root object lets a single Animation Clip control both the actual light and the glowing surface.

## 2. Prepare a unique emissive material

1. In `Assets/Art/Materials`, create a material or duplicate the material already used by the glowing mesh.
2. Give it a clear name, such as `M_LampBulb_01_Emission`.
3. Select the material and set **Shader** to **Universal Render Pipeline > Lit**.
4. Expand **Emission**.
5. Enable Emission.
6. Set a starting emission color. For a lamp that starts off, use black.
7. Assign this unique material to `Bulb_Mesh` in its **Mesh Renderer > Materials** list.

> Leave Emission enabled, even if the starting emission color is black. If emission is disabled entirely, a clip may change the color value without making the shader render emission.

## 3. Create a light-flicker Animation Clip

1. Select the root object, `Lamp_01`, in the **Hierarchy**.
2. Open **Window > Animation > Animation**.
3. Click **Create**.
4. Save the new clip in a project folder, for example:

   ```text
   Assets/Art/Animations/Lamp_01_Flicker.anim
   ```

5. Unity adds an **Animator** component and Animator Controller to `Lamp_01`. This is normal.
6. In the Animation window, click **Add Property**.
7. Expand `Light_Emitter`, then expand **Light**.
8. Add **Intensity**.
9. Click **Add Property** again.
10. Expand `Light_Emitter`, then expand **Light**.
11. Add **Color**.

The clip can now control the light's brightness and hue.

## 4. Keyframe light intensity and color

1. Turn on the red **Record** button in the Animation window.
2. Move the timeline marker to `0:00`.
3. Set the Light's Intensity and Color in the Inspector. Unity creates the first keyframes.
4. Move the marker later in the timeline.
5. Change Intensity and/or Color in the Inspector. Unity creates new keyframes.
6. Repeat to make a flicker, pulse, fade, or color change.
7. Turn Record off when finished.

Here is a simple one-second irregular flicker pattern. Use it as a starting point, not a required recipe.

| Time | Light intensity | Color idea |
|---|---:|---|
| 0:00 | 0.0 | black or dark warm orange |
| 0:04 | 2.0 | warm yellow-orange |
| 0:08 | 0.4 | dim orange |
| 0:12 | 2.5 | warm yellow-orange |
| 0:18 | 1.3 | warm orange |
| 0:25 | 2.2 | warm yellow-orange |
| 0:32 | 0.0 | black or dark warm orange |
| 0:40 | 1.8 | warm yellow-orange |
| 1:00 | 0.0 | black or dark warm orange |

For a convincing flicker, avoid perfectly even timing and perfectly repeating values.

### Make the changes abrupt or smooth

1. In the Animation window, switch to the **Curves** view.
2. Select the intensity curve or one of the color curves.
3. Right-click a keyframe.
4. For sharp, electronic, or broken-light flicker, choose **Both Tangents > Constant**.
5. For a slow pulse or breathing light, use smooth curves instead.

## 5. Keyframe the emissive material

1. Keep `Lamp_01` selected.
2. In the Animation window, click **Add Property**.
3. Expand `Bulb_Mesh`.
4. Expand **Mesh Renderer > Materials > Element 0**.
5. Add the emission property. Depending on the Unity version and shader, it may appear as one of these names:

   ```text
   Emission Color
   _EmissionColor
   Material._EmissionColor
   ```

6. Turn on the red **Record** button.
7. At the first keyframe, select the emissive material and set its emission color to black.
8. Move later in the clip.
9. Set the emission color to a bright HDR color that matches the Light's color.
10. Add dim and bright values at the same moments as the Light-intensity keys.
11. Turn Record off when finished.

In a URP/Lit material, emission brightness and emission color are represented together by the **Emission Color** value. Choose the hue you want, then raise its HDR brightness/intensity in the color picker for a strong visible glow.

### Keep the visible bulb and actual light in agreement

At the bright moments in the clip:

- use a bright emission color on `Bulb_Mesh`; and
- use a matching colored Light with higher intensity.

At the dim or off moments:

- set emission to black or nearly black; and
- set the Light intensity to `0` or a low value.

The visible bulb and its illumination should appear to come from the same source.

## 6. Make the clip repeat

1. In the **Project** window, select the `.anim` clip.
2. In the Inspector, enable **Loop Time**.
3. Click **Apply** if Unity shows an Apply button.
4. Press Play and view the effect through the **Game** camera.

If the first and final frames are very different, the loop may visibly jump. For an invisible loop, make the last value match or approach the first.

## 7. Add Bloom to the scene

Bloom is a post-processing effect that creates a soft halo around very bright parts of the image. It is not a light source; it does not illuminate nearby geometry.

### Create the base Bloom setup

1. Select the active `Main Camera`.
2. In its Inspector, enable **Post Processing**.
3. Create a Global Volume: **GameObject > Volume > Global Volume**.
4. Name it `V_BloomBase`.
5. In its **Volume** component, click **New** to create a Volume Profile.
6. Click **Add Override > Post-processing > Bloom**.
7. Enable the Bloom settings you want to change.
8. Set a modest starting **Intensity** and an appropriate **Threshold**.
9. Check the effect in the **Game** view.

If Bloom does not appear, confirm that the camera has Post Processing enabled and that the Global Volume and camera can affect one another on the same layer.

## 8. Animate Bloom with an Animation Clip

Do not try to key the Bloom override's internal settings directly in the Animation window. A cleaner clip-based method is to animate the **Weight** of a second, stronger Global Volume.

### Create the pulse Volume

1. Create a second Global Volume: **GameObject > Volume > Global Volume**.
2. Name it `V_BloomPulse`.
3. Give it a new Volume Profile.
4. Set its **Priority** higher than `V_BloomBase`.
5. Add a **Bloom** override.
6. Set this Bloom to the stronger effect you want at the peak of the pulse.
7. Set the `V_BloomPulse` Volume component's **Weight** to `0`.

### Create its clip

1. Select `V_BloomPulse` in the Hierarchy.
2. Open **Window > Animation > Animation**.
3. Click **Create** and save a clip such as `BloomPulse.anim`.
4. Click **Add Property**.
5. Expand **Volume** and add **Weight**.
6. Turn on the red Record button.
7. At the first keyframe, set Weight to `0`.
8. At the moment of the brightest flash, set Weight to `1`.
9. Set later keyframes back toward `0`.
10. Turn Record off.
11. Enable **Loop Time** on the clip if it should repeat.

When the animated Weight rises toward `1`, Unity blends in the stronger Bloom profile. This affects the whole camera image, so use it for a scene-wide flare or pulse, not for the glow of one object alone.

## 9. Test and troubleshoot

### The Light changes in the Animation window but not in Play mode

- Confirm that the root object has an **Animator** component.
- Confirm that the Animator's controller has the new clip as its default state.
- Confirm that the Light's Mode is **Realtime**.
- Test through the **Game** view, not only the Scene view.

### The bulb mesh does not glow

- Check that it uses **URP/Lit**, not an ordinary non-emissive material.
- Confirm that Emission is enabled in the material.
- Confirm that the animation property is `Emission Color` or `_EmissionColor`.
- Confirm that the bright key uses an HDR emission color rather than an ordinary dark color.
- Confirm that this object has its own material asset if other objects use the original material.

### The material clip affects the wrong object

- Duplicate the material asset.
- Assign the duplicate only to the flickering object.
- Re-add the material's emission property to the Animation Clip if necessary.

### Bloom does not show

- Enable **Post Processing** on the active camera.
- Confirm that the Global Volume affects that camera.
- Make the emissive material brighter.
- Lower Bloom's Threshold or raise its Intensity gradually.
- Check the result in the Game view.

## Final checklist

- [ ] The Light is set to **Realtime**.
- [ ] The root object has an Animation Clip and Animator.
- [ ] The clip keys the Light's **Intensity** and **Color**.
- [ ] The visible glowing mesh has a unique URP/Lit material.
- [ ] Emission is enabled in that material.
- [ ] The clip keys the material's **Emission Color**.
- [ ] Bright emission and light color match visually.
- [ ] The Animation Clip loops if required.
- [ ] The camera has Post Processing enabled.
- [ ] A base Global Volume contains Bloom.
- [ ] A stronger, higher-priority Global Volume can be pulsed by animating its **Weight**.
- [ ] The final effect has been checked in the Game view.
