## Java classes exposed to Lua (v40.43)
```
BufferedReader.class
java.io.BufferedWriter.class
DataInputStream.class
DataOutputStream.class

Double.class
Long.class
Float.class
Integer.class
Math.class
Void.class

SimpleDateFormat.class

ArrayList.class
Calendar.class
java.util.EnumMap.class
java.util.GregorianCalendar.class
HashMap.class
java.util.LinkedList.class
Stack.class
java.util.Vector.class
java.util.Iterator.class

fmod.fmod.FMODAudio.class
fmod.fmod.FMODSoundBank.class
fmod.fmod.FMODSoundEmitter.class

org.joml.Vector3f.class

se.krka.kahlua.vm.KahluaUtil.class

sun.util.BuddhistCalendar.class

zombie.audio.DummySoundBank.class
zombie.audio.DummySoundEmitter.class
zombie.audio.BaseSoundEmitter.class
zombie.audio.GameSound.class
zombie.audio.GameSoundClip.class

zombie.behaviors.Behavior.class
zombie.behaviors.Behavior.BehaviorResult.class
zombie.behaviors.general.PathFindBehavior.class

zombie.ai.states.AttackState.class
zombie.ai.states.BurntToDeath.class
zombie.ai.states.ClimbDownSheetRopeState.class
zombie.ai.states.ClimbOverFenceState.class
zombie.ai.states.ClimbOverFenceState2.class
zombie.ai.states.ClimbSheetRopeState.class
zombie.ai.states.ClimbThroughWindowState.class
zombie.ai.states.ClimbThroughWindowState2.class
zombie.ai.states.CrawlingZombieTurnState.class
zombie.ai.states.DieState.class
zombie.ai.states.FakeDeadZombieState.class
zombie.ai.states.IdleState.class
zombie.ai.states.JustDieState.class
zombie.ai.states.LuaState.class
zombie.ai.states.LungeState.class
zombie.ai.states.OpenWindowState.class
zombie.ai.states.PathFindState.class
zombie.ai.states.PlayerControlState.class
zombie.ai.states.ReanimatePlayerState.class
zombie.ai.states.ReanimateState.class
zombie.ai.states.SatChairState.class
zombie.ai.states.SatChairStateOut.class
zombie.ai.states.StaggerBackDieState.class
zombie.ai.states.StaggerBackState.class
zombie.ai.states.SwipeState.class
zombie.ai.states.SwipeStatePlayer.class
zombie.ai.states.ThumpState.class
zombie.ai.states.WalkTowardState.class
zombie.ai.states.WanderState.class
zombie.ai.states.ZombieStandState.class

zombie.characters.BodyDamage.BodyPartType.class
zombie.characters.BodyDamage.BodyPart.class
zombie.characters.BodyDamage.BodyDamage.class
GameKeyboard.class
fmod.fmod.EmitterType.class

zombie.characters.CharacterTimedActions.LuaTimedAction.class
zombie.characters.CharacterTimedActions.LuaTimedActionNew.class

zombie.characters.Moodles.Moodle.class
zombie.characters.Moodles.Moodles.class
zombie.characters.Moodles.MoodleType.class

ProfessionFactory.class
ProfessionFactory.Profession.class

PerkFactory.class
PerkFactory.Perk.class
PerkFactory.Perks.class

zombie.characters.traits.ObservationFactory.class
zombie.characters.traits.ObservationFactory.Observation.class

TraitFactory.class
TraitFactory.Trait.class

zombie.characters.IsoDummyCameraCharacter.class
zombie.characters.Stats.class
SurvivorDesc.class
zombie.characters.SurvivorFactory.class
zombie.characters.SurvivorFactory.SurvivorType.class

IsoGameCharacter.class
IsoGameCharacter.PerkInfo.class
IsoGameCharacter.XP.class
IsoPlayer.class
zombie.characters.IsoSurvivor.class
IsoZombie.class

zombie.core.Clipboard.class

zombie.core.fonts.AngelCodeFont.class

zombie.core.logger.ZLogger.class

zombie.core.properties.PropertyContainer.class

zombie.core.textures.ColorInfo.class
Texture.class

SteamFriend.class
SteamUGCDetails.class
SteamWorkshopItem.class

zombie.core.Color.class
zombie.core.Colors.class
Core.class
Language.class
PerformanceSettings.class
SpriteRenderer.class
Translator.class

DebugOptions.class
DebugOptions.BooleanDebugOption.class

zombie.erosion.ErosionConfig.class
zombie.erosion.ErosionConfig.Debug.class
zombie.erosion.ErosionConfig.Season.class
zombie.erosion.ErosionConfig.Seeds.class
zombie.erosion.ErosionConfig.Time.class
ErosionMain.class
zombie.erosion.season.ErosionSeason.class

ChooseGameInfo.Mod.class
ChooseGameInfo.Story.class
zombie.gameStates.GameLoadingState.class
zombie.gameStates.MainScreenState.class

zombie.globalObjects.CGlobalObjects.class
zombie.globalObjects.CGlobalObjectSystem.class
zombie.globalObjects.GlobalObject.class
zombie.globalObjects.SGlobalObjects.class
zombie.globalObjects.SGlobalObjectSystem.class

zombie.input.Keys.class
Mouse.class

zombie.inventory.types.Clothing.class
zombie.inventory.types.ComboItem.class
zombie.inventory.types.Drainable.class
zombie.inventory.types.DrainableComboItem.class
zombie.inventory.types.Food.class
zombie.inventory.types.HandWeapon.class
zombie.inventory.types.InventoryContainer.class
zombie.inventory.types.Key.class
zombie.inventory.types.KeyRing.class
zombie.inventory.types.Literature.class
zombie.inventory.types.AlarmClock.class
zombie.inventory.types.WeaponPart.class
zombie.inventory.types.Moveable.class
zombie.inventory.types.Radio.class

ItemContainer.class
InventoryItem.class
InventoryItemFactory.class
zombie.inventory.FixingManager.class
zombie.inventory.RecipeManager.class

zombie.iso.areas.isoregion.IsoRegion.class
zombie.iso.areas.isoregion.DataCell.class
zombie.iso.areas.isoregion.DataChunk.class
zombie.iso.areas.isoregion.ChunkRegion.class
zombie.iso.areas.isoregion.MasterRegion.class

zombie.iso.areas.IsoBuilding.class
zombie.iso.areas.IsoRoom.class
zombie.iso.areas.SafeHouse.class

zombie.iso.objects.interfaces.BarricadeAble.class

zombie.iso.objects.IsoBarbecue.class
zombie.iso.objects.IsoBarricade.class
zombie.iso.objects.IsoCrate.class
zombie.iso.objects.IsoCurtain.class
zombie.iso.objects.IsoCarBatteryCharger.class
zombie.iso.objects.IsoDeadBody.class
zombie.iso.objects.IsoDoor.class
zombie.iso.objects.IsoFire.class
IsoFireManager.class
zombie.iso.objects.IsoFireplace.class
zombie.iso.objects.IsoGenerator.class
zombie.iso.objects.IsoJukebox.class
zombie.iso.objects.IsoLightSwitch.class
zombie.iso.objects.IsoMolotovCocktail.class
zombie.iso.objects.IsoWaveSignal.class
zombie.iso.objects.IsoRadio.class
zombie.iso.objects.IsoTelevision.class
zombie.iso.objects.IsoStove.class
zombie.iso.objects.IsoThumpable.class
zombie.iso.objects.IsoTrap.class
zombie.iso.objects.IsoTree.class
zombie.iso.objects.IsoWheelieBin.class
zombie.iso.objects.IsoWindow.class
zombie.iso.objects.IsoWindowFrame.class
zombie.iso.objects.IsoWorldInventoryObject.class
zombie.iso.objects.IsoZombieGiblets.class
zombie.iso.objects.RainManager.class
zombie.iso.objects.ObjectRenderEffects.class

IsoSprite.class
zombie.iso.sprite.IsoSpriteInstance.class
zombie.iso.sprite.IsoSpriteManager.class
zombie.iso.sprite.IsoSpriteGrid.class

zombie.iso.SpriteDetails.IsoFlagType.class
zombie.iso.SpriteDetails.IsoObjectType.class

ClimateManager.class
ClimateManager.DayInfo.class
ClimateManager.ClimateFloat.class
ClimateManager.ClimateColor.class
ClimateManager.ClimateBool.class
zombie.iso.weather.WeatherPeriod.class
zombie.iso.weather.WeatherPeriod.WeatherStage.class
zombie.iso.weather.WeatherPeriod.StrLerpVal.class
ClimateManager.AirFront.class
zombie.iso.weather.ThunderStorm.class
zombie.iso.weather.ThunderStorm.ThunderCloud.class
zombie.iso.weather.fx.IsoWeatherFX.class
zombie.iso.weather.Temperature.class
zombie.iso.weather.Temperature.PlayerTempVars.class
zombie.iso.weather.ClimateColorInfo.class

zombie.iso.BuildingDef.class
IsoCamera.class
IsoCell.class
zombie.iso.IsoChunkMap.class
IsoDirections.class
IsoGridSquare.class
zombie.iso.IsoHeatSource.class
zombie.iso.IsoLightSource.class
zombie.iso.IsoLot.class
zombie.iso.IsoLuaMover.class
zombie.iso.IsoMetaChunk.class
zombie.iso.IsoMetaCell.class
IsoMetaGrid.class
IsoMetaGrid.Trigger.class
IsoMetaGrid.Zone.class
zombie.iso.IsoMovingObject.class
IsoObject.class
zombie.iso.IsoObjectPicker.class
IsoPushableObject.class
IsoUtils.class
IsoWorld.class
zombie.iso.LosUtil.class
zombie.iso.MetaObject.class
zombie.iso.RoomDef.class
zombie.iso.SliceY.class
Vector2.class
zombie.iso.Vector3.class

LuaEventManager.class
MapObjects.class

zombie.Quests.QuestCreator.class

Server.class
ServerOptions.class
ServerOptions.BooleanServerOption.class
ServerOptions.DoubleServerOption.class
ServerOptions.IntegerServerOption.class
ServerOptions.StringServerOption.class
ServerOptions.TextServerOption.class
zombie.network.ServerSettings.class
ServerSettingsManager.class

ZombiePopulationRenderer.class
ZombiePopulationRenderer.BooleanDebugOption.class

RadioAPI.class
zombie.radio.devices.DeviceData.class
zombie.radio.devices.DevicePresets.class
zombie.radio.devices.PresetEntry.class
ZomboidRadio.class
RadioData.class
zombie.radio.scripting.RadioScriptManager.class
SLSoundManager.class
zombie.radio.StorySounds.StorySound.class
zombie.radio.StorySounds.StorySoundEvent.class
zombie.radio.StorySounds.EventSound.class
zombie.radio.StorySounds.DataPoint.class

EvolvedRecipe.class
zombie.scripting.objects.Fixing.class
zombie.scripting.objects.Fixing.Fixer.class
zombie.scripting.objects.Fixing.FixerSkill.class
zombie.scripting.objects.GameSoundScript.class
Item.class
Item.ClothingBodyLocation.class
zombie.scripting.objects.ItemRecipe.class
Recipe.class
Recipe.Result.class
Recipe.Source.class
zombie.scripting.objects.ScriptModule.class
VehicleScript.class
VehicleScript.Area.class
VehicleScript.Part.class
VehicleScript.Passenger.class
VehicleScript.Position.class
VehicleScript.Wheel.class

ScriptManager.class

zombie.ui.ActionProgressBar.class
zombie.ui.Clock.class
zombie.ui.UIDebugConsole.class
zombie.ui.LuaUIWindow.class
zombie.ui.ModalDialog.class
zombie.ui.MoodlesUI.class
zombie.ui.NewHealthPanel.class
zombie.ui.ObjectTooltip.class
zombie.ui.ObjectTooltip.Layout.class
zombie.ui.ObjectTooltip.LayoutItem.class
zombie.ui.RadarPanel.class
zombie.ui.RadialMenu.class
zombie.ui.SpeedControls.class
TextManager.class
UIElement.class
zombie.ui.UIFont.class
zombie.ui.UITransition.class
UIManager.class
zombie.ui.UIServerToolbox.class
zombie.ui.UITextBox2.class
zombie.ui.VehicleGauge.class
zombie.ui.VirtualItemSlot.class
zombie.ui.TextDrawObject.class

zombie.util.list.PZArrayList.class

BaseVehicle.class
zombie.vehicles.PathFindBehavior2.class
zombie.vehicles.VehicleDoor.class
zombie.vehicles.VehicleLight.class
VehiclePart.class
zombie.vehicles.VehicleType.class
VehicleWindow.class

zombie.DummySoundManager.class
GameSounds.class
GameTime.class
GameWindow.class
SandboxOptions.class
SandboxOptions.BooleanSandboxOption.class
SandboxOptions.DoubleSandboxOption.class
SandboxOptions.EnumSandboxOption.class
SandboxOptions.IntegerSandboxOption.class
SoundManager.class
zombie.SystemDisabler.class
VirtualZombieManager.class
zombie.characters.DummyCharacterSoundEmitter.class
zombie.characters.CharacterSoundEmitter.class
SoundManager.AmbientSoundEffect.class
BaseAmbientStreamManager.class
AmbientStreamManager.class
zombie.characters.BodyDamage.Nutrition.class
zombie.iso.objects.BSFurnace.class
zombie.iso.MultiStageBuilding.class
zombie.iso.MultiStageBuilding.Stage.class
SleepingEvent.class
zombie.iso.objects.IsoCompost.class
zombie.network.Userlog.class
zombie.network.Userlog.UserlogType.class

zombie.config.ConfigOption.class
zombie.config.BooleanConfigOption.class
zombie.config.DoubleConfigOption.class
zombie.config.EnumConfigOption.class
zombie.config.IntegerConfigOption.class
zombie.config.StringConfigOption.class
Faction.class

LuaManager.GlobalObject.LuaFileWriter.class

org.lwjgl.input.Keyboard.class
zombie.network.DBResult.class
zombie.iso.areas.NonPvpZone.class
zombie.network.DBTicket.class
zombie.core.stash.StashSystem.class
zombie.core.stash.StashBuilding.class
zombie.core.stash.Stash.class
zombie.inventory.ItemType.class

zombie.MapGroups.class

zombie.chat.ChatMessage.class
zombie.chat.ChatBase.class
zombie.chat.ServerChatMessage.class
```

### PZ debug mode only
```
Field.class
Method.class
Coroutine.class
```
