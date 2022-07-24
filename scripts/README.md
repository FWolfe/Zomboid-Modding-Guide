## Contents
* [Back to Main](../../..)

  * [Overview](#overview)  
  * [The module block](#the-module-block)  
  * [The imports block](#the-imports-block)  
  * [The item block](#the-item-block)  
  * [The recipe block](#the-recipe-block)  
  * [The evolvedrecipe block](#the-evolvedrecipe-block)  
  * [The fixing block](#the-fixing-block)  
  * [The sound block](#the-sound-block)  
  * [The vehicle block](#the-vehicle-block)  

----------------------------------------
## Overview
Items, recipes, vehicles and similar stuff is defined in .txt files in the `media/scripts/` directory. These files are separated into various `{ block }` types. Within these blocks exist items properties: its name, how much it weighs, what type of item it is, how much time recipes take, what ingredients are required, how much horsepower a vehicle has, etc.

Each of the various block types has different attribute/value types, and syntax can vary slightly from other block types (items vs recipes), but the overall format is the same:

A basic example:
```
/** This line is a comment. anything inside these brackets is ignored **/
module MyMod {
    imports {
        Base
    }
    /** Comments can be multiline like this
        and can appear anywhere in the file, even in the
        middle of a { block }
    **/
    item MyItem
    {
        Type         = Normal,
        DisplayName  = My First Item,
        Icon         = MyIcon,
        Weight       = 0.1,
    }
}
```

----------------------------------------
### The module block
In the example above, you'll notice the file starts off with `module MyMod`. All items, recipes and other blocks need to be contained within the module block.  
The name used is used as a prefix for a item's full name: The item in the example is `MyMod.MyItem`.
Most (not all) vanilla items use the module `Base`. You are free to use this module as well but be aware defining things like items that already exist in the `Base` module you will **overwrite** them.  

If you have lots of items or don't want to conflict with any existing items or other mods its best to use a custom (or multiple) module names. Common practice is to use the same name as the mod ID.  

Sometimes for basic mods or compatibility reasons it may be better to use a existing module name such as `Base`. Creating a extra module namespace for a few simple items is not always the best option: unnecessary  modules are very minor performance hit.

----------------------------------------
### The imports block
Often for recipes and other blocks you will need to reference items from other modules. Normally when a item exists in a different script module, you have to use the full name such as `Base.Nails` to reference it. By declaring a `imports` block you can skip the module prefix on the item and just use `Nails`
```
imports {
    Base
}
```
*Note: when loading mods scripts, PZ will log a warning to the console if you do not import Base. This warning can be ignored unless you intended to import.*


----------------------------------------
### The item block
**_TODO: explain the item block, the various types of items and valid property differences between types._**

----------------------------------------
### The recipe block
Recipes are one form of crafting in Zomboid. They take input 'ingredients' (food, tools, items, etc) and produce a output item.   
Pay attention to the syntax differences between `item` blocks and `recipe` blocks. Items use the familiar `property = value,` syntax while recipes use `property:value,` and `=` are used strictly for item counts.

In its most basic form, a recipe has the following structure:
```
recipe Open Box of Nails
{
    NailsBox,

    Result:Nails=20,
    Sound:PutItemInBag,
    Time:5.0,
}
```
The first line `NailsBox,` is our input item, followed by a blank line. Input items are destroyed on recipe creation unless `keep` has been specified (more on that later).  
`Result:Nails=20,` is obviously the output item, `=20` is the number of this item to give the player. **Note** this number is multiplied by the resulting item's `count =` property in its `item` block definition. Nails have `count = 5`, so our end result is `20 * 5 = 100 Nails`  
`Sound` is the audio clip to play and `Time` how long this action takes.

Naturally the reverse of this recipe is:
```
recipe Place Nails in Box
{
    Nails=100,

    Result:NailsBox,
    Sound:PutItemInBag,
    Time:5.0,
}
```
A slightly more advanced recipe that uses multiple ingredients which can be different and calls lua functions:
```
recipe Slice Fillet
{
    keep KitchenKnife/HuntingKnife,
    Bass/Catfish/Perch/Crappie/Panfish/Pike/Trout,

    Result:FishFillet=2,
    Sound:SliceMeat,
    Time:50.0,
    OnTest:CutFish_TestIsValid,
    OnCreate:CutFish_OnCreate,
    Category:Cooking,
    OnGiveXP:Give10CookingXP,
}
```
Here you can see the ingredients are a knife and a fish. `keep` is declared before the knife to ensure it isn't destroyed after, and `/` is used to separate valid items (ie: KitchenKnife **OR** HuntingKnife).  
`OnTest` and `OnCreate` are *global* Lua functions called, the first for testing if the recipe is valid, and the second when the recipe has finished.  
`Category` controls which tab this recipe appears on in the player's Build Window. You can easily define custom tabs simply by using custom values here.  
`OnGiveXP` is another *global* Lua function. There are many predefined for giving various xp values to different skills, or you can create your own function.

----------------------------------------
### The evolvedrecipe block

----------------------------------------
### The fixing block
When fixing an item in Project Zomboid Build 41.72, you just need the repair item and the fixer; the template example as follows:
```
fixing Fix Baseball Bat
{
 Require : BaseballBat,

 Fixer : Woodglue=2; Woodwork=2,
 Fixer : DuctTape=2,
 Fixer : Glue=2,
 Fixer : Scotchtape=4,
}
```
----------------------------------------
### The sound block
PZ build 40 introduced new audio code to deal with issues in multiplayer. All sounds now need to be defined in a `sound` block to have them heard by players farther then 20 tiles. Defining a sound here also includes it in PZ's options advanced audio tab.  

If you don't have a sound listed here, the first time it plays it will get added to the audio tab, its MP radius will get set to 20 and a message will appear in the console: `WARNING: no GameSound called "MySound", adding a new one`

The vanilla shotgun blast is defined in the `item` block as:
```
    SwingSound	=	FirearmShotgun,
    SoundRadius	=	200,
```
And in the `sound` block as:
```
sound FirearmShotgun
{
    category = Item,
    clip
    {
        event = Weapons/Firearm/shotgun2,
        distanceMax = 200, /* SoundRadius in items.txt */
    }
}
```
The `SwingSound` must match the sound name, while the `SoundRadius` in the item block is *only the zombie agro distance*, `distanceMax` is the range other players will hear it.  
`event` only applies to sound clips that are in the vanilla soundbanks, for custom sounds we need to use `file`
```
sound Remington870
{
    category = Item,
    clip
    {
        file = media/sound/Remington870.ogg,
        distanceMax = 200,
    }
}
```


----------------------------------------
### The vehicle block
**_TODO: Outline syntax differences, template files, and important properties._**

