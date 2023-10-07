# Project Zomboid Modding Guide
**_This document is very much a Work-In-Progress._**

The goal of this guide is to have a one-stop-spot for how to mod TheIndieStone's [Project Zomboid](https://projectzomboid.com), covering all aspects of modding, tools required, tips and tricks, as well as tutorials, examples and useful code snippets.

Much of the current modding information is in outdated tutorials, scattered in forum posts, etc. Ideally by hosting a universal guide on Github, it can be collected all in one spot and anyone can contribute new information, corrections or updates as PZ's API changes over time.

This guide is not meant to be a tutorial (although will *contain* tutorials), but a reference for both new and experienced modders.

Contributions to this guide are more then welcome.


## Contents
* [Introduction](#Introduction)
  * [Overview](#overview)  
  * [Required Tools](#required-tools-software)
* [Mod Structure](./structure/README.md)
* [The Scripts](./scripts/README.md)  
* [The Code](./api/README.md)  
* [Translations](./translations/README.md)  
* [The Maps](./mapping/README.md)  
* [The Models](./modelling/README.md)  

----------------------------------------------------------------------------------
----------------------------------------------------------------------------------
## Introduction

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

Build 41 uses the [.X](https://en.wikipedia.org/wiki/.x) and [.fbx](https://en.wikipedia.org/wiki/FBX) formats.

#### Textures


#### Map Making

----------------------------------------
### Required Tools (software)
**_This section should describe the various software tools required for various areas, and provide url links for the more common software used_**

For the most part, you are free to use what ever tools you want. The exception is custom map creation, where you are currently limited to the tools released by TIS.

#### Text Editors:
Used in all areas of modding. [Notepad++](https://notepad-plus-plus.org) is often the preferred editor for windows OS's (it also runs amazingly well on linux with wine)

#### 3d Modelling:
Used for the creation of custom items, vehicles and weapons. [Blender](https://blender.org) is the common choice as its free and import/export scripts for PZ's older (build 40) model format exist. 

#### Image Editor:
If your doing custom icons, textures, maps or 3d models you'll need one of these. Paint.NET, Photoshop and [GIMP](https://gimp.org) are the most used, but whatever supports the .png format will work.

#### Custom Mapping:
**_TODO: list tools used in mapping._**

#### Debug Mode
The game comes with a variety of tools to assist in mod development. You can enable these by adding the `-debug` command line option on startup.

In Steam, you can add this under Properties / General / Launch Options.

----------------------------------------------------------------------------------

## Topics
* [Mod Structure](./structure/README.md)
* [The Scripts](./scripts/README.md)  
* [The Code](./api/README.md)  
* [Translations](./translations/README.md)  
* [The Maps](./mapping/README.md)  
* [The Models](./modelling/README.md)  
