# Pivot Painting

The aim of the addon is to store object properties in the textures.

Modified from https://github.com/Gvgeo/Pivot-Painter-for-Blender/issues/4

## The case of a plant/tree

For the creation of the textures, user has to create each part that need to move as a separate object with correct origin, rotation, and parent relations, need to apply scale and location. (Afterwards parts can be merged as needed.)

Addon will create textures with enough pixels to store each object information separate (1 pixel per object). 

To be able to read the info, also creates a uv map which points all the object's vertices to the same object's pixel.

Information in textures is stored from left to right and top to bottom, based on objects indices.

In godot will need to rotate the vertices from the object's origin while accounting parent's current offsets.

----

Texture name -> Name in code -> Function name

#### RGB CHANNELS

Pivot Point HDR -> PivotPoint -> pivotarray
Position of the origin of each object.
The location from which vertices will rotate.

Origin Position HDR -> OriginPosition -> originArray
Position of the center of the bounding box.

Origin Extents HDR -> OriginExtents -> ExtentsArray
Dimensions in local coordinates.

X Axis -> Xaxis -> xaxisArray
Y Axis -> Yaxis
Z Axis -> Zaxis
The direction of the local x axis.
Mostly used with X extend, to control the rotation amount. (vertex distance from origin vs object's size)

#### ALPHA CHANNEL

HDR - Parent Index -> Index -> indexarray
Has bit manipulation, storing int into float. Function: packTextureBits.
Index of the parent.
To access properties from parent's pixel in the texture. Used to inherit parent properties(rotation, position etc.) at runtime.

HDR - Number of Steps From Root -> Steps -> level
Number of parents for every object till you reach root.

HDR - Random 0-1 Value Per Element -> Randomhdr -> randomfloat
Random float. Same as SDR.

HDR - Bounding Box Diameter -> Diameter -> diagonal
Diagonal length of the bound box.

HDR - Selection Order -> SelectionOrder -> customorder
Has bit manipulation, storing int into float. Function: packTextureBits.
The order the objects were selected in blender while using selection order function of the addon.
Used in fortnite's style build system.

HDR - Normalized 0-1 Hierarchy Position -> Hierarchyhdr -> two parts in "level" and "setpixels"
Hierarchy, current level of the object / highest possible level. Same with SDR.
level as in  "HDR - Number of Steps From Root".

HDR - Object X Width -> Xwidth -> xextent (Xextent contains optional calculations from boundbox, read yextent if need)
HDR - Object Y Depth -> Ydepth -> yextent
HDR - Object Z Height -> Zheight -> zextent
The length of the object on the local x axis.
value = value / 8

Normalized 0-1 Hierarchy Position -> Hierarchy -> two parts in "level" and "setpixels"
Hierarchy, current level of the object / highest possible level. Same with HDR.

Random 0-1 Value Per Element -> Random -> randomfloat
Random float. Same as HDR.

X extent -> Xextent ->  
Y extent -> Yextent -> yextent
Z extent -> Zextent -> 
The length of the object on the local x axis. (Value 0-1)
value = value / 8
Same as HDR with:
    clip (value, 1, 256)
    value = value / 256

HDR - Scaled Bounding Box Diameter -> Diameterscaledhdr-> Diagonal length of the bound box
Diagonal length of the bound box scaled.
Same with "HDR - Bounding Box Diameter" with obj.matrix_world.to_scale() first.

Scaled Bounding Box Diameter -> Diameterscaled -> diagonalscaled
Diagonal length of the bound box scaled.
Same as HDR with:
    clip (value, 1, 256)
    value = value / 256

#### Bit manipulation, for Index and SelectionOrder

Storing int into float. Function: packTextureBits

	index = index +1024
	sigh=index&0x8000
	sigh=sigh<<16
	exptest=index&0x7fff
	if exptest==0:
		exp=0
	else:
		exp=index>>10
		exp=exp&0x1f
		exp=exp-15
		exp=exp+127
		exp=exp<<23
	mant=index&0x3ff
	mant=mant<<13
	index=sigh|exp|mant
  
------------------------

https://docs.blender.org/api/current/
