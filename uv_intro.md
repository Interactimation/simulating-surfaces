---
layout: page
title: Tiling with UV (Intro)
---

# Tiling Textures with UV Mapping (Intro)

We'll apply the stone wall texture to a modeled wall\
The texture will have to tile along the length of the wall and show only a part of itself on each end of the wall\
We don't cre about the top of the wall because we'll pur a concrete "cap" on it... or the bottom of the wall because that will contact the ground!

## Model the Wall

1. Create a polygon cube.
2. Scale it into a long, low wall.
3. Finish adjusting its proportions before making the UVs.
4. Select the wall and choose **Modify → Freeze Transformations**.

> You do not need to apply the texture before scaling or creating the UVs. Applying it earlier only lets you preview your UV adjustments.

## Create the UVs

1. Select the wall.
2. Create or recreate its UVs.
   - For a simple box, **UV → Automatic** is sufficient.
3. Open **UV → UV Editor**.

## Apply the Stone Texture

1. Select the wall.
2. Right-click and hold over it in the main viewport.
3. Choose **Assign New Material**.
4. Choose **Arnold → aiStandardSurface**.
5. Click the checker icon beside **Base Color**.
6. Choose **File** and load the stone image.
7. Press **6** to display the texture in the viewport.

The texture may initially appear too large, stretched, or turned sideways. This is corrected by editing the UV shells.

## Select a UV Shell

1. In the UV Editor, right-click and hold directly over the UVs.
2. Choose **UV Shell**.
   - Some versions of Maya label this simply **Shell**.
3. Click one of the long wall shells.
   - Shift-click to select additional shells.

## Rotate the UV Shell

1. Open the **UV Toolkit**.
2. Expand **Transform**.
3. Scroll down inside the toolkit if you see only **Pivot** and **Move**. **Rotate** and **Scale** are farther down.
4. Rotate the shell **90°** so its long dimension runs horizontally across the stone texture.
   - Use the Rotate gizmo.
   - You can also press **E** to activate it.
   - Use **+90°** or **−90°**, depending on the shell's orientation.

## Scale the UV Shell

1. With the shell selected, press **R** to activate the Scale gizmo.
2. Drag the center handle to scale the shell uniformly.
3. Enlarge it until the shell is approximately **one UV tile high**.
4. Allow its length to extend across as many UV tiles as needed.

If the texture fills one UV square:

- One square high produces one complete texture tile vertically.
- Several squares wide repeat the texture along the wall.
- The narrow ends of the wall should occupy less than one square horizontally.

UV shells do not need to remain inside the first 0–1 square. Extending them across additional squares makes the texture repeat.

> A larger UV shell produces smaller stones and more repetitions. A smaller UV shell produces larger stones and fewer repetitions.

Use uniform scaling whenever possible so the stones do not become stretched.