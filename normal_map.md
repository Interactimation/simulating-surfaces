---
layout: page
title: Normal Map
---

# Add a Normal Map to a Material in Maya

Assume the material already has its matching **Image Texture** connected to **Base Color** and the UVs are working correctly.

## 1. Open the Material

1. Open **Hypershade**.
2. Select the material that already has the Image Texture applied.
3. Open its **Attribute Editor**.

## 2. Add the Normal Map File

1. Find **Geometry → Bump Mapping** on the material.
2. Click the **checkerboard icon** beside **Bump Mapping**.
3. Choose **File**.
4. Browse to the matching **Normal Map** image.

The normal map should use the same UVs as the Image Texture, so the two textures should line up automatically.

## 3. Tell Maya This Is a Normal Map

Select the new **File** texture node.

Under **Color Space**, choose:

`Raw`

> Normal maps contain data rather than visible color, so they should not use an sRGB color transform.

## 4. Set the Bump Node

Maya creates a **bump2d** node between the Normal Map and the material.

Select the **bump2d** node and change:

**Use As:** `Tangent Space Normals`

This tells Maya that the image is a normal map rather than a grayscale bump map.

## 5. Check the Result

Switch the viewport to:

**Renderer → Arnold**

or make an **Arnold Render**.

You should now see small surface details—such as the edges and recesses between bricks—responding to the lighting without changing the actual geometry.

## If the Normal Map Looks Wrong

Check these first:

- **Color Space** on the Normal Map File node is `Raw`.
- **Use As** on the bump2d node is `Tangent Space Normals`.
- The Image Texture and Normal Map are a matching pair.
- Both textures are using the same UV set.
- The normal map has not been accidentally connected to **Base Color**.



