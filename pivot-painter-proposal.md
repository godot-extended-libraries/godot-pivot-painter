**Describe the project you are working on:**

Open world multiplayer game.

**Describe the problem or limitation you are having in your project:**

Skeleton animations are expensive for grass and foliage.

**Describe the feature / enhancement and how it helps to overcome the problem or limitation:**

Pivot painting will convert skeletal grass animation to hierarchical vertex animations.

**Describe how your proposal will work, with code, pseudocode, mockups, and/or diagrams:**

* Review the pivot painting reference
* Create a shader to read the files generated by the Blender plugin
* Import the sample Blender files from issue referenced in the pivot painting reference
* Make a Godot Engine 3.2 addon that provides this as an integrated package

**If this enhancement will not be used often, can it be worked around with a few lines of script?:**

* This can be done by an addon

**Is there a reason why this should be core and not an addon in the asset library?:**

* This is an addon suitable for the asset library
