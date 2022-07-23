## Contents
* [Back to Main](../../..)

  * [Overview](#overview)
  * [Folder Structure](#folder-structure) 
  * [Functionalities](#functionalities)
    * [Folders](#folders)
  * [Be Careful](#be-careful)
  * [Testing Translations Locally](#testing-translations-locally)

----------------------------------------
## Overview
In this topic, we will discuss the right layout of files and folders, running translations locally, and explanations into the txt files and their names.

## Folder Structure
This folder contains various translations in the game, each with its own txt file.
```
├── [ Translation Name ] Example: EN
    ├── Muldraugh, KY
        ├── description.txt 
        ├── title.txt   
    ├── Riverside, KY
        ├── description.txt
        ├── title.txt    
    ├── Rosewood, KY
        ├── description.txt
        ├── title.txt        
    ├── West Point, KY
        ├── description.txt
        ├── title.txt
    ├── Challenge_[ Translation Name ].txt
    ├── ContextMenu_[ Translation Name ].txt
    ├── DynamicRadio_[ Translation Name ].txt
    ├── EvolvedRecipeName_[ Translation Name ].txt
    ├── ContextMenu_[ Translation Name ].txt
    ├── Farming_[ Translation Name ].txt
    ├── GameSound_[ Translation Name ].txt
    ├── IG_UI_[ Translation Name ].txt
    ├── ItemName_[ Translation Name ].txt
    ├── MakeUp_[ Translation Name ].txt
    ├── Moodles_[ Translation Name ].txt
    ├── Moveables_[ Translation Name ].txt
    ├── MultiStageBuild_[ Translation Name ].txt
    ├── Recipes_[ Translation Name ].txt
    ├── Sandbox_[ Translation Name ].txt
    ├── Stash_[ Translation Name ].txt
    ├── SurvivalGuide_[ Translation Name ].txt
    ├── Tooltip_[ Translation Name ].txt
    ├── UI_[ Translation Name ].txt
    ├── language_[ Translation Name ].txt
    ├── credits.txt
```

## Functionalities
- #### Folders 
This is your spawning point of the game, and inside of that folders contains:
```
├── Muldraugh, KY
        ├── description.txt 
        ├── title.txt  
```
```description.txt``` This is the Map's description; you may translate it, but be cautious not to modify [HTML ELEMENTS](https://www.w3schools.com/html/html_elements.asp)

```title.txt``` This is the title of the map.

- ```Challenge_[ Translation Name ].txt```

This is the list of modes available in Project Zomboid, and each mode includes translations.

- ```ContextMenu_[ Translation Name ].txt```

This is a translation for the context menu in the game.

- ```DynamicRadio_[ Translation Name ].txt```

This is a translation of random occurrence for wheater, power outage, and survivor communication.

- ```EvolvedRecipeName_[ Translation Name ].txt```

This is a translation of the project zomboid's translation about opening cans and making a tea.

- ```Farming_[ Translation Name ].txt```

This is a translation of every item in the farming section.

- ```GameSound_[ Translation Name ].txt```

This file contains a translation for each sound; some may appear in the menu but not in this file.

- ```IG_UI_[ Translation Name ].txt```

This file contains a translation of all the game's UI text.

- ```ItemName_[ Translation Name ].txt```

This file contains a translation of all of the game's item names.

- ```MakeUp_[ Translation Name ].txt```

This file contains a translation of all types of make up.

- ```Moodles_[ Translation Name ].txt```

This file contains a translation of all of your character's moods, as seen in the game's side screen.

- ```Moveables_[ Translation Name ].txt```

This file contains a translation of crafting methods like in (carpentry); right-click.

- ```MultiStageBuild_[ Translation Name ].txt```

This file contains a translation of crafting walls and upgrading them.

- ```Recipes_[ Translation Name ].txt```

This file contains a translation of all recipes to craft an item.

- ```Sandbox_[ Translation Name ].txt```

This file contains a translation creating a custom sandbox in the game.

- ```Stash_[ Translation Name ].txt```

This file contains a translation of an random letter/maps from a survivors corpse/car/house

- ```SurvivalGuide_[ Translation Name ].txt```

This file contains a translation of the introduction to survival guidelines, crafting, water, vehicles, foraging, and so on.

- ```Tooltip_[ Translation Name ].txt```

This file contains a translation of the items functionalities (microwaves,firearms,TV's,clothing... etc)

- ```UI_[ Translation Name ].txt```

This file contains a translation of the game's UI (multiplayer mode)

- ```Language.txt```

This file contains the language, what version and the encoding

- ```credits.txt```

This file contains credits the to authors.

## Be Careful
Writing and modifiying the indiviual txt files you should replace the ```[ Translation Name ]``` into two words such as: EN ( English ), PH ( Philippines )

And also modify inside in file Example:
It should change the "'PH"' to a language associated with the file name _[Translation Name].txt.
```
Challenge_PH = { 
..................
..................
..................
}
```

Make sure that you will not modify [HTML ELEMENTS](https://www.w3schools.com/html/html_elements.asp)

## Testing Translations Locally

To test the translations (or modified English texts) in your game, place the files from this repository in `…\projectzomboid\media\lua\shared\Translate`

You can back up the original translation first and then replace the folder or make a copy.
