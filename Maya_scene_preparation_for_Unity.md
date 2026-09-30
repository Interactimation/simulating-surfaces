# Preparing a Maya Scene for Unity

## What this guide covers

These steps prepare a Maya scene for export to Unity. Do this **after** modeling, UV mapping, texture work, and any object animation are complete.

Keep the original Maya scene. The exported FBX is a delivery copy, not the master file.

## 1. Save a clean master copy

1. Save the current file with a clear name, such as `graveyard_MASTER.ma`.
2. Make a separate export copy, such as `graveyard_UNITY_EXPORT.ma`.
3. Do the remaining cleanup work in the export copy.

Never delete work from the master merely because it is not needed in Unity. Keep lights, rigs, reference images, hidden objects, alternate versions, and experiments there if they are useful to you.

## 2. Check scale and scene orientation

1. In Maya, open **Windows > Settings/Preferences > Preferences**.
2. Under **Settings**, set **Working Units > Linear** to **centimeter**.
3. Keep Maya's usual **Y-up** orientation.
4. Work at a believable real-world scale. Unity treats one unit as approximately one meter, and an FBX exported from Maya's default centimeters should normally convert correctly.
5. If scale matters, make a quick test object: a cube measuring **100 cm** in Maya should be about **1 unit / 1 meter** tall in Unity.

Do not try to solve a scale problem by making random changes to Unity's import scale. Correct the model in Maya first whenever possible.

## 3. Organize the scene

1. Give objects clear, unique names. Use names such as `lampPost_01`, `cryptWall_A`, or `treeDead_03`.
2. Avoid spaces, punctuation, and vague default names such as `pCube17` or `group4`.
3. Create one top-level group for what will be exported, named something such as `SCENE_ROOT`.
4. Place the meshes, visible props, and any wanted object animation beneath that group.
5. Keep things that are only Maya working aids outside that group: image planes, reference meshes, control rigs, guides, test geometry, and unused versions.
6. If you need a collection of objects to remain placed together, put them in a clearly named placement group, such as `lampCluster_A_GRP`.

## 4. Clean up static meshes

Do the following only to ordinary, static mesh objects. Do **not** freeze or delete history from animated objects, joints, skinned characters, control rigs, lights, or cameras unless you understand the consequences.

For each static mesh:

1. Select the mesh's transform node.
2. Set its pivot where it will be useful in a game. For example, put a door's pivot at its hinge and a rotating prop's pivot at its center of rotation.
3. Choose **Modify > Freeze Transformations**.
4. In the Freeze Transformations options, freeze **Translate**, **Rotate**, and **Scale** for a self-contained static prop. Its channel box should then show approximately:

   ```text
   Translate: 0, 0, 0
   Rotate:    0, 0, 0
   Scale:     1, 1, 1
   ```

5. Choose **Edit > Delete by Type > History**.
6. Check **Mesh Display > Conform** if a surface is black, invisible from one side, or has inconsistent face direction.
7. Use **Mesh Display > Soften Edge** or **Harden Edge** deliberately, so the intended shading is visible before export.

### A safer way to place clean props

For a reusable prop that must be placed somewhere in the scene:

1. Freeze the mesh itself while it is clean and centered on its own pivot.
2. Put the mesh inside a parent placement group.
3. Move and rotate the **group**, not the mesh.

This leaves the mesh's own transforms clean while preserving its position in the larger scene.

## 5. Do not freeze animated or rigged objects

Freezing transforms can damage or change animation. Follow these rules:

- **Static prop:** freeze transforms and delete history.
- **Object with keyframed movement, rotation, or scale:** do not freeze its animated transform channels after animation begins.
- **Light or camera:** do not freeze it if it is positioned or animated.
- **Joint, skinned mesh, blend-shape character, or control rig:** do not freeze transforms or casually delete history. Export it only after testing its animation separately.

If animation depends on constraints, expressions, Set Driven Keys, or other Maya-specific controls, plan to **bake the animation** during FBX export.

## 6. Check geometry before export

1. Delete hidden duplicate objects and any geometry that should not be in the game.
2. Remove accidental internal faces, overlapping duplicate faces, and loose construction pieces.
3. Check that every visible mesh has the intended material assigned.
4. Confirm that UVs exist and do not overlap unintentionally.
5. Check the scene in Maya's textured viewport. Make sure the visible result is what you expect to export.
6. Keep polygon counts reasonable. A simple scene should not contain unneeded subdivision levels, dense sculpt meshes, or hidden high-resolution copies.
7. If an object uses Maya's smooth-mesh preview (`3` key), decide whether it needs real additional geometry before export. Do not assume the preview will become permanent game geometry.

## 7. Prepare texture files

1. Gather every texture image used by the exported scene: color/albedo maps, normal maps, opacity maps, roughness/metallic maps, and emission maps where applicable.
2. Put copies of them in one clearly named folder beside the FBX, for example:

   ```text
   Graveyard_Unity/
     Models/
       graveyard.fbx
     Textures/
       stone_color.png
       stone_normal.png
       lamp_emission.png
   ```

3. Use common image formats such as PNG, TGA, JPG, or TIFF.
4. Give texture files descriptive names. Include terms such as `_color`, `_normal`, `_metallic`, `_roughness`, `_opacity`, or `_emission`.
5. Do not rely on a texture being found only through a personal hard-drive path. The actual image files must travel with the exported model.
6. Make a note of which texture belongs in which material slot, especially for normal maps and emission maps.

## 8. Prepare lights and emissive objects as reference

Maya lights and Maya shader networks are useful as visual reference, but they are not the final game lighting system.

1. Keep the Maya lights and materials in the master scene.
2. If desired, keep simple lights in the export scene to preserve their approximate positions, colors, directions, and names as reference.
3. Name lights descriptively, such as `lampWarm_01`, `moonlight_KEY`, or `torchBlue_03`.
4. For every emissive object, use a clear material name such as `M_lampBulb_Emission`.
5. Make a note or screenshot of important light colors, intensities, cone angles, and flicker timing.
6. Do not assume that Maya's Arnold lights, area-light size, light linking, renderer settings, glow, or material-network behavior will survive the FBX export exactly.
7. Do not assume that an animated Maya light intensity or animated material emission will reproduce correctly at runtime. Preserve it in Maya as reference.

## 9. Prepare animation for export

For object animation that needs to travel with the FBX:

1. Set the correct start and end frames in Maya's timeline.
2. Play the animation once from beginning to end and correct any jumps, missing keys, or constraint errors.
3. Make sure the animated object and any required parent groups are inside `SCENE_ROOT`.
4. Do not use live simulation, constraints, expressions, or Set Driven Keys without baking them for export.
5. For simple keyframed movement, rotation, and scale, keep the animation intact and bake it during FBX export.

## 10. Export the FBX

1. Select `SCENE_ROOT`, or select only the objects that should go to Unity.
2. Choose **File > Export Selection**. Do **not** use ordinary Save or Export All unless the entire file is intentionally the game scene.
3. Choose **FBX export** and save into the `Models` folder.
4. Use a clear filename, such as `graveyard_scene_v01.fbx`.
5. In the FBX export options:

   - Export the selected objects only.
   - Include **Geometry**.
   - Include **Tangents and Binormals** if that option is available; they help normal-mapped surfaces shade correctly.
   - Include **Animation** only if the selection contains animation that should travel to Unity.
   - When exporting animation, enable **Bake Animation** and use the correct start and end frames.
   - Include **Deformed Models / Skins / Blend Shapes** only when the project needs them.
   - Leave the up-axis at its normal Maya default; do not manually rotate the scene to compensate for Unity.
   - Prefer carrying a separate `Textures` folder rather than relying on embedded media.

6. Export the file.

## 11. Final handoff checklist

Before handing the scene to Unity, confirm:

- [ ] The Maya master scene is saved separately.
- [ ] The export scene contains only intended game objects.
- [ ] Static meshes have clean transforms, sensible pivots, and deleted construction history.
- [ ] Animated objects, lights, cameras, joints, and rigs were not frozen carelessly.
- [ ] Mesh normals and hard/soft edges look correct in Maya.
- [ ] UVs and material assignments are present.
- [ ] Every texture image is in the handoff `Textures` folder.
- [ ] The FBX was exported with **Export Selection**.
- [ ] Animation, if needed, was baked into the FBX export.
- [ ] The exported FBX and texture folder remain together.

## What the FBX is expected to carry

The FBX should reliably serve as the handoff for:

- mesh geometry
- object names and hierarchy
- transforms and pivots
- UVs and mesh normals
- material assignments as labels/reference
- texture image files, when supplied separately
- basic object movement animation, when baked

Treat the finished Maya render as visual reference rather than a promise of identical Unity rendering.
