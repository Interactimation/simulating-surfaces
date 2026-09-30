# Setting Up a Maya Scene in Unity

## What this guide covers

Use these steps after receiving a cleaned Maya FBX file and its `Textures` folder. This guide uses a standard **Universal Render Pipeline (URP)** project.

If the project uses Unity's older Built-in Render Pipeline, create **Standard** materials instead of **URP/Lit** materials. The matching fields have slightly different names, but the workflow is the same.

## 1. Set up the Unity project folders

In the Unity **Project** window, create this folder structure inside `Assets`:

```text
Assets/
  Art/
    Models/
    Textures/
    Materials/
    Prefabs/
  Scenes/
```

1. Put the exported `.fbx` file in `Assets/Art/Models`.
2. Put all supplied image files in `Assets/Art/Textures`.
3. Wait for Unity to finish importing them.
4. Save a new Unity scene immediately: **File > Save As** and save it in `Assets/Scenes`.

Keep the FBX and its texture images in the Unity project. Do not depend on texture files remaining somewhere else on your computer.

## 2. Check the model's import settings

1. In the **Project** window, select the FBX file.
2. In the **Inspector**, open the **Model** import settings.
3. Check that the imported model is the correct size. A one-meter Maya test cube should appear about one Unity unit tall.
4. If the scale is wrong, first check the source FBX and Maya units. Do not make arbitrary scale changes to individual objects in the Unity scene.
5. Under normals and tangents:

   - Set **Normals** to **Import**.
   - Set **Tangents** to **Calculate** or **Import**. Do **not** set tangents to **None** when the model uses normal maps.
   - If the imported shading looks wrong, test the other tangent setting and compare it with the Maya reference.

6. Leave **Generate Lightmap UVs** off at first. Turn it on later only if you decide to bake lighting for the finished environment.
7. Leave **Generate Colliders** off unless the player must collide with this geometry. Add colliders deliberately rather than automatically to every object.
8. Turn off **Import Cameras**.
9. Turn off **Import Lights**. Maya lights are reference only; the final scene lights will be created and adjusted in Unity.
10. Click **Apply** if you changed anything.

Unity can import lights from an FBX, but their size and appearance often differ from Maya's. Rebuilding the lighting in Unity prevents confusing double-lighting and gives you control over the final look.

## 3. Place the imported scene

1. Drag the FBX from the **Project** window into the **Hierarchy** or directly into the **Scene** view.
2. Select its root object.
3. In the Inspector's **Transform** component, set the root position to `0, 0, 0`, rotation to `0, 0, 0`, and scale to `1, 1, 1` unless the scene deliberately needs a different placement.
4. Expand the hierarchy and make sure the expected meshes are present.
5. Rename the root object clearly, for example `GraveyardScene`.
6. Save the scene.

If you want to reuse the finished setup in more than one Unity scene, drag its configured root object from the **Hierarchy** into `Assets/Art/Prefabs`. This creates an editable Prefab copy of the setup.

## 4. Prepare the texture files

### Color / albedo textures

1. Select a color texture in `Assets/Art/Textures`.
2. In the Inspector, leave **Texture Type** set to **Default**.
3. Click **Apply** if needed.

### Normal maps

1. Select a normal-map texture, usually named with `_normal` or `_n`.
2. In the Inspector, change **Texture Type** from **Default** to **Normal map**.
3. Click **Apply**.
4. If Unity offers to fix the texture for use as a normal map, choose **Fix Now**.

If a normal map makes bumps look like dents, or produces incorrect shading, select that normal-map texture and try **Flip Green Channel** if your version of Unity shows that option. If the problem remains, compare it to the Maya reference and check the source texture.

## 5. Rebuild each material in Unity

Do not expect a Maya material or Arnold shader network to reproduce its final appearance in Unity. Make Unity materials deliberately.

1. In `Assets/Art/Materials`, right-click and choose **Create > Material**.
2. Give the material a clear name, such as `M_Stone`, `M_WoodDark`, or `M_LampBulb`.
3. With the material selected, set **Shader** to **Universal Render Pipeline > Lit**.
4. Drag the color/albedo texture into the material's **Base Map** slot.
5. Drag the prepared normal-map texture into the material's **Normal Map** slot.
6. Adjust the material's base color, **Metallic**, and **Smoothness** until it resembles the intended Maya reference.
7. Repeat for every distinct surface material.

### Assign materials to the meshes

1. Select a mesh object in the **Hierarchy**.
2. In the Inspector, find its **Mesh Renderer** component.
3. Under **Materials**, drag the Unity material into the correct material slot.
4. If the mesh has several material slots, assign the correct Unity material to each slot.
5. Repeat for the other objects in the scene.

Use one shared Unity material for objects that truly use the same surface. For example, ten identical stone blocks should normally all use the same `M_Stone` material.

## 6. Set up emissive materials

An emissive material makes a surface look as though it gives off light. It does not replace a Unity light that illuminates other nearby objects.

1. Select or create the material for the glowing object.
2. Use the **Universal Render Pipeline > Lit** shader.
3. Expand the material's **Emission** section.
4. Enable emission if it is not already enabled.
5. Set an emission color, or place an emission texture in the emission map slot.
6. Increase the emission intensity until the object looks bright enough in the **Game** view.
7. Save the material.

For a lamp, torch, computer screen, or glowing crystal, use both:

- an **emissive material** for the visible glowing surface; and
- a Unity **Point Light** or **Spot Light** to light the surrounding scene.

## 7. Recreate the lighting in Unity

Start with a simple lighting setup. It is easier to add and adjust lights one at a time than to repair a complicated imported lighting rig.

### Directional light: sun or moon

1. In the **Hierarchy**, select the default `Directional Light`, or create one with **GameObject > Light > Directional Light**.
2. Rename it clearly, such as `L_Sun` or `L_Moon`.
3. Rotate it to set the overall direction of the light.
4. Set its color, intensity, and shadow settings in the Inspector.

### Point light: bulb, torch, or local glow

1. Choose **GameObject > Light > Point Light**.
2. Place it near the visible light source, but not inside a wall or opaque object.
3. Give it a clear name, such as `L_Lamp_01`.
4. Adjust its **Color**, **Intensity**, and **Range** in the Inspector.
5. Enable shadows only when they visibly improve the scene. Too many shadow-casting point lights can hurt performance.

### Spot light: cone-shaped light

1. Choose **GameObject > Light > Spot Light**.
2. Position and rotate it so its cone points in the intended direction.
3. Adjust **Range**, **Spot Angle**, **Color**, and **Intensity**.
4. Use it for flashlights, stage lights, street lamps, or focused beams.

### Light mode

For initial scene setup, use **Realtime** lights so changes are visible immediately in the Game view. Once the environment is visually final, you may decide whether any of its lighting should be baked.

## 8. Set the environment light

1. Open **Window > Rendering > Lighting**.
2. In the **Environment** section, set a skybox or choose an ambient color that supports the intended mood.
3. Adjust ambient intensity sparingly. Too much ambient light makes shadows disappear; too little can make unlit areas completely black.
4. Return to the **Game** view often while adjusting. The Scene view is useful for construction, but the Game view shows the camera's actual image.

## 9. Add glow with Bloom

Bloom gives bright emissive objects a soft halo. It is the usual way to make a luminous bulb, neon tube, magical object, or screen visibly glow.

1. Choose **GameObject > Volume > Global Volume**.
2. In the new object's **Volume** component, create a new **Profile** if Unity asks for one.
3. Click **Add Override**.
4. Choose **Post-processing > Bloom**.
5. Enable the Bloom properties you want to adjust.
6. Start with a modest intensity and threshold, then raise them gradually while looking through the Game camera.

If Bloom appears to do nothing, check that the active Unity camera has post-processing enabled and that the emissive material is bright enough.

## 10. Check the scene through the Game camera

1. Select the `Main Camera` in the Hierarchy.
2. Move and rotate it to frame the scene as intended.
3. Open the **Game** tab.
4. Check the scene for:

   - missing textures
   - flat or reversed normal maps
   - material slots using the wrong material
   - overly bright or overly dark areas
   - lights inside solid meshes
   - duplicate lights
   - unexpected shadows
   - emission that needs more or less intensity

5. Save the Unity scene after each meaningful round of improvements.

## Final checklist

- [ ] The FBX is in `Assets/Art/Models`.
- [ ] Texture image files are in `Assets/Art/Textures`.
- [ ] The FBX imports at the correct size.
- [ ] Imported lights and cameras are turned off.
- [ ] Normal maps use **Texture Type: Normal map**.
- [ ] Unity materials have been created and assigned to every mesh.
- [ ] Emissive objects have Unity emission set up.
- [ ] Scene lighting has been recreated with Unity lights.
- [ ] The environment light supports the scene's mood.
- [ ] Bloom has been added if glowing objects need a visible halo.
- [ ] The result has been checked in the **Game** view and saved.
