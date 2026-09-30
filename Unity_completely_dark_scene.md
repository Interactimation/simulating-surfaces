# Making a Completely Dark Unity Scene

## Goal

Set up the Unity scene so that, before you add any deliberate Unity lights, ordinary objects are completely black in the **Game** view.

After this setup, objects should become visible only when lit by lights you create in Unity. This guide assumes a standard **Universal Render Pipeline (URP)** project. In Unity's older Built-in Render Pipeline, use the equivalent settings with slightly different labels.

> The Main Camera does **not** emit light. However, its sky/background setting can show a bright sky, and the Unity Scene view can show an editor-only default light. Both must be addressed.

## 1. Remove or disable all scene lights

1. In the **Hierarchy**, find every object with a **Light** component.
2. Delete or disable the default `Directional Light`.
3. Delete or disable any Point Lights, Spot Lights, Area Lights, or imported Maya lights.
4. If the scene contains a parent object such as `Lights`, expand it and check for hidden child lights.
5. Check that no light GameObject is inactive only by accident. Either remove it deliberately or leave it clearly disabled for later use.

At this point, there should be no enabled **Light** components in the Hierarchy.

## 2. Set the camera background to solid black

1. Select `Main Camera` in the **Hierarchy**.
2. In the Inspector, find the camera's **Environment** section.
3. Change **Background Type** from **Skybox** to **Solid Color**.
4. Set the background color to pure black:

   ```text
   R: 0
   G: 0
   B: 0
   A: 255
   ```

5. If the project uses Unity's older Built-in Render Pipeline, set **Clear Flags** to **Solid Color**, then set **Background** to black.

This changes what the camera draws behind the scene. It does not itself light any object.

## 3. Remove the skybox and ambient environment light

1. Open **Window > Rendering > Lighting**.
2. Open the **Environment** section.
3. Set **Skybox Material** to **None**.
4. Under **Environment Lighting**, set **Source** to **Color**.
5. Set **Ambient Color** to black.
6. Set **Intensity Multiplier** to `0`.
7. Set **Sun Source** to **None**, if that field is present.
8. Under **Environment Reflections**, set **Intensity Multiplier** to `0`.
9. If there is a Reflection Source setting, set it to a black custom cubemap or leave it unused while its intensity is `0`.
10. Turn **Fog** off. If you need fog later, make it black and deliberately re-enable it.

The ambient light and environment reflections are easy to overlook. They can make objects visible even after every ordinary light has been removed.

## 4. Clear lighting that was baked earlier

Previously baked light can remain visible after its original lights have been deleted.

1. Keep the **Lighting** window open: **Window > Rendering > Lighting**.
2. Find the button named **Clear Baked Data**.
3. Click **Clear Baked Data**.
4. If the project contains a Lighting Settings asset, verify that it is not automatically generating old lighting data again.
5. Save the scene.

If the scene has any of these objects from an earlier lighting setup, delete or disable them unless you intend to use them later:

- Reflection Probes
- Light Probe Groups
- Adaptive Probe Volumes
- baked-lighting helper objects

## 5. Make sure the materials do not illuminate themselves

For the strict darkness test, every visible object should use a light-reactive shader with emission turned off.

1. Select each Unity material used by the scene.
2. Check its **Shader** field.
3. Use **Universal Render Pipeline > Lit** for ordinary surfaces.
4. Do not use an **Unlit** shader for ordinary objects. An Unlit material stays visible even when the scene has no lights.
5. Expand the material's **Emission** section.
6. Disable emission, remove any emission map, or set the emission color to black.
7. Repeat for all materials, including screens, bulbs, sky cards, particle materials, and decals.

An object with an emissive material is a visible light source even without a Unity Light component. Turn that off for this initial test.

## 6. Disable lighting-related effects during the darkness test

1. In the Hierarchy, look for a `Global Volume` object.
2. Temporarily disable it, or disable its **Bloom** override.
3. Disable any custom sky, volumetric effect, or screen-space effect that makes the scene look lit.
4. Check for particle systems using bright or unlit materials and temporarily disable them.

These effects can be restored one by one after you establish a genuinely black baseline.

## 7. Make the Scene view show real lighting

The **Scene** view can use a temporary editor light that does not exist in the actual game. This is useful while modeling, but misleading while checking darkness.

1. Click inside the **Scene** view.
2. Find the **Scene Lighting** toggle in the Scene view toolbar. It is usually shown with a small sun or light icon.
3. Turn **Scene Lighting on** so the Scene view uses the actual lights in the Unity scene.
4. Do not turn it off while testing darkness. When it is off, Unity can show the Scene view with a default editor light.
5. Use the **Game** view as the final test. It is the view the player will actually see.

## 8. Test the black baseline

1. Make sure the active camera can see a normal mesh that uses a **URP/Lit** material.
2. Open the **Game** tab.
3. The mesh should be completely black or invisible against the black background.
4. Add one temporary light: **GameObject > Light > Point Light**.
5. Place it near the mesh and give it a visible intensity and range.
6. Return to the Game view. Only the part of the mesh reached by that Point Light should be visible.
7. Delete the temporary light or keep it as the first deliberate scene light.

If the mesh is visible **before** you add that temporary light, work through this checklist again:

- Is there still an enabled Light component somewhere in the Hierarchy?
- Is the material Unlit or emissive?
- Is ambient intensity really `0`?
- Is reflection intensity really `0`?
- Was baked data cleared?
- Is a Reflection Probe, Light Probe Group, or Adaptive Probe Volume still providing lighting?
- Is the Game camera background set to black rather than Skybox?
- Is the Scene view showing a default editor light rather than actual Scene Lighting?

## 9. Add only deliberate Unity lights

Once the black baseline works, create the lighting you actually want.

1. Create lights through **GameObject > Light**.
2. Use a **Directional Light** for sun or moonlight.
3. Use a **Point Light** for bulbs, torches, and local glow.
4. Use a **Spot Light** for focused cones of illumination.
5. Give every light a clear name, such as `L_Moon`, `L_HallLamp_01`, or `L_Torch_03`.
6. Add lights one at a time and check each change in the **Game** view.
7. Only restore emission, Bloom, fog, reflections, or other visual effects when you choose to use them deliberately.

## Final checklist: completely dark starting point

- [ ] No enabled Light components remain in the scene.
- [ ] Main Camera uses a **Solid Color** black background.
- [ ] Skybox Material is **None**.
- [ ] Environment Lighting is black with intensity `0`.
- [ ] Environment Reflections have intensity `0`.
- [ ] Fog is off.
- [ ] Old baked lighting has been cleared.
- [ ] Reflection Probes, Light Probe Groups, and Adaptive Probe Volumes are absent or disabled.
- [ ] Visible materials use **URP/Lit**, not Unlit.
- [ ] Emission is off or black on every material.
- [ ] Bloom and other light-like effects are temporarily off.
- [ ] Scene Lighting is on in the Scene view.
- [ ] In the Game view, a normal Lit object is black until you add a Unity light.
