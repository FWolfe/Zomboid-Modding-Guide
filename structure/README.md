## Contents
* [Back to Main](../../..)

  * [Overview](#overview)
    * [mod.info](#modinfo)
    * [media/](#media)
    * [media/scripts/](#mediascripts)
    * [media/models/](#mediamodels)
    * [media/textures/](#mediatextures)
    * [media/sound/](#mediasound)
    * [media/lua/](#medialua)
    * [media/lua/shared/](#medialuashared)
    * [media/lua/client/](#medialuaclient)
    * [media/lua/server/](#medialuaserver)
  * [Finding Mods on Disk](#finding-mods-on-disk)

----------------------------------------
## Overview
This section contains a brief description of the file and folder structure that makes up a mod. Your mod may include other folders in addition to these, but these ones are the most common.

##### ```mod.info```
This file can be created/edited with any text editor. It contains details like your mod's ID, name, description, preview image etc.
```
name=My First Mod
poster=poster.png
id=MyFirst
description=Basic example mod
url=https://theindiestone.com/forums/
```

##### ```media/```
The actual content of your mod will be placed in subfolders of the `media` folder

##### ```media/scripts/```
This folder contains text files with item, vehicles and recipe definitions. See the section on Scripts for more information.

##### ```media/models/```
Used for 3d models of weapons, vehicles etc.

##### ```media/textures/```
Icons, 3d model textures and the like go in here, in `.png` format. Pay attention to the naming scheme: Item icons should use the `Item_` prefix.

##### ```media/sound/```
Sound files go in here, in `.wav` or `.ogg` format.

##### ```media/lua/```
If your mod includes any .lua scripts, you will need this folder as well as at least one of the subfolders listed below. Any .lua files need to go in the subfolders, not directly in this one.

##### ```media/lua/shared/```
Used for lua scripts shared by client and server-side logic. These are the first Lua scripts that get loaded.

##### ```media/lua/client/```
Used for client-side scripts. UI elements, context menus timed actions and the like. These get loaded after any 'shared' lua scripts.

##### ```media/lua/server/```
Used for server-side scripts. Item spawning, core farming, weather and other server-side events. These only get loaded when the game is actually started (loading a save, starting a server, etc).

## Finding Mods on Disk
Mods are stored separate from the game, under the Steam workshop folder. On Windows, this looks like `C:\Program Files (x86)\Steam\workshop\content\108600`. Under this directory you'll find several numbered sud-directories, corresponding to the Steam Workshop IDs for each mod.
