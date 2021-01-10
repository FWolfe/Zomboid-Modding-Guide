### Creating custom Clothing

### Creating the 3d model
[//]: # "NEED HELP ON THIS ONE" 

### Creating the clothing item

1. The first part is to select your model (This is assuming that your model works in game) - You will test this by accomplishing these next steps
2. The next part is to make an addition to the script.txt file and import it into base. 

For example, in the mod directory 
'<\AuthenticZ\Contents\mods\Authentic Z + Custom Zombies\media\scripts\clothing>'
in the script of PZAZ_clothing_jacket.txt : 

```txt
module AuthenticZClothing {
  imports {
      Base
  }
    item Jacket_Grimes             /** ClothingItem name **/
    {
        Type = Clothing,
        DisplayName = Rick's Jacket, /** This is the name that appears in the inventory menu in-game. **/
        ClothingItem = Jacket_Grimes, /** This is the name of the XML clothing item that you need to refer to correctly. 
        BodyLocation = Jacket,        /** This is connected with Bodylocations.lua which controls the restrictions on what combinations of clothing that your character can wear. **/
        Icon = JacketRick,            /** Inventory icon for the jacket. The sprite is named item_JacketRick in the textures folder in the mod directory. **/
        BloodLocation = Jacket,
        RunSpeedModifier = 0.93,
        CombatSpeedModifier = 0.98,
        BiteDefense = 20,
        ScratchDefense = 30,
        NeckProtectionModifier = 0.5,
        Insulation = 0.7,
        WindResistance = 0.7,
        FabricType = Cotton,
        WaterResistance = 0.5,
        Weight = 2,
    }
}    
```

2. Connect the script with a new .xml clothing item (you refer its name id in its script file)


'<\Authentic Z + Custom Zombies\media\clothing\clothingItems>' -> Jacket_Grimes.xml : 
```xml
<?xml version="1.0" encoding="utf-8"?>
<clothingItem>
	<m_MaleModel>skinned\clothes\bob_jacket</m_MaleModel>     <!-- The 3d models with animations -->
	<m_FemaleModel>skinned\clothes\kate_jacket</m_FemaleModel> <!-- The 3d models with animations -->
	<m_GUID>6c247231-dbe5-45cd-8e93-78d4ef716ff9</m_GUID> <!-- A unique GUID for the clothingitem. See Step 3. -->
	<m_Static>false</m_Static>
	<m_AllowRandomHue>false</m_AllowRandomHue>    <!-- No Hue -->
	<m_AllowRandomTint>false</m_AllowRandomTint>  <!-- No Tint  -->
	<m_AttachBone></m_AttachBone>
	<m_Masks>13</m_Masks>
	<m_Masks>3</m_Masks>
	<m_Masks>5</m_Masks>
	<m_UnderlayMasksFolder>media/textures/Clothes/Jacket/Masks</m_UnderlayMasksFolder>   <!-- Masks that the clothingitem uses -->
	<textureChoices>clothes\jacket\jacket_grimes</textureChoices>                        <!-- The texture that the model uses -->
</clothingItem>
```
3. Get a unique GUID. (Globally Unique Identifier)
Go to [GUID Generator](https://www.guidgenerator.com/online-guid-generator.aspx) , and copy and paste in a new GUID into its respected section. All clothingitems, models, and makeup get
their own GUID. You will use this ID code in every .XML file that you want to use this clothingitem in. 

4. Then you also add an addition to the GUID table. 
```(Authentic Z + Custom Zombies\media -> fileGuidTable.xml)
	<!-- Authentic Z Custom Clothing -->     /** This is how you comment in a .XML file btw! **/
	<files>
		<path>media/clothing/clothingItems/Jacket_Grimes.xml</path>
		<guid>6c247231-dbe5-45cd-8e93-78d4ef716ff9</guid>
	</files>
```
---Your item should now be able to spawn in game, via debug. Of course you got to add distributions to it if you want it to appear in the world.

### Possible Issues that you may run across.

1. The item will not appear, this is probably from you not referencing the clothingitem name correctly in the script. 

2. The clothing item will appear with a red and white checkerboard format - that comes from not referencing the texture file correctly.

3. The item will appear in inventory but appears invisible on character - this is probably because it did not get a new GUID table addition. 
And then of course, you got to give that clothing item an inventory icon, which you reference in the /media/textures file with item_YourFile.png.


[//]: # "Extra"
### HOW TO MAKE CUSTOM CLOTHING APPEAR ON ZOMBIES

Since you have gotten this far, the bulk of the tedious work is done. 

1. Go to clothing.xml , located in media\clothing and take a look at the format of the outfits. 
These are outfits that the game designates onto zombies. Making a new one correctly will add it to the game's list of zombie outfits. 
```xml
<m_MaleOutfits>
		<m_Name>AuthenticRickGrimes</m_Name>                                <!-- The Name of the zombie outfit, it will appear like this in the debug list of Zombie manager -->
		<m_Guid>1fc501ec-3d70-4a9d-828a-77a23dbcffd9</m_Guid>               <!-- The outfit needs a new GUID. Get a new GUID here: [GUID Generator](https://www.guidgenerator.com/online-guid-generator.aspx) -->
		<m_Top>false</m_Top>
		<m_Pants>false</m_Pants>
		<m_AllowPantsHue>false</m_AllowPantsHue>                            <!-- No pants hue change to the textures -->
		<m_AllowTopTint>false</m_AllowTopTint>                              <!-- No top tint to the textures -->
		<m_AllowTShirtDecal>false</m_AllowTShirtDecal>                      <!-- No decal allowed on shirt -->
		<m_items> <!-- Pants -->                                          <!-- This is how you comment in a .XML file btw! -->
			<itemGUID>92d60950-28a8-42cb-bee6-0384069f250d</itemGUID>
		</m_items>
		<m_items> <!-- Hat -->
			<itemGUID>7445da27-d930-4290-af26-515a04c96f8f</itemGUID>
		</m_items>
		<m_items> <!-- Jacket -->                                      <!-- What we made above, we put here, only put the GUID refering to it -->
			<itemGUID>6c247231-dbe5-45cd-8e93-78d4ef716ff9</itemGUID>        
		</m_items>
		<m_items> <!-- T- shirt -->
			<itemGUID>993a89f1-7dde-406d-ba1f-3d02d14db5cc</itemGUID>       <!-- a 50/50 chance of it choosing between these two T-shirt clothing items -->
			<subItems>
				<itemGUID>1a355b1d-3b97-4a11-a244-2b80aad8c009</itemGUID>
			</subItems>
		</m_items>
		<m_items> <!-- Shoes -->
			<itemGUID>593bea3d-c127-4574-ac1e-2197dc56e486</itemGUID>
		</m_items>
		<m_items> <!-- Holster -->                                        <!-- Pistols do not spawn in here yet, you set that in "AttachedWeaponDefinitions.lua" -->
			<itemGUID>88f11c9e-e717-446c-82ec-f749320df25a</itemGUID>
		</m_items>
		<m_items> <!-- Rings -->
			<probability>0.3</probability>                                  <!-- When probability is thrown in, it gives it a chance of it not spawning at all. Here its a .3 of any -->
			<itemGUID>4cbb081e-0e9a-4248-bec1-0aa226e082e1</itemGUID>       <!-- Piece of jewelry to spawn. **/
			<subItems>
				<itemGUID>e4b3135f-a541-4870-a7b9-2ba6f730bc5c</itemGUID>
			</subItems>
		</m_items>
		<m_items> <!-- Classic Watches -->
			<itemGUID>5fc22466-2942-4ba1-8c33-2a8528b73021</itemGUID>
			<subItems>
				<itemGUID>ce1a8520-7120-432e-8a3a-aa7b3094f0bb</itemGUID>         <!-- BTW copy and paste this GUID into grepWin search and you find out what this is. -->
			</subItems>
		</m_items>
	</m_MaleOutfits>
```
2. Do note that this would create a "Male only" zombie outfit. Replacing the top and bottom cases in a new outfit with a "<m_FemaleOutfits>" & "</m_FemaleOutfits>" will make 
a female variation. Reuse that GUID from the make for the female to have the game recognize both sex of outfits - this pairs them. (It won't be exclusive to a sex)

3. If you set to spawn a zombie in "ZombieZoneDefinitions.lua" and it is a sex exclusive outfit (Male or Female only),
You got to state it as such or else the game will try to spawn the other sex, realize that it doesn't exist, then choose to spawn a random outfit from its clothing.xml list.
For Example :
```lua
ZombiesZoneDefinition.Police = {
	AuthenticRickGrimes = {        /** Term for the ZombieZoneDefinitions.lua file **/
		name="AuthenticRickGrimes",  /** Name of the Outfit in Clothing.xml **/
		chance=3,                    /** Small chance of 3% to spawn in the zone (police station) **/
		gender="male",               /**  This is a male only zombie, state it as such.   **/
	},
  }
```
4. There are two other files that you can use to customize your zombie outfits - HairOutfitDefinitions.lua & AttachedWeaponDefinitions.lua
Which you place later in this folder of the mod : 
\Authentic Z + Custom Zombies\media\lua\shared\Definitions

In HairOutfitDefinitions.lua, you can designate one specific haircut or a range of haircuts for that zombie outfit. 
In AttachedWeaponDefinitions.lua, you can choose what items will spawn attached to the zombie for that outfit.  


