# Project Zomboid Modding Guide

**_This document is very much a Work-In-Progress._**

**_The goal is to create a solid 1-stop spot for how to mod TheIndieStone's Project Zomboid, covering all aspects of modding, tools required, tips and tricks, as well as examples and code snippits for the more common tasks._**

**_Much of the current modding information is in outdated tutorials, scattered in forum posts, and locked away in the brains of modders and other community members. Ideally by hosting a universal guide on Github, it will be collected all in one spot and anyone can contribute new information, corrections or updates as PZ's API changes over time._**

## Intro
**This section should provide a basic description of the various areas of modding, and basic hints as to difficulty and tools required**

"Modding" is a vague term covering multiple areas. Many of these areas sometimes overlap but should be considered separate as they require different skill sets and knowledge.

### Adding basic items and recipes
This can be done with little (or none) coding knowledge and minimal tools. Item and recipe definitions are inside standard .txt documents, although these txt files must conform to a specific syntax and style.

### Code changes, advanced items and recipes
Being able to add on to or edit Zomboid's code is a major bonus, and where the real power of mods comes in. PZ is coded in a mixture of Java (the main engine) and Lua (the moddable components).
Knowledge of the Lua scripting language is HIGHLY advised. You'll save yourself a lot of grief and effort learning Lua before diving into this area. Fortunately Lua is a simple language by design, and relatively easy to learn. The internet is scattered with tutorials.

### 3d Models

### Textures

### Map Making


----------------------------------------------------------------------------------

## Tools (software)
**This section should describe the various software tools required for various areas, and provide url links for the more common software used**

For the most part, you are free to use what ever tools you want. The exception is custom map creation, where you are currently limited to the tools released by TIS.

### Text Editors:
Used in all areas of modding. Notepad++ is often the preferred editor for windows OS's (it also runs amazingly well on linux with wine)

### 3d Modelling:
Used for the creation of custom items, vehicles and weapons. Blender is the common choice as its free, and import/export scripts for PZ's model format exist. Any 3d app can be used to make your models, with blender doing the final conversion.

### Image Editor:
If your doing custom icons, textures, maps or 3d models you'll need one of these. Paint.NET, Photoshop and GIMP are the most used, but whatever supports the .png format will work.

### Custom Mapping:



----------------------------------------------------------------------------------

## Mod structure
**Describe the various files and folders that make up a mod, and their purpose**

mod.info (required file)

media (required folder)

media/scripts (optional folder)

media/models (optional folder)

media/textures (optional folder)

media/sounds (optional folder)

media/lua (optional folder) required for lua scripts

media/lua/client

media/lua/shared

media/lua/server


----------------------------------------------------------------------------------
