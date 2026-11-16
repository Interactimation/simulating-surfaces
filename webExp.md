---
layout: page
title: Web Export
---

# Unity 6 WebGL to GitHub Pages

1. Open **File > Build Profiles > Add Build Profile** and select **Web**. If prompted, click **Install with Unity Hub**, install **Web Build Support**, and reopen Unity.
2. Leave any configuration presets unchecked. Click **Add Build Profile**, then **Switch Profile**.
3. In **Scene List**, make your starting scene the first checked scene. Use **Add Settings > Scene List** if the list is hidden and **Add Open Scenes** if your scene is missing.
4. Go to **Edit > Project Settings > Player > Web > Publishing Settings**. Set **Compression Format** to **Gzip** and enable **Decompression Fallback**.
5. Back in **Build Profiles**, click **Build** and save to a new folder named `webgl`.
6. Copy the entire `webgl` folder beside your GitHub Pages home page. Add `[Play the game](./webgl/index.html)` to the home page Markdown.
7. Commit and push to your site's publishing branch. Once Pages deploys, test the link.


> Decompression Fallback lets Unity unpack the build in the browser. _Unity Editor GitHub allows up to 100 MiB per file on a standard Git push._ If your Unity file is larger than this, see [Itch Upload](itch.md)