## Contents
* [Back to Main](../../..)

  * [Overview](#overview)  
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
    * [Troublesome Event Callbacks]()
    * [Misc Performance Tips]()
  * [Code Quality Tips](#code-quality-tips)  
  * [Code Snippets](#code-snippets)  

  ----------------------------------------
## Overview
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
local MyTable = {"a", "b"; "c", nil, d = 5; [6] = 'e', f = function(self) print(self.d) end, "The End" }

-- as a sorted list
print(MyTable[1]) -- prints "a"
print(MyTable[2]) -- prints "b"
print(MyTable[3]) -- prints "c"
print(MyTable[4]) -- nil
print(MyTable[5]) -- prints "The End"
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

The Lua controls many aspects of the game. A small sample of whats in the Lua, and thus directly moddable:

* User interface elements
* inventory management and item equipping
* timed actions and general interaction with the world
* item spawning
* building
* farming
* vehicle usage and events that happen over time while driving

This section of the guide covers where different aspects can be found in the files. Zomboid's Lua files are broken up into 3 main folders:

* `media/lua/shared`
Used for lua scripts shared by client and server-side logic. These are the first Lua scripts that get loaded.
* `media/lua/client`
Used for client-side scripts. UI elements, context menus timed actions and the like. These get loaded after any 'shared' Lua scripts.
* `media/lua/server`
Used for server-side scripts. Item spawning, core farming, weather and other server-side events. These only get loaded when the game is actually started (loading a save, starting a server, etc).

Many aspects such as farming or vehicles have both a `client` and `server` component.

#### Professions and Traits
`shared/NPCs/` defines all professions and traits, also hair color pallets and survivor random names and other character creation options.

#### Custom Challanges
`client/LastStand/`

#### User Interface
_**Note:**_ *many interface elements such as the mechanics window are listed in their corresponding sections instead of here. This section contains standalone UI elements.*

`client/OptionScreens/` contains the pre-game interface. Main menu, options and mod selection, plus the game setup and character creation screens.

`client/XpSystem/ISUI/` character skills window, health and info panels.

`client/SurvivorGuide/` tutorial/help window (F1 key)

`client/ISUI/` contains base UI classes such as buttons or richtext boxes, as well as many specific windows and elements like tooltips and inventories.

#### TimedActions
`client/TimedActions/` contains the majority of timed actions, although many such as farming or reloading are not part of this folder, and listed in their corresponding sections.

#### Building
`client/BuildingObjects/ISUI/`

`client/BuildingObjects/TimedActions/`

`server/BuildingObjects/`

#### Farming
`client/Farming/ISUI/`

`client/Farming/TimedActions/`

`server/Farming/`

#### Firearm Logic
`shared/Reloading/` Contains reloading, racking and firing logic. Expect major differences between PZ build 40 and 41 here.

#### Hidden Stashes
`shared/StashDescriptions/`

#### Settings
`shared/defines.lua` the ZomboidGlobals modifiers

`shared/Sandbox/` predefined sandbox presets

#### Vehicles
`shared/VehicleZoneDefinition.lua` defines where specific vehicle types can be found and spawn chances.

`client/Vehicles/ISUI/` user interface elements such as the dashbar, radial and mechanics window.

`client/Vehicles/TimedActions/` various timed actions such as opening windows, adding gas, hotwiring etc.

`server/Vehicles/VehiclesCommands.lua` contains server callbacks in response to client actions sent with sendClientCommand().

`server/Vehicles/VehiclesDistributions.lua` item distributions inside vehicle containers are defined here.

`server/Vehicles/Vehicles.lua` contains callback functions in response to various vehicle events such as creation or update, mechanics callbacks on part installation or removal, and utility functions. Part condition on vehicle spawn, loss of condition while driving, fuel consumption and battery usage are a example of what can be modified here.

#### Reload Modified Files

You can reload modified files to reflect latest chanes within a live game session, without either restarting or reloading the world. In a debug-enabled game, press F11 and then use the bottom-right "Lua Files" window to find your modified script. Click "RELOAD" which appears when hovening over the script's name.

----------------------------------------
### Zomboid's API
**_TODO: Describe major parts of the api, common globals and the event system._**

The API (application programming interface) for Project Zomboid consists of 2 parts: the Lua global functions and tables, and the Java exposed functions and [classes](./JavaClasses.md).  

Given that Project Zomboid is far from complete, its API is constantly evolving. Official documentation is outdated and sparse. The most effective way to find what's available to you is by reading the code. For Lua this is simple enough but for Java it means [decompiling](#decompiling-the-java).

#### The Event System
The majority of Zomboid's Lua code is triggered by a Event/Callback system. Lua functions are registered as callbacks in response to events triggered by Java. The exception is code run when the Lua files are loaded (code outside functions), and Lua functions specifically called by Java (applies only to some vanilla functions).  

There is a wide variety of events that you can register a callback to including stuff like key presses, equipping a item, loot spawning, starting a game etc.

Registering callbacks to a event is easy:
```lua
local function someFun(player)
    -- do something
end
Events.OnPlayerUpdate.Add(someFun)
```

Removing callbacks is equally as easy, but you can only remove a event if you have a pointer to the callback function:
```lua
Events.OnPlayerUpdate.Remove(someFun)
```

A common mistake with beginners adding events is accidentally doing this:
```lua
Events.OnPlayerUpdate.Add(someFun()) -- don't include () after your function or you'll call it instead of passing it
```

Events are not always triggered by Java, in some instances they are triggered by Lua. A example is the ItemPicker in build 40 and below: Java directly calls the Lua ItemPicker functions, which then triggers the `OnFillContainer` event.
```lua
-- trigger a event from Lua
triggerEvent(event_type, arg1, arg2, arg3)
```

New event types can also be added by Lua:
```lua
LuaEventManager.AddEvent("SomeEvent") -- add our new event type
Events.SomeEvent.Add(someFun) -- add our callback to our new event
-- now we can use triggerEvent("SomeEvent", args)
```

When creating callbacks to events, there are a few things that need to be taken into consideration:

* Some events are triggered more often then others, and callbacks may need to be optimized for performance.
* Some events are only triggered client-side or server-side, thus callbacks need to be as well.

Different events will pass different argument types to your callbacks. When using a new event its worth debugging it first, checking if its only client or server-side, how often its called, and exactly what types of arguments it has. Using the function in the following example we can easily see what arguments a event callback is getting.

```lua
local function printEvent(event, ...)
    print("--------------------------------------")
    local d = {select(1,...)} -- convert the ... argument to a table
    for i, v in ipairs(d) do d[i] = tostring(v) end -- convert each to a string representation
    print("-- "..event .. "("..table.concat(d, ", ")..")")
    print("--")
    print("--")
end
-- add to some events, note the use of the wrapper function around printEvent
Events.OnNewGame.Add(function(...) printEvent("OnNewGame", ...) end)
Events.OnEquipPrimary.Add(function(...) printEvent("OnEquipPrimary", ...) end)
Events.OnEquipSecondary.Add(function(...) printEvent("OnEquipSecondary", ...) end)
```

Which will print something similar to this in the console:
```
1521394450391 --------------------------------------
1521394450391 -- OnNewGame(zombie.characters.IsoPlayer@1c5b2c8, zombie.iso.IsoGridSquare@10b1392)
1521394450391 --
1521394450391 --
```

#### The Hook System


----------------------------------------
### Decompiling The Java
**_TODO: Brief outline of installing and working with a decompiler._**

To truly understand Zomboid's Java API, you need to peak 'under the hood'. For those unfamiliar with Java, the code is written in .java files (standard text like .lua) then 'compiled' into .class files. Compiling is the process for turning human readable code (text) into machine readable instructions. To read Zomboid's Java we need to 'decompile' it: turn the machine readable instructions back into human readable code.

Its worth mentioning that decompiling is not exact. The output code is going to  be slightly different then the code as it was originally written. That's because usually in a language there are multiple ways of writing the same thing, some are more optimized then others. Part of the compilers job is to optimize the code as it converts it to machine instruction. When you decompile you're working with this modified version of the original.

There are multiple Java decompilers but the most commonly used and easiest to work with is [JD-GUI](https://github.com/java-decompiler/jd-gui).

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

#### Troublesome Event Callbacks

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

**Break from functions and loop structures as early as possible**

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
