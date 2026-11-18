---
layout: page
title: Itch
---

## Publish a Unity Game on itch.io for Browser Play

Already have a Unity Web build? Start at step 4.

1. In Unity 6, open **File > Build Profiles > Add Build Profile** and select **Web**. If prompted, click **Install with Unity Hub**, install **Web Build Support**, and reopen Unity.
2. Add the Web profile and click **Switch Profile**. Make your starting scene the first checked scene in **Scene List**. Use **Add Open Scenes** if needed.
3. Click **Build** and choose an empty folder.
4. ZIP everything **inside** the build folder so `index.html` is at the top level of the ZIP.
5. On itch.io, create a project and set **Kind of project** to **HTML**. Upload the ZIP and check **This file will be played in the browser**.
6. Under **Embed options**, choose **Embed in page** and enter the width and height of your game window.
7. Click **Save & view page** and test the game. When ready, set **Visibility** to **Public** and save.