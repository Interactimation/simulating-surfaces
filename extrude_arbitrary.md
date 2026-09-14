# Create / Extrude an Arbitrary Polygon

# You can draw your own polygons and extrude them into 3D

> **EXAMPLE:** The classic [arched tombstone](https://davisgraveyard.com/dgblog/wp-content/uploads/2024/04/PXL_20240407_000909630.PORTRAIT-scaled.jpg):

* Go to Front View so you're drawing on a flat plane.
* Choose Mesh Tools → Create Polygon.
* Click points around the tombstone silhouette:
    * bottom left
    * up the left side
    * several points around the arch
    * down the right side
bottom right
    * click the first point again, or press Enter, to close the polygon.

* Switch to Face mode and select the large face.
* Press Ctrl/Cmd + E to Extrude.
* Drag Thickness outward to make the stone three-dimensional.

If the resulting object is featureless black, it's normals may be facing the wrong direction.

* Pull Down, from the top row, Mesh Display and choose "Reverse"

> **SUGGESTION:** For the arch, use perhaps 6–10 points around the curve rather than trying to make it with only two or three. You can then adjust the vertices afterward to make the curve cleaner.

> **ALTERNATIVE:** For a perfectly symmetrical tombstone, there's an even cleaner method: draw only half the silhouette, extrude it, then [Mirror it](https://www.youtube.com/watch?v=k6MKu9YxXr0).

## A Different Process Uses the Multi-Cut Tool

* Create a Cube and scale it to tombstone proportions.
* Go to top View.
* Shift Right Click → Multi-Cut
* Hold down Cntrl+Shift and click to make a single edge

Repeat this several times across the top

You can then move the edges or their vertices into an arch
Add more cuts if you need a smoother curve.
