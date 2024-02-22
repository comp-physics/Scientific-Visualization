# Contours
Contours (and isosurfaces) can be very usefull in scientific visualization.
In order to create contors in Paraview, you must first have point data.
If your data is cell data, the `Cell Data to Point Data` filter must be applied.
After this, a `Contour` filter can be applied.
The basic properties of a contour figure are shown below.

<img src="../Resources/contours/A.png" alt="drawing" width="40%"/>

- Contour by is the variable contours will be taken over
- Isosurfaces is the list of values for which contours will be made at.
You can add and remove contour values by using the plus and minus buttons.
Also note that the value range can be used to help select contour values.

## Usefull Settings

- Opacity: Make the contours slightly see through.
This can be usefull if you want to show contours of one variable over a 2D image of another variable.

- Line Width: Paraview's default linewidth of 1 pixel is often too thin.
Increasing this makes your lines easier to see.

- Render Lines as Tubes: This renders lines as tubes and can make contours look better in 3D.

## Example
The following is an image of contours of velocity magnitude for a jet inpinging a solid wall.

<img src="../Resources/contours/B.png" alt="drawing" style="border-radius: 5%"/>