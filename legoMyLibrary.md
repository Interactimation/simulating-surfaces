---
layout: page
title: Lego Library
---

# Lego my Library

The assignment is to create a "toy" model, in Maya, of the Library.
Don't worry about scale and too much detail. 
_Think of it as making a _Lego_ version of the library, a sort of 3D sketch._

For windows you can just delete faces, or you can explore transparent materials, _your call!_

Use simple geometries, tilable textures and their corresponding "bump" or "height" maps. You can use anyone's images, texture or maps from the shared folder but be sure maps go with textures!

For now you can leave off the signage, tho bonus points go to anyone who tries a little 3D text on the walls!

THE GOAL IS CUTE, TOYLIKE, SIMPLE, APPEALING

------

## Maya Refresher: Materials and Modeling

Use the **Modeling** menu set and polygon primitives with their default UVs.
Press **6** over the viewport to display textures.

## 1. Plain-color materials

1. Right-click the object → **Assign New Material → Lambert**.
2. In the **Attribute Editor**, rename the material.
3. Click the **Color** swatch and choose a color.

## 2. Tileable images and height maps

### Add the color image

1. Create and assign a new **Lambert**, as above.
2. Click the checker button beside **Color** → **File**.
3. Beside **Image Name**, click the folder and open your tileable color image.
4. Open its **place2dTexture** tab. Set **Repeat UV** to, for example, **4, 4**. Higher values make smaller, more frequent tiles.
5. Adjust **Rotate UV** and **Offset** if needed.

### Add and adjust the height map

1. Right-click the object → **Material Attributes**.
2. Click the checker beside **Bump Mapping** → **File**.
3. In the **bump2d** tab, set **Use As** to **Bump**.
4. Follow the connection arrow beside **Bump Value** to the File node; load the matching height image under **Image Name**.
5. Set **Color Space → Raw**; under **Color Balance**, enable **Alpha Is Luminance**.
6. In this image's **place2dTexture** tab, match the color image's **Repeat UV**, **Rotate UV**, and **Offset**.
7. Return to **bump2d**. Start **Bump Depth** at **0.1** and adjust. A negative value reverses raised and recessed detail.

## 3. Materials on individual faces

1. Hold right-click over the object → **Face**.
2. Select faces; **Shift-click** to add more.
3. Right-click → **Assign Existing Material** → choose your material.
4. Hold right-click → **Object Mode** when finished.

## 4. Edge loops

1. Select the object → **Mesh Tools → Insert Edge Loop**.
2. Click-drag an edge to position the loop; release to insert it.
3. Repeat as needed; press **Q** to finish.

Loops continue through four-sided faces; triangles can stop them.

## 5. Extruding faces

1. Hold right-click over the object → **Face**; select the faces.
2. Choose **Edit Mesh → Extrude**.
3. Adjust **Thickness** to push or pull; **Offset** to inset.
4. Keep **Keep Faces Together** on for a connected extrusion; off for separate faces.
5. Press **Q** to finish.