# Project Zomboid Modding Guide
**_This document is very much a Work-In-Progress._**

## Contents
* [Introduction](#Introduction)
  * [About this guide](#about-this-guide)  
  * [Overview](#overview)  
  * [Required Tools](#required-tools-software)
* [Mod Structure](#mod-structure)
* [The Scripts](#the-scripts)  
  * [The module block](#the-module-block)  
  * [The imports block](#the-imports-block)  
  * [The item block](#the-item-block)  
  * [The recipe block](#the-recipe-block)  
  * [The evolvedrecipe block](#the-evolvedrecipe-block)  
  * [The fixing block](#the-fixing-block)  
  * [The sound block](#the-sound-block)  
  * [The vehicle block](#the-vehicle-block)  
* [The Code](#the-code)  
  * [New To Programming](#new-to-programming)  
  * [New To Lua](#new-to-lua)  
  * [Zomboid's Lua Component](#zomboids-lua-component)  
  * [The Vanilla Lua](#the-vanilla-lua)  
  * [Zomboid's API](#zomboid-api)  
  * [Decompiling The Java](#decompiling-the-java)  
  * [Overwriting Vanilla Code](#overwriting-vanilla-code)  
  * [Overwriting Another Mod's Code](#overwriting-another-mods-code-3rd-party-patching)  
  * [Performance Tips](#performance-tips)  
    * [Global vs Local]()
    * [Event Callbacks]()
    * [Misc Performance Tips]()
  * [Code Quality Tips](#code-quality-tips)  
  * [Code Snippets](#code-snippets)  
* [Translations](#translations)  
* [The Maps](#the-maps)  
* [The Models](#the-models)  

----------------------------------------------------------------------------------
----------------------------------------------------------------------------------
## Introduction

### About this guide
The goal of this guide is to have a one-stop-spot for how to mod TheIndieStone's [Project Zomboid](https://projectzomboid.com), covering all aspects of modding, tools required, tips and tricks, as well as tutorials, examples and useful code snippets.

Much of the current modding information is in outdated tutorials, scattered in forum posts, etc. Ideally by hosting a universal guide on Github, it can be collected all in one spot and anyone can contribute new information, corrections or updates as PZ's API changes over time.

This guide is not meant to be a tutorial (although will *contain* tutorials), but a reference for both new and experienced modders.

Contributions to this guide are more then welcome.

----------------------------------------
### Overview
"Modding" is a vague term covering multiple areas. Many of these areas sometimes overlap but should be considered separate as they require different skill sets and knowledge.

#### Adding basic items and recipes
This can be done with little (or none) coding knowledge and minimal tools. Item and recipe definitions are inside standard .txt documents, although these txt files must conform to a specific syntax and style.  

#### Code changes, advanced items and recipes
Being able to add on to or edit Zomboid's code is a major bonus, and where the real power of mods comes in. PZ is coded in a mixture of Java (the main engine) and Lua (the moddable components).  
Knowledge of the [Lua programming language](https://www.lua.org/) is HIGHLY advised. You'll save yourself a lot of grief and effort learning Lua before diving into this area. Fortunately Lua is a simple language by design, and relatively easy to learn.  
The internet is scattered with tutorials.

#### 3d Models
For build 40 and below, 3d art for Project Zomboid primarily consists of weapon models and vehicles. At this time all 3d mods fall into one of these 2 types.  Models are in a custom .txt format.
The processes of exporting from your modelling app and importing in PZ is fairly simple, though initial positioning, scaling and rotation can take a bit of tweaking. Minor coding is required to tell Zomboid to load the custom models.

Build 41 is expected to bring additional 3d models such as clothing and static items used in timed actions, and a different model format.

#### Textures


#### Map Making

----------------------------------------
### Required Tools (software)
**_This section should describe the various software tools required for various areas, and provide url links for the more common software used_**

For the most part, you are free to use what ever tools you want. The exception is custom map creation, where you are currently limited to the tools released by TIS.

#### Text Editors:
Used in all areas of modding. [Notepad++](https://notepad-plus-plus.org) is often the preferred editor for windows OS's (it also runs amazingly well on linux with wine)

#### 3d Modelling:
Used for the creation of custom items, vehicles and weapons. [Blender](https://blender.org) is the common choice as its free, and import/export scripts for PZ's model format exist. Any 3d app can be used to make your models, with blender doing the final conversion.

#### Image Editor:
If your doing custom icons, textures, maps or 3d models you'll need one of these. Paint.NET, Photoshop and [GIMP](https://gimp.org) are the most used, but whatever supports the .png format will work.

#### Custom Mapping:


----------------------------------------------------------------------------------
----------------------------------------------------------------------------------
## Mod structure
This section contains a brief description of the file and folder structure that makes up a mod. Your mod may include other folders in addition to these, but these ones are the most common.

#### mod.info
This file can be created/edited with any text editor. It contains details like your mod's ID, name, description, preview image etc.
```
name=My First Mod
poster=poster.png
id=MyFirst
description=Basic example mod
url=https://theindiestone.com/forums/
```

#### media/
The actual content of your mod will be placed in subfolders of the `media` folder

#### media/scripts/
This folder contains text files with item, vehicles and recipe definitions. See the section on Scripts for more information.

#### media/models/
Used for 3d models of weapons, vehicles etc.

#### media/textures/
Icons, 3d model textures and the like go in here, in `.png` format. Pay attention to the naming scheme: Item icons should use the `Item_` prefix.

#### media/sounds/
Sound files go in here, in `.wav` or `.ogg` format.

#### media/lua/
If your mod includes any .lua scripts, you will need this folder as well as at least one of the subfolders listed below. Any .lua files need to go in the subfolders, not directly in this one.

#### media/lua/shared/
Used for lua scripts shared by client and server-side logic. These are the first Lua scripts that get loaded.

#### media/lua/client/
Used for client-side scripts. UI elements, context menus timed actions and the like. These get loaded after any 'shared' lua scripts.

#### media/lua/server/
Used for server-side scripts. Item spawning, core farming, weather and other server-side events. These only get loaded when the game is actually started (loading a save, starting a server, etc).

----------------------------------------------------------------------------------
----------------------------------------------------------------------------------
## The Scripts
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

----------------------------------------------------------------------------------
----------------------------------------------------------------------------------
## The Code
Project Zomboid's code is a 2-language system, using both Java and Lua. The main engine and API features are primarily Java,
with large chunks of the logic in Lua.  Most modding is done to the Lua component, while the Java component is generally considered non-moddable.

_Note: The java can be modified, but requires knowledge of java, decompiling and recompiling the .class files. Mods with Java edits also can not be installed using normal methods, these files need to go directly to Zomboid's install directory. Modifying the Java is beyond the scope of this document._

Unlike the Java, Lua can be edited with the text editor of your choice and requires no other tools. _Those familiar with basic Lua can skip ahead._

Lua is designed to be a simple and lightweight language, without a lot of bells and whistles. It is easy to learn, its syntax is simple and there are a minimal number of built-in functions and modules.  It does include some concepts that can initially throw people coming from other languages.

----------------------------------------
### New To Programming
**_TODO: Include a brief lua tutorial here, and links to more online._**

----------------------------------------
### New To Lua
One major difference between Lua and other languages is the syntax. Unlike C based languages, `{ }` are not used to contain code blocks. Brackets are used for declaring tables. Instead a keyword such as `then`, `else`, `do` etc followed by `end` contain a block.

`;` is not normally used in Lua to specify line endings, although it can be (it will be ignored by the parser). The exception is multiple statements on a single line. `if true then x = 1 end` requires no `;` but `if true then x = 1; y = 2 end` does.

`( )` are not required around conditionals, except for complex ones.

Variables are *global*, unless declared with the `local` keyword. (A odd choice for the language, since one of Lua's core philosophies is to declare with as smallest scope possible)

Lua has very few data types. Strings, Numbers, Tables and Userdata (data and objects from other languages, normally C). Integers and floats are treated the same.

There are no objects, sorted/ordered lists or unsorted/unordered lists. These are all the same thing in Lua: a table.

Tables can operate as a sorted or unsorted list, or a in a object-like structure with inheritance (metatables). They can also operate as all 3, at the same time.

When used as a sorted list, table indexes start at 1, not 0

Both `,` and `;` are acceptable separators for entries in a table.

Using `MyTable.myFunction()` calls a function in a table normally, while using `MyTable:myFunction()` calls it as a object method. It is the same as `MyTable.myFunction(MyTable)`

A example of how strange (and versatile) tables can be:
```lua
-- mixed table
local MyTable = {"a", "b"; "c", nil, d = 5; [6] = 'e', f = function(self) print(self.d) end }

-- as a sorted list
print(MyTable[1]) -- prints "a"
print(MyTable[2]) -- prints "b"
print(MyTable[3]) -- prints "c"
print(MyTable[4]) -- nil
print(MyTable[5]) -- also nil, was never defined.
print(MyTable[6]) -- prints "e"
print(#MyTable) -- prints 3. Stops at first nil

for i, v in ipairs(MyTable) do
    print(i, v) -- prints "1 a", "2 b" and "3 c". Stops at first nil
end

-- as a unsorted list
print(MyTable.d) -- prints 5
print(MyTable['d']) -- prints 5

-- with functions
MyTable:f() -- prints 5
MyTable.f() -- errors, no 'self' argument passed
MyTable.f(MyTable) -- prints 5
MyTable.f({d = 7}) -- prints 7
MyTable['f']() -- errors, no 'self' argument passed
MyTable['f'](MyTable) -- prints 5

```

----------------------------------------
### Zomboid's Lua Component
Zomboid's Lua is not 'pure' Lua, it is modified Kahlua, a Lua interpreter writen entirely in Java. It
lacks the performance of pure Lua, but provides a almost seamless integration of the Java and Lua components. Not all Lua modules are implemented such as `io.*` and `os.*`  

Java classes and functions are 'exported' to Lua providing normal access to them, however things like java reflection will not work. These classes and functions are manually specified for export by the Zomboid developers, thus the Lua does not have full access to Java and is at least partially sandboxed.

One thing to bear in mind is functions called from Lua will often return Java objects. Java Arrays and Lists in particular can catch people off guard, since they are quite different from a Lua table list. For example iterating over a Lua table list:
```lua
local tbl = {'a', 'b', 'c'}
for index, value in ipairs(tbl) do
    print(index, value)
end
-- or we could do this:
for index=1, #tbl do -- use # to get the size of the table
    print(index, tbl[index])
end

-- prints:
-- 1    a
-- 2    b
-- 3    c
```
Now say we have a player object want a list of the known recipes:
```lua
local known = player:getKnownRecipes() -- returns a Java List, not a Lua table.
```
Unlike a Lua table list, our the first entry in the Java List is index 0 (not 1), and `ipairs` will no longer work, nor will `#` to get the size, or `[]` to get the value at a specified index. For this we need to use the methods in Java's List class `:size()` and `:get()`
```lua
for index=0, known:size() -1 do
    print(index, known:get(index))
end
```
The upside of this is Java Lists and Arrays have shortcut methods not available in standard Lua tables, like using `:contains("some string")` to test if a string is in the List. Doing this with a Lua table requires iterating over the table manually checking each index.

----------------------------------------
### The Vanilla Lua
**_TODO: Brief outline of what aspects of the game are controlled by lua, and where these aspects can be found in the files._**

----------------------------------------
### Zomboid's API
**_TODO: Describe major parts of the api, common globals and the event system._**

----------------------------------------
### Decompiling The Java
**_TODO: Brief outline of installing and working with a decompiler._**

To truly understand Zomboid's Java API, you need to peak 'under the hood'. For those unfamiliar with Java, the code is written in .java files (standard text like .lua) then 'compiled' into .class files. Compiling is the process for turning human readable code (text) into machine readable instructions. To read Zomboid's Java we need to 'decompile' it: turn the machine readable instructions back into human readable code.

Its worth mentioning that decompiling is not exact. The output code is going to  be slightly different then the code as it was originally written. That's because usually in a language there are multiple ways of writing the same thing, some are more optimized then others. Part of the compilers job is to optimize the code as it converts it to machine instruction. When you decompile you're working with this modified version of the original.

There are multiple Java decompilers but the most commonly used and easiest to work with is [JD-GUI]()

#### Installing

#### Where to start

----------------------------------------
### Overwriting Vanilla Code
A big advantage with large parts of the game logic in Lua means vanilla code can **replaced**, not just added on to. Meaning core parts of the game logic can be modified. Overwriting vanilla code comes with risks though:

**_The more you replace the more likely it is to break across Zomboid versions, and more likely to be incompatible with other mods_**

A common mistake is blindly copying a vanilla file into a mod with a few minor edits. If that file changes next Zomboid update, you need to check and redo your work. If some other mod also has a modified version of that file, then one mod will overwrite the other. Following a few basic rules will drastically help compatibility and reduce potential breakage:

**Always overwrite as little as possible to achieve your goal. Don't replace a whole file if you can do it by overwriting a single function.**  

Using custom tooltips for items as a example (many mods include custom tooltips), the file that handles these tooltips is `client/ISUI/ISToolTipInv.lua` and the method that draws the tooltip is `ISToolTipInv:render()`.  
So we could just copy that whole file and edit our render function, but that's likely to cause compatibility issues and entirely pointless anyways. `ISToolTipInv` is a *global* lua table, we can simply go:
```lua
function ISToolTipInv:render()
    -- ... some custom code ...
end
```
This can be done from any file, without having to replace the original.

**Store the original function in a local variable. If your overwrite doesn't need to run, call the original.**

To solve the problem that many different types of mods want to overwrite the `:render()` method at the same time (ie: weapon and food mods), always store the functions you overwrite in a local variable, so you can call it if you dont need to run your custom code. A food related mod doesn't need to show custom tooltips for weapons and vice versa.  
Run a check early in your overwrite to see if its valid or if you can use the original callback.
```lua
local original_render = ISToolTipInv.render
function ISToolTipInv:render()
    if not CONDITION then
        original_render(self)
    end
    -- ... some custom code ...
end
```
By calling the original, multiple mods overwriting the same function have higher chances of being compatible and it will be more resilient to minor changes in the original vanilla code.

Obviously This not only applies to tooltips, but all overwrites.


----------------------------------------
### Overwriting Another Mod's Code (3rd Party Patching)
Sometimes it is necessary to overwrite or patch code in another mod. There can be many reasons for needing to do this, such as the mod is broken, or outdated, or incompatible with another mod.

Overwriting another mod's code is essentially the same as overwriting vanilla code, and the same basic rules apply. However there are additional things to consider:

**Lua files from all active mods load in alphabetical order.**

When you overwrite vanilla code load order is not as important as vanilla Lua files load before mod Lua files. But when patching mod's code load order matters. You can't overwrite code that hasn't been loaded yet. One way to solve this problem is by ensuring your file gets loaded after by exploiting the alphabetical name load order.  

If the original mod's file is `shared/OriginalCode.lua`. Naming your patch `shared/NewCode.lua` will cause your file to load first (N before O), but naming it `shared/PatchCode.lua` will load after (O before P).  

*Tip: Check the console.log and pay attention to which files get loaded first*

Another more robust solution is to **_delay overwrite code using events_**. Assume the mod we want to overwrite the function `someRandomFun()` in a mod:
```lua
local original_fun
local function newRandomFun()
    -- do something
end
-- add a even callback that performs the overwrite
Events.OnGameBoot.Add(function()
    -- store the original function. Note we dont store until we're ready to overwrite
    original_fun = someRandomFun
    -- now overwrite
    someRandomFun = newRandomFun
end)
```
Exactly which event to use to inject your overwrites is up to you, and dependant on the nature of the overwrite and *where* the code is. Some events don't trigger serverside, some won't trigger clientside etc.

**Overwriting event callback functions requires removing the callback first.**

If the function you want to replace has already been as a event callback, simply overwriting the function won't help.
You need to remove the callback, then add the new one. If the original mod's file is:
```lua
function someRandomFun()
    -- do something
end
Events.OnPlayerUpdate.Add(someRandomFun)
```
Then we need to make some adjustments to our overwrite:
```lua
local original_fun
local function newRandomFun()
    -- do something
end
-- add a event callback that performs the overwrite
Events.OnGameBoot.Add(function()
    Events.OnPlayerUpdate.Remove(someRandomFun) -- remove the callback
    Events.OnPlayerUpdate.Add(newRandomFun) -- add our new callback

    -- store the original function.
    original_fun = someRandomFun
    -- overwrite is probably redundant at this point, but best done just in case.
    someRandomFun = newRandomFun
end)
```

**NOTE:** Removing a event callback is only possible if you can get a reference to the callback function. Functions declared local, then added to a event **can not** be removed from the event. Nor can anonymous functions such as:
```lua
Events.OnGameBoot.Add(function()
    -- do something
end)
```

----------------------------------------
### Performance Tips
There is usually many ways to produce the same outcome when code is involved, some ways are more efficient then others. Don't assume just because your mod is 'small' or 'doesn't do much' that you can forget about optimizing. It only takes one poorly written line of code to bring a system to a crawl. And its most likely people will run your mod with many other mods, which may or may not be optimized. It all adds up.

This doesn't mean you should overly worry about optimization for each part of the code. Trying to tweak each line or code block is a massive waste of time and effort. The trick is knowing where to optimize. A good guideline is **_the more often a block of code runs, the more you need to optimize it._**

Code that only runs once is not so much of a concern as code that is getting triggered by a `OnPlayerUpdate` event.

Even without specific performance optimization some practices should be implemented throughout your mod to help it be less of a drag on the user's machine.

----------------------------------------------------------------------------------

#### Global vs Local
Global variables and functions can be accessed from any file, anywhere in the code. Local variables can only be accessed from the file or block of code they were declared in. While being able to access from anywhere is a handy feature, it's often more problematic. Accessing a global is slower, they run the risk of being accidentally overwritten. One of Lua's core philosophies is to declare variables and functions in the smallest scope possible (local).

**Prefer local variables and functions**

One way to think of the differences between global and local is like a phone book. Global is a entire city and local is your personal phone book, only the numbers you actually plan on calling. Its much faster to find or change things in the smaller personal book then the larger directory. By being local, you also don't have to worry about anyone else adding or changing the same name and overwriting the data.

The downside is these locals can't be accessed from outside the code block or file they were created in.

Lua was designed to have minimal items in the global namespace for performance reasons. Project Zomboid's global namespace is heavily polluted (ie: has too many items). The more items that are in the global space, the longer it takes to access any specific one. Performance differences in Zomboid's kahlua interpreter vs standard Lua likely exist on the global vs local aspect, however benchmarks have shown much faster access to locals.

**Remember the local keyword**

Don't forget that functions and variables are global unless declared with the `local` keyword.
```lua
function Fun() -- global function
    x = 5 -- global variable
    return x
end
Fun2 = function() -- global function
    local x = 5 -- local variable
    return x
end

local function Fun() -- local function
    x = 5 -- global variable
    return x
end
local Fun2 = function() -- local function
    local x = 5 -- local variable
    return x
end
```

**Variables declared inside functions should always be local**

If your function declares a variable, make it local. If you need the variable after the function runs and simply returning it is not a option, declare it local earlier in the file outside the function block.

```lua
local x
local function Fun()
    x = 5
    return "something else"
end
```

**If you need to add to the global space, only add one table**

Often local won't do. You need to access multiple functions and variables from other files. There is a simple effective compromise: *store all your functions and variables into a single global table*

Tables are designed to organize your data and functions. Use them. Storing everything you need in a single table will save from polluting the global namespace more (thus slowing down everything) and still give you complete access. Consider the following table structure:

```lua
MyMod = {
    Settings = { }, -- configuration options
    Client = { -- client side functions and data
        Events = { }, -- client event hooks
    },
    Server = { -- server side functions and data
        Events = { }, -- server event hooks
    },
}
```

If you load this table first before the rest of the mod in the shared folder, you can just add everything inside it. You'll probably want to pull the subtables into local. It gives faster access and cleaner code, especially if you need to access the subtables a lot.

```lua
-- loaded from the client folder
local Client = MyMod.Client
function Client.clientFun() return true end
```

**Pull global tables and functions into local**

Part of Lua's *smallest scope* philosophy applies not only to declaring variables and functions, but accessing them as well. You gain performance by pulling global into the local space as it now has a shortcut then trying to find it in the global namespace every time.
```lua
-- table we declared global in another file
local MyMod = MyMod
```
This applies to everything, including Zomboid's Lua functions and tables, functions and classes in the Java API, and built-in Lua functions and modules.

```lua
local table = table -- Lua's table module
local ipairs = ipairs -- built in Lua function
local ZombRand = ZombRand -- java function
```

Consider the following:
```lua
local MyTable = { }
function MyTable.Fun(table_list) -- table_list is a list of numbers: ie {4, 1, 12}
    for _, num in ipairs(table_list)
        print(ZombRand(num))
    end
end
```
Everytime the function runs, it needs to lookup `ipairs`, and for every number in the table is has to lookup `print` and `ZombRand`. If the table is large, we can gain a performance boost by doing this:
```lua
local MyTable = { }
function MyTable.Fun(table_list) -- table_list is a list of numbers: ie {4, 1, 12}
    -- pull into local
    local print = print
    local ZombRand = ZombRand
    -- no need to pull ipairs here
    -- its only called once and we're stuck looking it up anyways
    for _, num in ipairs(table_list)
        print(ZombRand(num))
    end
end
```

Now it only needs to lookup `print` and `ZombRand` once when the function is called instead of every step of the loop. But this doesn't particularly help and might actually reduce performance with very small tables. Its also still having to do lookups when the function is run. A better option would be:
```lua
local MyTable = { }
local print = print
local ZombRand = ZombRand
local ipairs = ipairs
function MyTable.Fun(table_list) -- table_list is a list of numbers: ie {4, 1, 12}
    for _, num in ipairs(table_list)
        print(ZombRand(num))
    end
end
```

Now when our function runs it doesn't need to look anything up in the global space at all.

If you really want to control local namespace pollution as well and want declare things local only in parts of the file that actually need them, use the `do .. end` block

```lua
local MyTable = { }
local ipairs = ipairs
do -- add a block to limit access
    local ZombRand = ZombRand
    local print = print

    -- since the function is going in MyTable, its access doesn't change
    function MyTable.Fun(table_list) -- table_list is a list of numbers: ie {4, 1, 12}
        for _, num in ipairs(table_list)
            print(ZombRand(num))
        end
    end
end
-- functions and code after the do block have access to the local ipairs
-- but not ZombRand or print
```

----------------------------------------------------------------------------------

#### Event Callbacks

----------------------------------------------------------------------------------

#### Misc Performace Tips

**Pay attention to conditional order**

A big part of optimizing is running as few code instructions as possible. Consider the following conditional statement which includes 2 comparison instructions:
```lua
if x and y then
    -- do something
end
```

This first checks if `x` is true (not false or nil), then checks `y`. If both `x` and `y` have 50/50% chances of both being true or false, then order doesn't matter. However if odds are different then we can tweak this for performance. Lets assume the following for this example:
```
x has a 70% chance of being true, 30% false
y has a 20% chance of being true, 80% false
```

Now we want a `and` conditional checking that both of these are true. With `x` first, there is a 70% (true) chance we end up calling both conditional instructions `x and y`
```lua
if x and y then -- bad option, 70% chance of triggering both checks
    -- do something
end
```

If we reverse the order and check `y` first, there is only a 20% (true) chance it will continue on to check `x`. The second part of the conditional is never executed if the first is invalid.
```lua
if y and x then -- better option, only 20% we check both
    -- do something
end
```

When using `and` put the most likely false first. When using `or` this is reversed, and you need to put the most likely true first
```lua
if x or y then -- if x is true (70% chance) then it never checks y
    -- do something
end
```

**Break from loop structures early**

Remember you can use `break` to exit loops once you achieve your goals.
```lua
local MyTable = {'a', 'b', 'c'}
local index = nil
for i, value in ipairs(MyTable) do
    if value == 'b' then
        index = i
        break -- no need to continue our loop
    end
end
```

Often you don't want to break from a loop, but skip over to the next part. Sadly Lua offers no such feature. We can however fake it using a `repeat ... until true` loop inside our main loop
```lua
local MyTable = {'a', 'b', 'c'}
local index = nil
for i, value in ipairs(MyTable) do
    repeat
        if value ~= 'b' then break end -- if its not b, we break out of the inner repeat loop
        index = i
    until true
end
```

Here we use `break` to exit the `repeat ... until` loop and continue onto the next step of the `ipairs` loop. And since `true` is always true, our `repeat ... until true` loop exits on the first pass instead of actually looping.

**Avoid calling object methods repetitively**

Far too often you see bits of code like this:
```lua
if getPlayer():getInventory():contains("MyMod.MyItem") and getPlayer():getInventory():contains("MyMod.MyItem") then
```

If your going to use the return value of a function multiple times, cache the results in a local variable.
```lua
local inventory = getPlayer():getInventory()
if inventory:contains("MyMod.MyItem") and inventory:contains("MyMod.MyItem") then
```

Its cleaner and saves jumping back and forth from the Java to Lua. Our first example calls the Java component 6 times. The `getPlayer` function, and the `getInventory` and `contains` methods all get called from lua twice (6 total). The second example only calls Java 4 times, a 33% difference and this is only with 2 checks. If we wanted to check for a 3rd item, our example differences would be 9 calls vs 5, four inventory item checks bring the difference to 12 and 6 (50% less calls to Java).

----------------------------------------
### Code Quality Tips
No matter how humble your code is, eventually someone is going to read it. It takes very little extra effort to write clean, readable code. Doing so will help not only other people reading and learning from your work, but also make your work more maintainable in the long run.

**Document your code**

Having documentation or comments in the code explaining is use or intended purpose is a massive help for people trying to understand it. Not only is it a help for other people, but helps you as well if the project is large or you spend a significant time away from it.

The amount of documentation is dependant on the complexity of the code (simple bits are often self-explanatory), but don't assume that just because a function's purpose is obvious to you, its going to be obvious to others as well. For large projects you can even use programs such as [LDoc](https://github.com/stevedonovan/LDoc) or [LuaDoc](https://keplerproject.github.io/luadoc) can auto-generate HTML documentation from comments in the code although this requires using a specific syntax for your comments. (ORGM uses LDoc to generate its HTML docs)

**Be consistent in style**

Lua allows for many different styles of writing: using `;` line endings or not, `( )` around conditionals or not, etc. Its up to decide which style you prefer to work in, but pick one and stick with it.  Don't mix `;` line endings and no endings styles.

**Be consistent in names.**

Use a consistent naming scheme, such as tables capitalized `MyTable`, functions mixed (or camelCased) `myFunction`, and scalar variables lower cased `my_variable`. Again its up to you, but be consistent. It makes it easier to see the intent and use of a variable at a quick glance will show if its a table, function or something else. Some developers go the extra mile with variables and add a prefix to the name to identify scope and data types: `s_variable` for strings, `i_variable` for integers, `g_variable` for globals, etc.

**Be consistent in indentation.**

Use proper indentation levels, for both lua and the scripts .txt files. Stuff like the example below is extremely difficult to read, and FAR too common.
```
module Base {
    item MyItem {
        Type = Normal,
            DisplayName = "My Item",
    }
        item MyItem2 {
                    Type = Normal,
                    DisplayName = "My Item",
    }
}
```
**Know your editor.**

What happens when you hit the `tab` key? Does it insert a tab character? or spaces? Some developers prefer spaces over tabs, the choice is yours. Just don't mix them. Again, **be consistent**. The indentation on a mixed tab/spaces file may look fine on your screen, but with a different editor or fonts it wont.

Most editors will have a option for using tabs or multiple spaces (often 4) when the tab key is pressed. They also have a option for viewing 'whitespaces' in the file, visibly showing where all the tabs and spaces are. When copy/pasting code blocks and snippets from other sources (like the vanilla files), be aware it might not match your tab/spaces preference. *Cleanup any copied code.*  
Fortunately most editors also have a handy tool for auto-converting all tabs to spaces and vice versa.

----------------------------------------
### Code Snippets
**_TODO: Commonly used bits._**


----------------------------------------------------------------------------------
----------------------------------------------------------------------------------

## Translations
**_TODO: Describe the process of creating and supporting mod translations._**


----------------------------------------------------------------------------------

## The Maps
**_TODO: Describe the tools and process for creating custom maps._**


----------------------------------------------------------------------------------
----------------------------------------------------------------------------------

## The Models
**_TODO: Describe process for loading importing and loading custom 3d models in game._**
