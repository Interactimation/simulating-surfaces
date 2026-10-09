# Preparing a Maya Scene for Unity

For a static scene. Add animation later in Unity.

## 1. Save an export copy

1. Save your original Maya scene.
2. Use **File > Save Scene As** to create `graveyard_UNITY_EXPORT.ma`.
3. Do the following cleanup in this copy.

## 2. Prepare the meshes

1. Give objects and materials clear names.
2. Remove unwanted geometry and working aids from the export copy.
3. Check object size. Under **Windows > Settings/Preferences > Preferences > Settings**, check the linear units and keep **Y-up**. With centimeter units, 100 cm represents one meter.
4. Check UVs, material assignments, and texture appearance.

For each ordinary static mesh:

1. Select it in Object Mode.
2. Choose **Edit > Delete by Type > History**.
3. Hold **D** and move its pivot where needed; release **D**.
4. Choose **Modify > Freeze Transformations**. In its options, enable **Translate**, **Rotate**, and **Scale**.
5. Check the shading. Use **Mesh Display > Soften Edge** or **Harden Edge** where needed.

Do not apply this cleanup to rigs, skinned meshes, or animated objects.

## 3. Group the objects for export

1. Select the meshes you want in Unity.
2. Press **Ctrl+G**.
3. Name the group `SCENE_ROOT`.

To add an object later, select the object first, then Shift-select `SCENE_ROOT` in the Outliner and press **P**.

Keep Maya lights as reference. Recreate lighting and materials in Unity; take a screenshot of the Maya scene if useful.

## 4. Create the folders inside your Unity project

1. Open your Unity project.
2. In Unity's **Project** panel, right-click **Assets > Show in Explorer**.
3. In Windows File Explorer, open `Assets` if needed.
4. Inside `Assets`, create a folder named `Graveyard_Unity`.
5. Inside it, create `Models`, `Textures`, and `Materials`.

Your folders should be:

```text
YourUnityProject/
  Assets/
    Graveyard_Unity/
      Models/
      Textures/
      Materials/
```

These are folders you create inside the Unity project.

## 5. Copy the texture images

1. In Maya, open **Windows > General Editors > File Path Editor**.
2. Expand the listed folders to see the image filenames and locations.
3. Open those locations in Windows File Explorer.
4. Copy the images with **Ctrl+C**.
5. Paste them with **Ctrl+V** into `Assets/Graveyard_Unity/Textures`.

Include color images, normal maps, and any opacity, metallic, roughness, or emission images used by your scene.

**Copy the files; leave the originals where Maya expects them.** Keep their existing filenames. Note which images belong to each material.

## 6. Export the FBX from Maya

1. Select `SCENE_ROOT` in the Outliner.
2. Choose **File > Export Selection**.
3. Set **Files of type** to **FBX export**.
4. Browse to your Unity project's `Assets/Graveyard_Unity/Models` folder.
5. Name the file `graveyard.fbx`.
6. In the FBX options:
   - Enable **Smoothing Groups**.
   - Disable **Animation**.
   - Disable **Embed Media**; you copied the images separately.
   - Keep **Units > Automatic** enabled and **Up Axis > Y**.
7. Click **Export Selection**.

## 7. Check the files in Unity

1. Return to Unity and wait for importing to finish.
2. Open **Assets > Graveyard_Unity** in the Project panel.
3. Check that the FBX appears in `Models` and the images appear in `Textures`.
4. Select each normal-map image.
5. In the Inspector, set **Texture Type > Normal map**, then click **Apply**.
6. Leave color images set to **Default**.

The files are now imported. Next, create Unity materials in `Materials`, assign their images, and apply the materials to the meshes.
