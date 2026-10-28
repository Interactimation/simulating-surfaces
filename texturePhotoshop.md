---
layout: page
title: Textures in Photoshop
---

# Photoshop

We can [create our own tileable image textures](https://www.youtube.com/watch?v=mQLXH3HpYik) in photoshop

In class we'll attempt a more difficult tiling of brick textures, taken as something less than a 90˚ angle… and with a little ingenuity, we might be able to make our own Height or Bump Maps

For looser textures like pebbles or stucco:

## Seamless Pebble or Stucco Texture in Photoshop

1. Start with a sharp, full-resolution photograph. Shoot straight at the surface in even light; avoid shadows and blurry areas.

2. Open it in Photoshop. Save a working copy as a **PSD**.

3. Select the **Crop tool (C)**. Choose **1:1**, enable **Delete Cropped Pixels**, and crop a square.

4. Open **Image > Image Size**. Use **2048 × 2048 pixels**.
   - Reduce a larger image; avoid enlarging a smaller one.
   - Pixel dimensions determine screen detail; the PPI setting does not.

5. Choose **Filter > Other > Offset**:
   - **Horizontal:** 1024 pixels
   - **Vertical:** 1024 pixels
   - **Undefined Areas:** Wrap Around

6. Select **Clone Stamp (S)**. Set **Opacity** and **Flow** to **100%**. Use a small brush with a moderately firm edge.

7. **Alt-click** (Windows) or **Option-click** (Mac) a clean area, then paint over the central seams in short strokes.
   - Sample fresh areas frequently.
   - For pebbles, preserve whole stones; place joins between them.
   - Avoid painting across the outside borders.
   - Avoid blurring or smudging: copy sharp detail over the seams.

8. Apply the same **Offset** again. Repair remaining seams. Check detail at **100% zoom**.

9. Enable **View > Pattern Preview** and zoom out. Check for visible joins or obvious repeating patches; repair and check again.

10. Save the **PSD**, then save a **PNG** at the same pixel dimensions.

### Testing Tileability

### Test a seamless tile in Photoshop

1. Open your tile.
2. Choose **Select > All**.
3. Choose **Edit > Define Pattern**. Name it and click **OK**.
4. Choose **File > New**. Make the image at least three times the tile’s width and height.
   Example: 512 × 512 tile → 1536 × 1536 image.
5. Choose **Edit > Fill**. Set **Contents** to **Pattern**.
6. Under **Custom Pattern**, select your tile. Click **OK**.
7. Inspect for visible seams, gaps, and obvious repetition. Check seams at **100% zoom**.
8. After editing the original tile, define a new pattern and repeat the test.

### Delete unused patterns

1. Choose **Window > Patterns**.
2. Select an unwanted pattern.
3. Open the panel menu and choose **Delete Pattern**. Confirm.

