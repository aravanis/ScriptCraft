# ScriptCraft API Reference

[![MVCode](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/553/original/new-logo.png)](https://www.mvcodeclub.com)

Original content by [Walter Higgins](https://github.com/walterhiggins)

Additions and modifications by Aaron Powell @ [MVCode](https://www.mvcodeclub.com)

NOTE: **Work in Progress -- some information may be incorrect or incomplete**

## Table of Contents
 * [Global Variables](#global-variables)
   * [server variable](#server-variable)
   * [self variable](#self-variable)
   * [events variable](#events-variable)
 * [Global Functions](#global-functions)
   * [echo function](#echo-function)
   * [setTimeout() function](#settimeout-function)
   * [clearTimeout() function](#cleartimeout-function)
   * [setInterval() function](#setinterval-function)
   * [clearInterval() function](#clearinterval-function)
 * [Events](#events)
  * [Registering Events](#registering-events)
  * [List of Events](#list-of-events)
 * [Module Loading](#module-loading)
 * [String Colors](#string-colors)
 * [Items Module](#items-module)
 * [Entity Module](#entity-module)
 * [Blocks Module](#blocks-module)
 * [Recipes Module](#recipes-module)
 * [Inventory Module](#inventory-module)
 * [Action Module](#action-module)
 * [BlockFace Module](#blockface-module)
 * [Bukkit Module](#bukkit-module)
   * [bukkit.broadcastMessage()](#bukkitbroadcastmessage)
   * [bukkit.dispatchCommand()](#bukkitdispatchcommand) 
 * [Color Module](#color-module)
 * [DyeColor Module](#dyecolor-module)
 * [DamageCause Module](#damagecause-module)
 * [Effect Module](#effect-module)
 * [Enchantment Module](#enchantment-module)
 * [EntityTypeCheck Module](#entitytypecheck-module)
 * [EntityType Module](#entitytype-module)
 * [File Module](#file-module)
 * [FireworkEffect Module](#fireworkeffect-module)
 * [GameMode Module](#gamemode-module)
 * [ItemFlag Module](#itemflag-module)
 * [Location Module](#location-module)
 * [NewPotionEffect Module](#newpotioneffect-module)
 * [Scoreboard Module](#scoreboard-module)
 * [SpawnReason Module](#spawnreason-module)
 * [Particle Module](#particle-module)
 * [EquipmentSlot Module](#equipmentslot-module)
 * [Vector Module](#vector-module)
 * [Drone Module](#drone-module)
   * [Constructing a Drone Object](#constructing-a-drone-object)
   * [Drone Methods](#drone-methods)
 * [Utilities Module](#utilities-module)
   * [Utility Methods](#utility-methods)
 * [Using Spigot JavaDocs](#using-spigot-javadocs)

## Global Variables

There are a few special javascript variables provided by ScriptCraft that can be accessed in all plugins.

### server variable

The Minecraft Server object

### self variable

The current player. (Note - this value should not be used in multi-threaded scripts or event-handling code - it's not thread-safe). This variable is only safe to use at the in-game prompt and should *never* be used in modules. For example you can use it here...

```javascript
/js console.log(self.name)
```

... but not in any event handling code. `self` is a temporary short-lived variable which only exists in the context of the in-game or server command prompts.

### events variable

The `events` object is used to add new event handlers to Minecraft.

## Global Functions

ScripCraft provides some global functions which can be used by all plugins.

### echo function

The `echo()` function displays a message on the in-game screen. 

#### Example

```javascript
/js echo( self, 'Hello World')
```

### setTimeout() function

This function mimics the [setTimeout()](http://www.w3schools.com/jsref/met_win_settimeout.asp) function used in browser-based javascript. However, the function will only accept a function reference, not a string of javascript code.  Where setTimeout() in the browser returns a numeric value which can be subsequently passed to clearTimeout(), this implementation returns an object which can be subsequently passed to ScriptCraft's own clearTimeout() implementation.

#### Example

```javascript
//
// start a storm in 5 seconds
//    
setTimeout( function() {
    var world = server.worlds.get(0);
    world.setStorm(true);
}, 5000);
```

### clearTimeout() function

A scriptcraft implementation of [clearTimeout()](http://www.w3schools.com/jsref/met_win_cleartimeout.asp).

### setInterval() function

This function mimics the [setInterval()](http://www.w3schools.com/jsref/met_win_setinterval.asp) function used in browser-based javascript. However, the function will only accept a function reference, not a string of javascript code.  Where setInterval() in the browser returns a numeric value which can be subsequently passed to clearInterval(), this implementation returns an object which can be subsequently passed to ScriptCraft's own clearInterval() implementation.

### clearInterval() function

A scriptcraft implementation of [clearInterval()](http://www.w3schools.com/jsref/met_win_clearinterval.asp).

## Events

Events are how the server tells your plugin that something has happened in the world. Bukkit defines many events, in multiple categories; e.g. player actions (player logged in, player clicked a block, player died, player respawned...), block events (block placed, block broken, block's neighbour changed...), entity events (a mob targeted you, a creeper exploded...), world-wide events (a world loaded or unloaded, a chunk loaded or unloaded), and many more.

If we want our plugin to do something specific when one of these events occurs, we need to **register a callback function for that event**. This will cause the function we registered to be called by the server whenever the specified event occurs.

### Registering Events

```javascript
/*  This will register the function onBlockBreak() to be called
    whenever a BlockBreakEvent occurs, resulting in the message
    "You broke a block!" being sent to the player that broke a block
*/
var onBlockBreak = function(event) {
    echo(event.player, "You broke a block!"); 
};
events.blockBreak(onBlockBreak);
```

### List of Events

* [events.weatherChange()](#eventsweatherchange)
* [events.lightningStrike()](#eventslightningstrike)
* [events.thunderChange()](#eventsthunderchange)
* [events.vehicleMove()](#eventsvehiclemove)
* [events.vehicleDestroy()](#eventsvehicledestroy)
* [events.vehicleExit()](#eventsvehicleexit)
* [events.vehicleEntityCollision()](#eventsvehicleentitycollision)
* [events.vehicleBlockCollision()](#eventsvehicleblockcollision)
* [events.vehicleEnter()](#eventsvehicleenter)
* [events.vehicleDamage()](#eventsvehicledamage)
* [events.vehicleUpdate()](#eventsvehicleupdate)
* [events.vehicleCreate()](#eventsvehiclecreate)
* [events.enchantItem()](#eventsenchantitem)
* [events.prepareItemEnchant()](#eventsprepareitemenchant)
* [events.playerInteractEntity()](#eventsplayerinteractentity)
* [events.playerEggThrow()](#eventsplayereggthrow)
* [events.playerUnleashEntity()](#eventsplayerunleashentity)
* [events.playerInventory()](#eventsplayerinventory)
* [events.playerLevelChange()](#eventsplayerlevelchange)
* [events.playerPortal()](#eventsplayerportal)
* [events.playerItemConsume()](#eventsplayeritemconsume)
* [events.playerTeleport()](#eventsplayerteleport)
* [events.playerBedEnter()](#eventsplayerbedenter)
* [events.playerUnregisterChannel()](#eventsplayerunregisterchannel)
* [events.playerArmorStandManipulate()](#eventsplayerarmorstandmanipulate)
* [events.playerChat()](#eventsplayerchat)
* [events.playerShearEntity()](#eventsplayershearentity)
* [events.playerItemDamage()](#eventsplayeritemdamage)
* [events.asyncPlayerChat()](#eventsasyncplayerchat)
* [events.playerDropItem()](#eventsplayerdropitem)
* [events.playerRegisterChannel()](#eventsplayerregisterchannel)
* [events.playerMove()](#eventsplayermove)
* [events.playerItemBreak()](#eventsplayeritembreak)
* [events.playerBucketEmpty()](#eventsplayerbucketempty)
* [events.playerStatisticIncrement()](#eventsplayerstatisticincrement)
* [events.playerToggleFlight()](#eventsplayertoggleflight)
* [events.playerItemHeld()](#eventsplayeritemheld)
* [events.playerAchievementAwarded()](#eventsplayerachievementawarded)
* [events.playerToggleSneak()](#eventsplayertogglesneak)
* [events.playerExpChange()](#eventsplayerexpchange)
* [events.playerResourcePackStatus()](#eventsplayerresourcepackstatus)
* [events.playerPreLogin()](#eventsplayerprelogin)
* [events.playerJoin()](#eventsplayerjoin)
* [events.playerAnimation()](#eventsplayeranimation)
* [events.playerEditBook()](#eventsplayereditbook)
* [events.playerPickupItem()](#eventsplayerpickupitem)
* [events.playerInteractAtEntity()](#eventsplayerinteractatentity)
* [events.playerChangedWorld()](#eventsplayerchangedworld)
* [events.playerFish()](#eventsplayerfish)
* [events.playerChatTabComplete()](#eventsplayerchattabcomplete)
* [events.playerRespawn()](#eventsplayerrespawn)
* [events.playerBedLeave()](#eventsplayerbedleave)
* [events.asyncPlayerPreLogin()](#eventsasyncplayerprelogin)
* [events.playerInteract()](#eventsplayerinteract)
* [events.playerBucketFill()](#eventsplayerbucketfill)
* [events.playerVelocity()](#eventsplayervelocity)
* [events.playerQuit()](#eventsplayerquit)
* [events.playerLogin()](#eventsplayerlogin)
* [events.playerSwapHandItems()](#eventsplayerswaphanditems)
* [events.playerKick()](#eventsplayerkick)
* [events.playerToggleSprint()](#eventsplayertogglesprint)
* [events.playerCommandPreprocess()](#eventsplayercommandpreprocess)
* [events.playerGameModeChange()](#eventsplayergamemodechange)
* [events.furnaceSmelt()](#eventsfurnacesmelt)
* [events.prepareAnvil()](#eventsprepareanvil)
* [events.inventoryDrag()](#eventsinventorydrag)
* [events.craftItem()](#eventscraftitem)
* [events.furnaceBurn()](#eventsfurnaceburn)
* [events.inventoryOpen()](#eventsinventoryopen)
* [events.inventoryPickupItem()](#eventsinventorypickupitem)
* [events.inventoryMoveItem()](#eventsinventorymoveitem)
* [events.inventoryClick()](#eventsinventoryclick)
* [events.inventoryClose()](#eventsinventoryclose)
* [events.inventoryCreative()](#eventsinventorycreative)
* [events.inventory()](#eventsinventory)
* [events.prepareItemCraft()](#eventsprepareitemcraft)
* [events.furnaceExtract()](#eventsfurnaceextract)
* [events.brew()](#eventsbrew)
* [events.serverCommand()](#eventsservercommand)
* [events.serverListPing()](#eventsserverlistping)
* [events.serviceRegister()](#eventsserviceregister)
* [events.pluginDisable()](#eventsplugindisable)
* [events.remoteServerCommand()](#eventsremoteservercommand)
* [events.mapInitialize()](#eventsmapinitialize)
* [events.serviceUnregister()](#eventsserviceunregister)
* [events.pluginEnable()](#eventspluginenable)
* [events.villagerAcquireTrade()](#eventsvillageracquiretrade)
* [events.playerDeath()](#eventsplayerdeath)
* [events.entityCreatePortal()](#eventsentitycreateportal)
* [events.entityCombust()](#eventsentitycombust)
* [events.sheepDyeWool()](#eventssheepdyewool)
* [events.expBottle()](#eventsexpbottle)
* [events.entityTame()](#eventsentitytame)
* [events.projectileLaunch()](#eventsprojectilelaunch)
* [events.entityDamage()](#eventsentitydamage)
* [events.itemSpawn()](#eventsitemspawn)
* [events.projectileHit()](#eventsprojectilehit)
* [events.foodLevelChange()](#eventsfoodlevelchange)
* [events.itemDespawn()](#eventsitemdespawn)
* [events.villagerReplenishTrade()](#eventsvillagerreplenishtrade)
* [events.entityPortalEnter()](#eventsentityportalenter)
* [events.entityPortal()](#eventsentityportal)
* [events.entityTarget()](#eventsentitytarget)
* [events.entityDeath()](#eventsentitydeath)
* [events.entitySpawn()](#eventsentityspawn)
* [events.sheepRegrowWool()](#eventssheepregrowwool)
* [events.entityShootBow()](#eventsentityshootbow)
* [events.creeperPower()](#eventscreeperpower)
* [events.entityCombustByBlock()](#eventsentitycombustbyblock)
* [events.entityBreakDoor()](#eventsentitybreakdoor)
* [events.entityDamageByEntity()](#eventsentitydamagebyentity)
* [events.entityUnleash()](#eventsentityunleash)
* [events.entityExplode()](#eventsentityexplode)
* [events.entityInteract()](#eventsentityinteract)
* [events.entityToggleGlide()](#eventsentitytoggleglide)
* [events.explosionPrime()](#eventsexplosionprime)
* [events.horseJump()](#eventshorsejump)
* [events.creatureSpawn()](#eventscreaturespawn)
* [events.entityCombustByEntity()](#eventsentitycombustbyentity)
* [events.entityDamageByBlock()](#eventsentitydamagebyblock)
* [events.entityTargetLivingEntity()](#eventsentitytargetlivingentity)
* [events.entityTeleport()](#eventsentityteleport)
* [events.playerLeashEntity()](#eventsplayerleashentity)
* [events.spawnerSpawn()](#eventsspawnerspawn)
* [events.itemMerge()](#eventsitemmerge)
* [events.slimeSplit()](#eventsslimesplit)
* [events.pigZap()](#eventspigzap)
* [events.fireworkExplode()](#eventsfireworkexplode)
* [events.potionSplash()](#eventspotionsplash)
* [events.entityChangeBlock()](#eventsentitychangeblock)
* [events.entityPortalExit()](#eventsentityportalexit)
* [events.entityRegainHealth()](#eventsentityregainhealth)
* [events.entityBlockForm()](#eventsentityblockform)
* [events.blockSpread()](#eventsblockspread)
* [events.blockMultiPlace()](#eventsblockmultiplace)
* [events.blockExplode()](#eventsblockexplode)
* [events.notePlay()](#eventsnoteplay)
* [events.cauldronLevelChange()](#eventscauldronlevelchange)
* [events.blockFade()](#eventsblockfade)
* [events.blockPlace()](#eventsblockplace)
* [events.blockPhysics()](#eventsblockphysics)
* [events.blockIgnite()](#eventsblockignite)
* [events.blockBreak()](#eventsblockbreak)
* [events.blockBurn()](#eventsblockburn)
* [events.blockFromTo()](#eventsblockfromto)
* [events.blockRedstone()](#eventsblockredstone)
* [events.blockPistonRetract()](#eventsblockpistonretract)
* [events.blockDispense()](#eventsblockdispense)
* [events.signChange()](#eventssignchange)
* [events.blockPistonExtend()](#eventsblockpistonextend)
* [events.blockCanBuild()](#eventsblockcanbuild)
* [events.blockGrow()](#eventsblockgrow)
* [events.leavesDecay()](#eventsleavesdecay)
* [events.blockExp()](#eventsblockexp)
* [events.blockForm()](#eventsblockform)
* [events.blockDamage()](#eventsblockdamage)
* [events.hangingPlace()](#eventshangingplace)
* [events.hangingBreakByEntity()](#eventshangingbreakbyentity)
* [events.hangingBreak()](#eventshangingbreak)
* [events.structureGrow()](#eventsstructuregrow)
* [events.spawnChange()](#eventsspawnchange)
* [events.worldLoad()](#eventsworldload)
* [events.worldInit()](#eventsworldinit)
* [events.worldUnload()](#eventsworldunload)
* [events.worldSave()](#eventsworldsave)
* [events.chunkUnload()](#eventschunkunload)
* [events.chunkPopulate()](#eventschunkpopulate)
* [events.portalCreate()](#eventsportalcreate)
* [events.chunkLoad()](#eventschunkload)

### events.weatherChange()

#### Parameters 

 * callback - A function which is called whenever the [weather.WeatherChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/weather/WeatherChangeEvent.html) is fired

### events.lightningStrike()

#### Parameters 

 * callback - A function which is called whenever the [weather.LightningStrikeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/weather/LightningStrikeEvent.html) is fired



### events.thunderChange()

#### Parameters 

 * callback - A function which is called whenever the [weather.ThunderChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/weather/ThunderChangeEvent.html) is fired



### events.vehicleMove()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleMoveEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleMoveEvent.html) is fired



### events.vehicleDestroy()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleDestroyEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleDestroyEvent.html) is fired



### events.vehicleExit()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleExitEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleExitEvent.html) is fired



### events.vehicleEntityCollision()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleEntityCollisionEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleEntityCollisionEvent.html) is fired



### events.vehicleBlockCollision()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleBlockCollisionEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleBlockCollisionEvent.html) is fired



### events.vehicleEnter()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleEnterEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleEnterEvent.html) is fired



### events.vehicleDamage()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleDamageEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleDamageEvent.html) is fired



### events.vehicleUpdate()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleUpdateEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleUpdateEvent.html) is fired



### events.vehicleCreate()

#### Parameters 

 * callback - A function which is called whenever the [vehicle.VehicleCreateEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/vehicle/VehicleCreateEvent.html) is fired



### events.enchantItem()

#### Parameters 

 * callback - A function which is called whenever the [enchantment.EnchantItemEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/enchantment/EnchantItemEvent.html) is fired



### events.prepareItemEnchant()

#### Parameters 

 * callback - A function which is called whenever the [enchantment.PrepareItemEnchantEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/enchantment/PrepareItemEnchantEvent.html) is fired



### events.playerInteractEntity()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerInteractEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerInteractEntityEvent.html) is fired



### events.playerEggThrow()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerEggThrowEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerEggThrowEvent.html) is fired



### events.playerUnleashEntity()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerUnleashEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerUnleashEntityEvent.html) is fired



### events.playerInventory()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerInventoryEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerInventoryEvent.html) is fired



### events.playerLevelChange()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerLevelChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerLevelChangeEvent.html) is fired



### events.playerPortal()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerPortalEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerPortalEvent.html) is fired



### events.playerItemConsume()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerItemConsumeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerItemConsumeEvent.html) is fired



### events.playerTeleport()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerTeleportEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerTeleportEvent.html) is fired



### events.playerBedEnter()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerBedEnterEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerBedEnterEvent.html) is fired



### events.playerUnregisterChannel()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerUnregisterChannelEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerUnregisterChannelEvent.html) is fired



### events.playerArmorStandManipulate()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerArmorStandManipulateEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerArmorStandManipulateEvent.html) is fired



### events.playerChat()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerChatEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerChatEvent.html) is fired



### events.playerShearEntity()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerShearEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerShearEntityEvent.html) is fired



### events.playerItemDamage()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerItemDamageEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerItemDamageEvent.html) is fired



### events.asyncPlayerChat()

#### Parameters 

 * callback - A function which is called whenever the [player.AsyncPlayerChatEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/AsyncPlayerChatEvent.html) is fired



### events.playerDropItem()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerDropItemEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerDropItemEvent.html) is fired



### events.playerRegisterChannel()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerRegisterChannelEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerRegisterChannelEvent.html) is fired



### events.playerMove()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerMoveEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerMoveEvent.html) is fired



### events.playerItemBreak()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerItemBreakEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerItemBreakEvent.html) is fired



### events.playerBucketEmpty()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerBucketEmptyEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerBucketEmptyEvent.html) is fired



### events.playerStatisticIncrement()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerStatisticIncrementEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerStatisticIncrementEvent.html) is fired



### events.playerToggleFlight()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerToggleFlightEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerToggleFlightEvent.html) is fired



### events.playerItemHeld()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerItemHeldEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerItemHeldEvent.html) is fired



### events.playerAchievementAwarded()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerAchievementAwardedEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerAchievementAwardedEvent.html) is fired



### events.playerToggleSneak()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerToggleSneakEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerToggleSneakEvent.html) is fired



### events.playerExpChange()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerExpChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerExpChangeEvent.html) is fired



### events.playerResourcePackStatus()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerResourcePackStatusEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerResourcePackStatusEvent.html) is fired



### events.playerPreLogin()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerPreLoginEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerPreLoginEvent.html) is fired



### events.playerJoin()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerJoinEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerJoinEvent.html) is fired



### events.playerAnimation()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerAnimationEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerAnimationEvent.html) is fired



### events.playerEditBook()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerEditBookEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerEditBookEvent.html) is fired



### events.playerPickupItem()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerPickupItemEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerPickupItemEvent.html) is fired



### events.playerInteractAtEntity()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerInteractAtEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerInteractAtEntityEvent.html) is fired



### events.playerChangedWorld()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerChangedWorldEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerChangedWorldEvent.html) is fired



### events.playerFish()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerFishEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerFishEvent.html) is fired



### events.playerChatTabComplete()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerChatTabCompleteEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerChatTabCompleteEvent.html) is fired



### events.playerRespawn()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerRespawnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerRespawnEvent.html) is fired



### events.playerBedLeave()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerBedLeaveEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerBedLeaveEvent.html) is fired



### events.asyncPlayerPreLogin()

#### Parameters 

 * callback - A function which is called whenever the [player.AsyncPlayerPreLoginEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/AsyncPlayerPreLoginEvent.html) is fired



### events.playerInteract()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerInteractEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerInteractEvent.html) is fired



### events.playerBucketFill()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerBucketFillEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerBucketFillEvent.html) is fired



### events.playerVelocity()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerVelocityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerVelocityEvent.html) is fired



### events.playerQuit()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerQuitEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerQuitEvent.html) is fired



### events.playerLogin()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerLoginEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerLoginEvent.html) is fired



### events.playerSwapHandItems()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerSwapHandItemsEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerSwapHandItemsEvent.html) is fired



### events.playerKick()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerKickEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerKickEvent.html) is fired



### events.playerToggleSprint()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerToggleSprintEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerToggleSprintEvent.html) is fired



### events.playerCommandPreprocess()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerCommandPreprocessEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerCommandPreprocessEvent.html) is fired



### events.playerGameModeChange()

#### Parameters 

 * callback - A function which is called whenever the [player.PlayerGameModeChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerGameModeChangeEvent.html) is fired



### events.furnaceSmelt()

#### Parameters 

 * callback - A function which is called whenever the [inventory.FurnaceSmeltEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/FurnaceSmeltEvent.html) is fired



### events.prepareAnvil()

#### Parameters 

 * callback - A function which is called whenever the [inventory.PrepareAnvilEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/PrepareAnvilEvent.html) is fired



### events.inventoryDrag()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryDragEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryDragEvent.html) is fired



### events.craftItem()

#### Parameters 

 * callback - A function which is called whenever the [inventory.CraftItemEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/CraftItemEvent.html) is fired



### events.furnaceBurn()

#### Parameters 

 * callback - A function which is called whenever the [inventory.FurnaceBurnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/FurnaceBurnEvent.html) is fired



### events.inventoryOpen()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryOpenEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryOpenEvent.html) is fired



### events.inventoryPickupItem()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryPickupItemEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryPickupItemEvent.html) is fired



### events.inventoryMoveItem()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryMoveItemEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryMoveItemEvent.html) is fired



### events.inventoryClick()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryClickEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryClickEvent.html) is fired



### events.inventoryClose()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryCloseEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryCloseEvent.html) is fired



### events.inventoryCreative()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryCreativeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryCreativeEvent.html) is fired



### events.inventory()

#### Parameters 

 * callback - A function which is called whenever the [inventory.InventoryEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/InventoryEvent.html) is fired



### events.prepareItemCraft()

#### Parameters 

 * callback - A function which is called whenever the [inventory.PrepareItemCraftEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/PrepareItemCraftEvent.html) is fired



### events.furnaceExtract()

#### Parameters 

 * callback - A function which is called whenever the [inventory.FurnaceExtractEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/FurnaceExtractEvent.html) is fired



### events.brew()

#### Parameters 

 * callback - A function which is called whenever the [inventory.BrewEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/inventory/BrewEvent.html) is fired



### events.serverCommand()

#### Parameters 

 * callback - A function which is called whenever the [server.ServerCommandEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/ServerCommandEvent.html) is fired



### events.serverListPing()

#### Parameters 

 * callback - A function which is called whenever the [server.ServerListPingEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/ServerListPingEvent.html) is fired



### events.serviceRegister()

#### Parameters 

 * callback - A function which is called whenever the [server.ServiceRegisterEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/ServiceRegisterEvent.html) is fired



### events.pluginDisable()

#### Parameters 

 * callback - A function which is called whenever the [server.PluginDisableEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/PluginDisableEvent.html) is fired



### events.remoteServerCommand()

#### Parameters 

 * callback - A function which is called whenever the [server.RemoteServerCommandEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/RemoteServerCommandEvent.html) is fired



### events.mapInitialize()

#### Parameters 

 * callback - A function which is called whenever the [server.MapInitializeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/MapInitializeEvent.html) is fired



### events.serviceUnregister()

#### Parameters 

 * callback - A function which is called whenever the [server.ServiceUnregisterEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/ServiceUnregisterEvent.html) is fired



### events.pluginEnable()

#### Parameters 

 * callback - A function which is called whenever the [server.PluginEnableEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/server/PluginEnableEvent.html) is fired



### events.villagerAcquireTrade()

#### Parameters 

 * callback - A function which is called whenever the [entity.VillagerAcquireTradeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/VillagerAcquireTradeEvent.html) is fired



### events.playerDeath()

#### Parameters 

 * callback - A function which is called whenever the [entity.PlayerDeathEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/PlayerDeathEvent.html) is fired



### events.entityCreatePortal()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityCreatePortalEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityCreatePortalEvent.html) is fired



### events.entityCombust()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityCombustEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityCombustEvent.html) is fired



### events.sheepDyeWool()

#### Parameters 

 * callback - A function which is called whenever the [entity.SheepDyeWoolEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/SheepDyeWoolEvent.html) is fired



### events.expBottle()

#### Parameters 

 * callback - A function which is called whenever the [entity.ExpBottleEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/ExpBottleEvent.html) is fired



### events.entityTame()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityTameEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityTameEvent.html) is fired



### events.projectileLaunch()

#### Parameters 

 * callback - A function which is called whenever the [entity.ProjectileLaunchEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/ProjectileLaunchEvent.html) is fired



### events.entityDamage()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityDamageEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityDamageEvent.html) is fired



### events.itemSpawn()

#### Parameters 

 * callback - A function which is called whenever the [entity.ItemSpawnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/ItemSpawnEvent.html) is fired



### events.projectileHit()

#### Parameters 

 * callback - A function which is called whenever the [entity.ProjectileHitEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/ProjectileHitEvent.html) is fired



### events.foodLevelChange()

#### Parameters 

 * callback - A function which is called whenever the [entity.FoodLevelChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/FoodLevelChangeEvent.html) is fired



### events.itemDespawn()

#### Parameters 

 * callback - A function which is called whenever the [entity.ItemDespawnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/ItemDespawnEvent.html) is fired



### events.villagerReplenishTrade()

#### Parameters 

 * callback - A function which is called whenever the [entity.VillagerReplenishTradeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/VillagerReplenishTradeEvent.html) is fired



### events.entityPortalEnter()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityPortalEnterEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityPortalEnterEvent.html) is fired



### events.entityPortal()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityPortalEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityPortalEvent.html) is fired



### events.entityTarget()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityTargetEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityTargetEvent.html) is fired



### events.entityDeath()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityDeathEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityDeathEvent.html) is fired



### events.entitySpawn()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntitySpawnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntitySpawnEvent.html) is fired



### events.sheepRegrowWool()

#### Parameters 

 * callback - A function which is called whenever the [entity.SheepRegrowWoolEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/SheepRegrowWoolEvent.html) is fired



### events.entityShootBow()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityShootBowEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityShootBowEvent.html) is fired



### events.creeperPower()

#### Parameters 

 * callback - A function which is called whenever the [entity.CreeperPowerEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/CreeperPowerEvent.html) is fired



### events.entityCombustByBlock()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityCombustByBlockEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityCombustByBlockEvent.html) is fired



### events.entityBreakDoor()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityBreakDoorEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityBreakDoorEvent.html) is fired



### events.entityDamageByEntity()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityDamageByEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityDamageByEntityEvent.html) is fired



### events.entityUnleash()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityUnleashEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityUnleashEvent.html) is fired



### events.entityExplode()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityExplodeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityExplodeEvent.html) is fired



### events.entityInteract()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityInteractEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityInteractEvent.html) is fired



### events.entityToggleGlide()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityToggleGlideEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityToggleGlideEvent.html) is fired



### events.explosionPrime()

#### Parameters 

 * callback - A function which is called whenever the [entity.ExplosionPrimeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/ExplosionPrimeEvent.html) is fired



### events.horseJump()

#### Parameters 

 * callback - A function which is called whenever the [entity.HorseJumpEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/HorseJumpEvent.html) is fired



### events.creatureSpawn()

#### Parameters 

 * callback - A function which is called whenever the [entity.CreatureSpawnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/CreatureSpawnEvent.html) is fired



### events.entityCombustByEntity()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityCombustByEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityCombustByEntityEvent.html) is fired



### events.entityDamageByBlock()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityDamageByBlockEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityDamageByBlockEvent.html) is fired



### events.entityTargetLivingEntity()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityTargetLivingEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityTargetLivingEntityEvent.html) is fired



### events.entityTeleport()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityTeleportEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityTeleportEvent.html) is fired



### events.playerLeashEntity()

#### Parameters 

 * callback - A function which is called whenever the [entity.PlayerLeashEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/PlayerLeashEntityEvent.html) is fired



### events.spawnerSpawn()

#### Parameters 

 * callback - A function which is called whenever the [entity.SpawnerSpawnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/SpawnerSpawnEvent.html) is fired



### events.itemMerge()

#### Parameters 

 * callback - A function which is called whenever the [entity.ItemMergeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/ItemMergeEvent.html) is fired



### events.slimeSplit()

#### Parameters 

 * callback - A function which is called whenever the [entity.SlimeSplitEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/SlimeSplitEvent.html) is fired



### events.pigZap()

#### Parameters 

 * callback - A function which is called whenever the [entity.PigZapEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/PigZapEvent.html) is fired



### events.fireworkExplode()

#### Parameters 

 * callback - A function which is called whenever the [entity.FireworkExplodeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/FireworkExplodeEvent.html) is fired



### events.potionSplash()

#### Parameters 

 * callback - A function which is called whenever the [entity.PotionSplashEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/PotionSplashEvent.html) is fired



### events.entityChangeBlock()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityChangeBlockEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityChangeBlockEvent.html) is fired



### events.entityPortalExit()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityPortalExitEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityPortalExitEvent.html) is fired



### events.entityRegainHealth()

#### Parameters 

 * callback - A function which is called whenever the [entity.EntityRegainHealthEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityRegainHealthEvent.html) is fired



### events.entityBlockForm()

#### Parameters 

 * callback - A function which is called whenever the [block.EntityBlockFormEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/EntityBlockFormEvent.html) is fired



### events.blockSpread()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockSpreadEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockSpreadEvent.html) is fired



### events.blockMultiPlace()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockMultiPlaceEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockMultiPlaceEvent.html) is fired



### events.blockExplode()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockExplodeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockExplodeEvent.html) is fired



### events.notePlay()

#### Parameters 

 * callback - A function which is called whenever the [block.NotePlayEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/NotePlayEvent.html) is fired



### events.cauldronLevelChange()

#### Parameters 

 * callback - A function which is called whenever the [block.CauldronLevelChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/CauldronLevelChangeEvent.html) is fired



### events.blockFade()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockFadeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockFadeEvent.html) is fired



### events.blockPlace()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockPlaceEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockPlaceEvent.html) is fired



### events.blockPhysics()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockPhysicsEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockPhysicsEvent.html) is fired



### events.blockIgnite()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockIgniteEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockIgniteEvent.html) is fired



### events.blockBreak()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockBreakEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockBreakEvent.html) is fired



### events.blockBurn()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockBurnEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockBurnEvent.html) is fired



### events.blockFromTo()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockFromToEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockFromToEvent.html) is fired



### events.blockRedstone()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockRedstoneEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockRedstoneEvent.html) is fired



### events.blockPistonRetract()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockPistonRetractEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockPistonRetractEvent.html) is fired



### events.blockDispense()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockDispenseEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockDispenseEvent.html) is fired



### events.signChange()

#### Parameters 

 * callback - A function which is called whenever the [block.SignChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/SignChangeEvent.html) is fired



### events.blockPistonExtend()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockPistonExtendEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockPistonExtendEvent.html) is fired



### events.blockCanBuild()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockCanBuildEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockCanBuildEvent.html) is fired



### events.blockGrow()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockGrowEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockGrowEvent.html) is fired



### events.leavesDecay()

#### Parameters 

 * callback - A function which is called whenever the [block.LeavesDecayEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/LeavesDecayEvent.html) is fired



### events.blockExp()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockExpEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockExpEvent.html) is fired



### events.blockForm()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockFormEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockFormEvent.html) is fired



### events.blockDamage()

#### Parameters 

 * callback - A function which is called whenever the [block.BlockDamageEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/block/BlockDamageEvent.html) is fired



### events.hangingPlace()

#### Parameters 

 * callback - A function which is called whenever the [hanging.HangingPlaceEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/hanging/HangingPlaceEvent.html) is fired



### events.hangingBreakByEntity()

#### Parameters 

 * callback - A function which is called whenever the [hanging.HangingBreakByEntityEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/hanging/HangingBreakByEntityEvent.html) is fired



### events.hangingBreak()

#### Parameters 

 * callback - A function which is called whenever the [hanging.HangingBreakEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/hanging/HangingBreakEvent.html) is fired



### events.structureGrow()

#### Parameters 

 * callback - A function which is called whenever the [world.StructureGrowEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/StructureGrowEvent.html) is fired



### events.spawnChange()

#### Parameters 

 * callback - A function which is called whenever the [world.SpawnChangeEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/SpawnChangeEvent.html) is fired



### events.worldLoad()

#### Parameters 

 * callback - A function which is called whenever the [world.WorldLoadEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/WorldLoadEvent.html) is fired



### events.worldInit()

#### Parameters 

 * callback - A function which is called whenever the [world.WorldInitEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/WorldInitEvent.html) is fired



### events.worldUnload()

#### Parameters 

 * callback - A function which is called whenever the [world.WorldUnloadEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/WorldUnloadEvent.html) is fired



### events.worldSave()

#### Parameters 

 * callback - A function which is called whenever the [world.WorldSaveEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/WorldSaveEvent.html) is fired



### events.chunkUnload()

#### Parameters 

 * callback - A function which is called whenever the [world.ChunkUnloadEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/ChunkUnloadEvent.html) is fired



### events.chunkPopulate()

#### Parameters 

 * callback - A function which is called whenever the [world.ChunkPopulateEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/ChunkPopulateEvent.html) is fired



### events.portalCreate()

#### Parameters 

 * callback - A function which is called whenever the [world.PortalCreateEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/PortalCreateEvent.html) is fired



### events.chunkLoad()

#### Parameters 

 * callback - A function which is called whenever the [world.ChunkLoadEvent event](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/world/ChunkLoadEvent.html) is fired

## Module Loading

If we are writing a plugin that relies on functions provided by ScriptCraft modules, we need to load them into our plugin.js file.  We can import all of the modules at once by loading `master.js` as shown:

```javascript
load("scriptcraft/modules/bukkit/master.js");
```

Adding this line to the top of a plugin file will give you access to all ScriptCraft modules.

## String Colors

The display color of strings can be changed by using the string formatting methods provided by this module.

### Usage

```javascript
"This text is red!".red()
```

### Example

```javascript
echo(self, "Hello Minecraft!".aqua());
```
![Hello Minecraft!](https://d14nx13ylsx7x8.cloudfront.net/lesson_image_blocks/assets/000/004/661/original/temp1450223230.png)


### The following string formatting methods are available:

  * aqua()
  * black()
  * blue()
  * bold()
  * brightgreen()
  * darkaqua()
  * darkblue()
  * darkgray()
  * darkgreen()
  * purple()
  * darkpurple()
  * darkred()
  * gold()
  * gray()
  * green()
  * italic()
  * lightpurple()
  * indigo()
  * green()
  * red()
  * pink()
  * yellow()
  * white()
  * strike()
  * random()
  * magic()
  * underline()
  * reset()

## Items Module

The `items` module exports a factory function for each item material in Minecraft that returns an `ItemStack`. The functions take one argument, `numItems`, that determines how many of the given item will be in the returned ItemStack. `numItems` has a default value of `1`.

See the [Spigot JavaDocs: Material](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html) for a list of possible item materials.

### Usage

    items.book(); // returns an ItemStack with one book in it
    items.book(2); // returns an ItemStack with two books in it

Items with names that are longer than one word are written in [camelCase](https://en.wikipedia.org/wiki/CamelCase) which means the first word is lowercase and each subsquent word is capitalized, with all spaces removed.

If we want an item with the type `ACACIA_FENCE_GATE` we would write `items.acaciaFenceGate()`

### Example

```javascript
// adds 5 baked potatoes to the inventory of "player"
inventory(player).add(items.bakedPotato(5));
player.updateInventory();
```

### List of item functions

  * acaciaDoor()
  * acaciaDoorItem()
  * acaciaFence()
  * acaciaFenceGate()
  * acaciaStairs()
  * activatorRail()
  * air()
  * anvil()
  * apple()
  * armorStand()
  * arrow()
  * bakedPotato()
  * banner()
  * barrier()
  * beacon()
  * bed()
  * bedBlock()
  * bedrock()
  * beetroot()
  * beetrootBlock()
  * beetrootSeeds()
  * beetrootSoup()
  * birchDoor()
  * birchDoorItem()
  * birchFence()
  * birchFenceGate()
  * birchWoodStairs()
  * blazePowder()
  * blazeRod()
  * boat()
  * boatAcacia()
  * boatBirch()
  * boatDarkOak()
  * boatJungle()
  * boatSpruce()
  * bone()
  * boneBlock()
  * book()
  * bookAndQuill()
  * bookshelf()
  * bow()
  * bowl()
  * bread()
  * brewingStand()
  * brewingStandItem()
  * brick()
  * brickStairs()
  * brownMushroom()
  * bucket()
  * burningFurnace()
  * cactus()
  * cake()
  * cakeBlock()
  * carpet()
  * carrot()
  * carrotItem()
  * carrotStick()
  * cauldron()
  * cauldronItem()
  * chainmailBoots()
  * chainmailChestplate()
  * chainmailHelmet()
  * chainmailLeggings()
  * chest()
  * chorusFlower()
  * chorusFruit()
  * chorusFruitPopped()
  * chorusPlant()
  * clay()
  * clayBall()
  * clayBrick()
  * coal()
  * coalBlock()
  * coalOre()
  * cobbleWall()
  * cobblestone()
  * cobblestoneStairs()
  * cocoa()
  * command()
  * commandChain()
  * commandMinecart()
  * commandRepeating()
  * compass()
  * cookedBeef()
  * cookedChicken()
  * cookedFish()
  * cookedMutton()
  * cookedRabbit()
  * cookie()
  * crops()
  * darkOakDoor()
  * darkOakDoorItem()
  * darkOakFence()
  * darkOakFenceGate()
  * darkOakStairs()
  * daylightDetector()
  * daylightDetectorInverted()
  * deadBush()
  * detectorRail()
  * diamond()
  * diamondAxe()
  * diamondBarding()
  * diamondBlock()
  * diamondBoots()
  * diamondChestplate()
  * diamondHelmet()
  * diamondHoe()
  * diamondLeggings()
  * diamondOre()
  * diamondPickaxe()
  * diamondSpade()
  * diamondSword()
  * diode()
  * diodeBlockOff()
  * diodeBlockOn()
  * dirt()
  * dispenser()
  * doublePlant()
  * doubleStep()
  * doubleStoneSlab2()
  * dragonEgg()
  * dragonsBreath()
  * dropper()
  * egg()
  * elytra()
  * emerald()
  * emeraldBlock()
  * emeraldOre()
  * emptyMap()
  * enchantedBook()
  * enchantmentTable()
  * endBricks()
  * endCrystal()
  * endGateway()
  * endRod()
  * enderChest()
  * enderPearl()
  * enderPortal()
  * enderPortalFrame()
  * enderStone()
  * expBottle()
  * explosiveMinecart()
  * eyeOfEnder()
  * feather()
  * fence()
  * fenceGate()
  * fermentedSpiderEye()
  * fire()
  * fireball()
  * firework()
  * fireworkCharge()
  * fishingRod()
  * flint()
  * flintAndSteel()
  * flowerPot()
  * flowerPotItem()
  * frostedIce()
  * furnace()
  * ghastTear()
  * glass()
  * glassBottle()
  * glowingRedstoneOre()
  * glowstone()
  * glowstoneDust()
  * goldAxe()
  * goldBarding()
  * goldBlock()
  * goldBoots()
  * goldChestplate()
  * goldHelmet()
  * goldHoe()
  * goldIngot()
  * goldLeggings()
  * goldNugget()
  * goldOre()
  * goldPickaxe()
  * goldPlate()
  * goldRecord()
  * goldSpade()
  * goldSword()
  * goldenApple()
  * goldenCarrot()
  * grass()
  * grassPath()
  * gravel()
  * greenRecord()
  * grilledPork()
  * hardClay()
  * hayBlock()
  * hopper()
  * hopperMinecart()
  * hugeMushroom1()
  * hugeMushroom2()
  * ice()
  * inkSack()
  * ironAxe()
  * ironBarding()
  * ironBlock()
  * ironBoots()
  * ironChestplate()
  * ironDoor()
  * ironDoorBlock()
  * ironFence()
  * ironHelmet()
  * ironHoe()
  * ironIngot()
  * ironLeggings()
  * ironOre()
  * ironPickaxe()
  * ironPlate()
  * ironSpade()
  * ironSword()
  * ironTrapdoor()
  * itemFrame()
  * jackOLantern()
  * jukebox()
  * jungleDoor()
  * jungleDoorItem()
  * jungleFence()
  * jungleFenceGate()
  * jungleWoodStairs()
  * ladder()
  * lapisBlock()
  * lapisOre()
  * lava()
  * lavaBucket()
  * leash()
  * leather()
  * leatherBoots()
  * leatherChestplate()
  * leatherHelmet()
  * leatherLeggings()
  * leaves()
  * leaves2()
  * lever()
  * lingeringPotion()
  * log()
  * log2()
  * longGrass()
  * magma()
  * magmaCream()
  * map()
  * melon()
  * melonBlock()
  * melonSeeds()
  * melonStem()
  * milkBucket()
  * minecart()
  * mobSpawner()
  * monsterEgg()
  * monsterEggs()
  * mossyCobblestone()
  * mushroomSoup()
  * mutton()
  * mycel()
  * nameTag()
  * netherBrick()
  * netherBrickItem()
  * netherBrickStairs()
  * netherFence()
  * netherStalk()
  * netherStar()
  * netherWartBlock()
  * netherWarts()
  * netherrack()
  * noteBlock()
  * obsidian()
  * packedIce()
  * painting()
  * paper()
  * pistonBase()
  * pistonExtension()
  * pistonMovingPiece()
  * pistonStickyBase()
  * poisonousPotato()
  * pork()
  * portal()
  * potato()
  * potatoItem()
  * potion()
  * poweredMinecart()
  * poweredRail()
  * prismarine()
  * prismarineCrystals()
  * prismarineShard()
  * pumpkin()
  * pumpkinPie()
  * pumpkinSeeds()
  * pumpkinStem()
  * purpurBlock()
  * purpurDoubleSlab()
  * purpurPillar()
  * purpurSlab()
  * purpurStairs()
  * quartz()
  * quartzBlock()
  * quartzOre()
  * quartzStairs()
  * rabbit()
  * rabbitFoot()
  * rabbitHide()
  * rabbitStew()
  * rails()
  * rawBeef()
  * rawChicken()
  * rawFish()
  * record10()
  * record11()
  * record12()
  * record3()
  * record4()
  * record5()
  * record6()
  * record7()
  * record8()
  * record9()
  * redMushroom()
  * redNetherBrick()
  * redRose()
  * redSandstone()
  * redSandstoneStairs()
  * redstone()
  * redstoneBlock()
  * redstoneComparator()
  * redstoneComparatorOff()
  * redstoneComparatorOn()
  * redstoneLampOff()
  * redstoneLampOn()
  * redstoneOre()
  * redstoneTorchOff()
  * redstoneTorchOn()
  * redstoneWire()
  * rottenFlesh()
  * saddle()
  * sand()
  * sandstone()
  * sandstoneStairs()
  * sapling()
  * seaLantern()
  * seeds()
  * shears()
  * shield()
  * sign()
  * signPost()
  * skull()
  * skullItem()
  * slimeBall()
  * slimeBlock()
  * smoothBrick()
  * smoothStairs()
  * snow()
  * snowBall()
  * snowBlock()
  * soil()
  * soulSand()
  * speckledMelon()
  * spectralArrow()
  * spiderEye()
  * splashPotion()
  * sponge()
  * spruceDoor()
  * spruceDoorItem()
  * spruceFence()
  * spruceFenceGate()
  * spruceWoodStairs()
  * stainedClay()
  * stainedGlass()
  * stainedGlassPane()
  * standingBanner()
  * stationaryLava()
  * stationaryWater()
  * step()
  * stick()
  * stone()
  * stoneAxe()
  * stoneButton()
  * stoneHoe()
  * stonePickaxe()
  * stonePlate()
  * stoneSlab2()
  * stoneSpade()
  * stoneSword()
  * storageMinecart()
  * string()
  * structureBlock()
  * structureVoid()
  * sugar()
  * sugarCane()
  * sugarCaneBlock()
  * sulphur()
  * thinGlass()
  * tippedArrow()
  * tnt()
  * torch()
  * trapDoor()
  * trappedChest()
  * tripwire()
  * tripwireHook()
  * vine()
  * wallBanner()
  * wallSign()
  * watch()
  * water()
  * waterBucket()
  * waterLily()
  * web()
  * wheat()
  * wood()
  * woodAxe()
  * woodButton()
  * woodDoor()
  * woodDoubleStep()
  * woodHoe()
  * woodPickaxe()
  * woodPlate()
  * woodSpade()
  * woodStairs()
  * woodStep()
  * woodSword()
  * woodenDoor()
  * wool()
  * workbench()
  * writtenBook()
  * yellowFlower()


## Entity Module

The `entity` module provides easy access to all of the entity interfaces in Minecraft.  Entities are any non-voxel objects that can exist in a world, including all players, monsters, projectiles, etc.

For a description of the entities, click [here](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/entity/package-summary.html)

### Usage

```javascript
entity.zombie      // refers to the org.bukkit.entity.ZOMBIE interface
```

### Example

```javascript
// launches a witherSkull projectile if the player right-clicks the air while holding a bone
var onPlayerInteract = function(event) {
	if (event.action == action.rightClickAir) {
		var player = event.player;
		if (player.itemInHand.type.equals(material.bone)) {
		  var skull = player.launchProjectile(entity.witherSkull.class);
			skull.shooter = player;
			skull.velocity = player.location.direction.multiply(1.8);
		}
	}
};
events.playerInteract(onPlayerInteract);
```

### List of entities

  * ageable	
  * ambient	
  * animals	
  * animalTamer	 
  * areaEffectCloud	
  * armorStand	 
  * arrow	
  * bat	
  * blaze	
  * boat	
  * caveSpider	
  * chicken	
  * complexEntityPart	
  * complexLivingEntity	
  * cow	
  * creature	
  * creeper	
  * damageable	
  * dragonFireball	 
  * egg	
  * enderCrystal	
  * enderDragon	
  * enderDragonPart	
  * enderman	
  * endermite	 
  * enderPearl	
  * enderSignal	
  * entity	
  * experienceOrb	
  * explosive	
  * fallingBlock	
  * fallingSandDeprecated
  * fireball	
  * firework	 
  * fishHook	
  * flying	
  * ghast	
  * giant	
  * golem	
  * guardian	 
  * hanging	
  * horse
  * humanEntity	
  * ironGolem	
  * item	
  * itemFrame	
  * largeFireball	
  * leashHitch	
  * lightningStrike	
  * lingeringPotion	
  * livingEntity	
  * magmaCube	
  * minecart	
  * monster	
  * mushroomCow	
  * npc	
  * ocelot	
  * painting	
  * pig	
  * pigZombie	
  * player	
  * polarBear	
  * poweredMinecart
  * projectile	
  * rabbit	 
  * sheep	
  * shulker	 
  * shulkerBullet	 
  * silverfish	
  * skeleton	
  * slime	
  * smallFireball	
  * snowball	
  * snowman	
  * spectralArrow	
  * spider	
  * splashPotion	
  * squid	
  * storageMinecart
  * tameable	 
  * thrownExpBottle	
  * thrownPotion	
  * tippedArrow	 
  * tNTPrimed	
  * vehicle	
  * villager	
  * waterMob	
  * weather	
  * witch	
  * wither	
  * witherSkull	
  * wolf	
  * zombie	

## Blocks Module

The `blocks` module provides easy access to all block types without having to use their [data values](http://minecraft.gamepedia.com/Data_values/Block_IDs).

### Usage

```javascript
blocks.oak
blocks.wool.green
```

### Examples

```javascript
box( blocks.oak ); // creates a single oak wood block
box( blocks.sand, 3, 2, 1 ); // creates a block of sand 3 wide x 2 high x 1 long
box( blocks.wool.green, 2 ); // creates a block of green wool 2 blocks wide
```

There is also convenience array `blocks.rainbow` which is an array of the 7 colors of the rainbow (or closest
approximations).

The blocks module is globally exported by the Drone module.

## Recipes Module

The `recipes` module provides convenience functions for adding and removing recipes from the game.

The `recipes.add()` function takes an object with 3 keys as a parameter:

* result: 	ItemStack the recipe will create
* ingredients: 	Object with key-value pairs denoting the letter that will represent each type of ingredient in the "shape"
* shape: 	Array that defines the shape of the recipe in the crafting table.  It **must be** an array of 3 strings each with 3 characters, representing the 9 spaces in the crafting table.  The first element is the top row, the second the middle row, and the third is the bottom row.  Use spaces to represent empty slots in the table.

### Usage

```javascript
recipes.add(
{
    result: resultItem,
    ingredients: {X: ingredientItem1, Y: ingedientItem2},
    shape: [" Y ", "YXY", " Y "]
});
```

### Example

```javascript
// Adds a recipe that can be used to craft a custom bow 
// called the "Bow of Exploding" using a bow and TNT
var bow = items.bow(1);
var tnt = items.tnt(1);
var explodeBow = items.bow(1);

var explodeBowMeta = explodeBow.itemMeta;
explodeBowMeta.displayName = "Bow of Exploding";
explodeBowMeta.lore = ["Excite. Very boom."];
explodeBow.itemMeta = explodeBowMeta;

recipes.add(
{
    result: explodeBow,
    ingredients: {B: bow, T: tnt},
    shape: ["   ", "TB ", "   "]
});
```

## Inventory Module

This module provides functions to add items to, remove items from and check the contents of a player or NPC's inventory. 

### Usage

The `inventory` module is best used in conjunction with the items module.

```javascript
// gives every player a cookie and a baked potato
utils.players(function(player){
  inventory(player)
    .add( items.cookie(1) )
    .add( items.bakedPotato(1) )
});

// give a player 6 cookies then take away 4 of them
inventory(player)
  .add( items.cookie(6) )
  .remove ( items.cookie(4) )

// check if a player has any cookies
var hasCookies = inventory(player).contains( items.cookie(1) );
```

The inventory module exposes a single function which when passed a player or NPC will return an object with 3 methods:
    *NOTE: all methods expect a parameter of the type `org.bukkit.inventory.ItemStack`
            so use the `items` module to construct items to pass into these methods

* add : Adds items to the inventory
* remove : removes items from the inventory
* contains : checks to see if there is the specified type and amount of item in the inventory

### Example

```javascript
// When a player throws a snowball, adds a new one to their inventory
var onSnowballThrow = function(event) {
  if (isSnowball(event.entity)) {
    var player = event.entity.shooter;
    inventory(player).add(items.snowBall(1));
  }
};
events.projectileLaunch(onSnowballThrow);
```

## Action Module

The `action` module can be used to reference the 5 different action types players can perform.

### Usage

```javascript
action.leftClickAir       // refers to org.bukkit.event.block.Action.LEFT_CLICK_AIR
```

### Example

```javascript
// Sends player a message if they right-click on a block
var onPlayerInteract = function(event) {
  if (event.action == action.rightClickBlock) {
    echo(event.player, "You right-clicked on a block!")
  }
}
events.playerInteract(onPlayerInteract);
```

### Possible actions:

  * leftClickAir
  * leftClickBlock
  * rightClickAir
  * rightClickBlock
  * physical                      <--- physical action occurs when a player steps on a pressure plate

## BlockFace Module

The BlockFace module can be used to reference the different possible directions a block can be facing (for blocks like stairs and armor stands)

### Usage

```javascript
blockFace.south         // refers to org.bukkit.block.BlockFace.SOUTH
```

### Example

```javascript
// sets the direction bannerBlock is facing to "south"
// bannerBlock is a standing banner block
var bannerData = bannerBlock.state.data;
bannerData.facingDirection = blockFace.south;
bannerState.update();
```

### Possible BlockFace directions:

  * down
  * east
  * eastNorthEast
  * eastSouthEast
  * north
  * northEast
  * northNorthEast
  * northNorthWest
  * northWest
  * self
  * south
  * southEast
  * southSouthEast
  * southSouthWest
  * southWest
  * up
  * west
  * westNorthWest
  * westSouthWest

## Bukkit Module

The `bukkit` module provides functions that are executed server-wide.

The full list of functions can be found in the [Method Summary of the Bukkit Class](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Bukkit.html)

### bukkit.broadcastMessage()

The `bukkit.broadcastMessage()` function can be used to broadcast a message to all players on the server.

#### Parameters

  * message: the message you want to send (string)

#### Example

```javascript
// sends the message "Welcome to the server!" to all players on the server
bukkit.broadcastMessage("Welcome to the server!");
```

### bukkit.dispatchCommand()

The `bukkit.dispatchCommand()` function can be used to execute a command as if it were sent from the server's console.

NOTE: **Be careful when sending commands to the server, it will do anything you ask it to!**

#### Parameters

  * sender: the sender of the command (CommandSender)
  
  	NOTE: This will usually be the "console" referenced by `server.consoleSender`
  * command: the command you want to execute (string)

#### Example

```javascript
// executes the command "time set 0" as if we typed "/time set 0" in game as OP
// or "time set 0" into the server console directly
bukkit.dispatchCommand(server.consoleSender, "time set 0");
```

## Color Module

Provides access to all possible colors in `org.bukkit.Color`

NOTE: The "color" module is not to be confused with the "dyeColor" module.  The "dyeColor" module is used for coloring dyes, cloth, and banners whereas the "color" module is used for coloring things like leather armor.

### Usage
```javascript
color.red                   // references org.bukkit.Color.RED
```

### Example

```javascript
// Sets the color of a pair of leather boots to red and gives them to "player"
var boots = items.leatherBoots(1);
var bootsMeta = boots.itemMeta;
bootsMeta.color = color.red;
boots.itemMeta = bootsMeta;
player.equipment.boots = boots;
```

### List of colors:

  * aqua
  * black
  * blue
  * fuchsia
  * gray
  * green
  * lime
  * maroon
  * navy
  * olive
  * orange
  * purple
  * red
  * silver
  * teal
  * white
  * yellow

## DyeColor Module

Provides access to all possible colors in `org.bukkit.DyeColor`

NOTE: The "dyeColor" module is not to be confused with the "color" module.  The "dyeColor" module is used for coloring dyes, cloth, and banners whereas the "color" module is used for coloring things like leather armor.

### Usage
```javascript
dyeColor.blue                   // references org.bukkit.DyeColor.BLUE
```

### Example

```javascript
// sets the dyeColor of bannerBlock to blue
// bannerBlock is a standing banner block
var bannerState = bannerBlock.state;
bannerState.baseColor = dyeColor.blue;
bannerState.update();
```

### List of dye colors:

  * black
  * blue
  * brown
  * cyan
  * gray
  * green
  * lightblue
  * lime
  * magenta
  * orange
  * pink
  * purple
  * red
  * silver
  * white
  * yellow

## DamageCause Module

Provides access to all possible causes of damage in `org.bukkit.event.entity.EntityDamageEvent.DamageCause` that could result in an EntityDamageEvent being fired.

### Usage
```javascript
damageCause.fall                   // returns enum org.bukkit.event.entity.EntityDamageEvent.DamageCause.FALL
```

### Example

```javascript
// prevents entities from taking falling damage
var onEntityDamage = function(event) {
  if (event.cause == damageCause.fall) {
    event.cancelled = true;
  }
}
events.entityDamage(onEntityDamage);
```

### List of damage causes:

  * blockExplosion
  	* Damage caused by being in the area when a block explodes.
  * contact
  	* Damage caused when an entity contacts a block such as a Cactus.
  * custom
  	* Custom damage.
  * dragonBreath
  	* Damage caused by a dragon breathing fire.
  * drowning
  	* Damage caused by running out of air while in water
  * entityAttack
  	* Damage caused when an entity attacks another entity.
  * entityExplosion
  	* Damage caused by being in the area when an entity, such as a Creeper, explodes.
  * fall
  	* Damage caused when an entity falls a distance greater than 3 blocks
  * fallingBlock
  	* Damage caused by being hit by a falling block which deals damage
  * fire
  	* Damage caused by direct exposure to fire
  * fireTick
  	* Damage caused due to burns caused by fire
  * flyIntoWall
  	* Damage caused when an entity runs into a wall.
  * hotFloor
  	* Damage caused when an entity steps on Material.MAGMA.
  * lava
  	* Damage caused by direct exposure to lava
  * lightning
  	* Damage caused by being struck by lightning
  * magic
  	* Damage caused by being hit by a damage potion or spell
  * melting
  	* Damage caused due to a snowman melting
  * poison
  	* Damage caused due to an ongoing poison effect
  * projectile
  	* Damage caused when attacked by a projectile.
  * starvation
  	* Damage caused by starving due to having an empty hunger bar
  * suffocation
  	* Damage caused by being put in a block
  * suicide
  	* Damage caused by committing suicide using the command "/kill"
  * thorns
  	* Damage caused in retaliation to another attack by the Thorns enchantment.
  * void
  	* Damage caused by falling into the void
  * wither
  	* Damage caused by Wither potion effect

## Effect Module

Effects are sent to players' clients by the server and add visuals and/or sounds to the game.  The `effect` module provides access to all effect enums in `org.bukkit.Effect`.  [Spigot JavaDocs: Effect](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Effect.html)

Use the `playEffect()` function to display the effects, as shown in the example below.

### Usage
```javascript
effect.enderSignal                // returns enum org.bukkit.Effect.ENDER_SIGNAL
```

### Example

```javascript
// Displays the "mobspawnerFlames" visual effect when the player moves
var onPlayerMove = function(event) {
    var player = event.player;
    player.playEffect(player.location, effect.mobspawnerFlames, 5);
};
events.playerMove(onPlayerMove);
```

### List of effects:

  * anvilBreak
  	* The sound played when an anvil breaks
  * anvilLand
  	* The sound played when an anvil lands after falling
  * anvilUse
  	* The sound played when an anvil is used
  * batTakeoff
  	* Sound played by a bat taking off
  * blazeShoot
  	* Sound of blaze firing.
  * bowFire
  	* Sound of a bow firing.
  * brewingStandBrew
  	* The sound played by brewing stands when brewing
  * chorusFlowerDeath
  	* The sound played when a chorus flower dies
  * chorusFlowerGrow
  	* The sound played when a chorus flower grows
  * click1
  	* A click sound.
  * click2
  	* An alternate click sound.
  * cloud
  	* A puff of white smoke
  * colouredDust
  	* Multicolored dust particles
  * crit
  	* Critical hit particles
  * doorClose
  	* Sound of a door closing.
  * doorToggle
  	* Sound of a door opening.
  * dragonBreath
  	* The sound/particles used by the enderdragon's breath attack.
  * endGatewaySpawn
  	* The sound/particles caused by a end gateway spawning
  * enderSignal
  	* An ender eye signal; a visual effect.
  * enderdragonGrowl
  	* The sound of an enderdragon growling
  * enderdragonShoot
  	* Sound of an enderdragon firing
  * endereyeLaunch
  	* The sound played when launching an endereye
  * explosion
  	* Explosion particles
  * explosionHuge
  	* The biggest explosion particle effect
  * explosionLarge
  	* A larger version of the explode particle
  * extinguish
  	* Sound of fire being extinguished.
  * fenceGateClose
  	* Sound of a door closing.
  * fenceGateToggle
  	* Sound of a door opening.
  * fireworkShoot
  	* The sound played when launching a firework
  * fireworksSpark
  	* The spark that comes off a fireworks
  * flame
  	* Fire particles
  * flyingGlyph
  	* The symbols that fly towards the enchantment table
  * footstep
  	* A small gray square
  * ghastShoot
  	* Sound of ghast firing.
  * ghastShriek
  	* Sound of ghast shrieking.
  * happyVillager
  	* The particle that appears when trading with a villager
  * heart
  	* The particle that appears when breading animals
  * instantSpell
  	* A puff of white stars
  * ironDoorClose
  	* Sound of a door closing.
  * ironDoorToggle
  	* Sound of a door opening.
  * ironTrapdoorClose
  	* Sound of a door closing.
  * ironTrapdoorToggle
  	* Sound of a door opening.
  * itemBreak
  	* The particles generated when a tool breaks.
  * largeSmoke
  	* The smoke particles that appears on blazes, minecarts with furnaces and fire
  * lavaPop
  	* The particles that pop out of lava
  * lavadrip
  	* The lava drip particle that appears on blocks under lava
  * magicCrit
  	* Blue critical hit particles
  * mobspawnerFlames
  	* The flames seen on a mobspawner; a visual effect.
  * note
  	* The note that appears above note blocks
  * particleSmoke
  	* Smoke particles
  * portal
  	* The particles shown at nether portals
  * portalTravel
  	* The sound played when traveling through a portal
  * potionBreak
  	* Visual effect of a splash potion breaking.
  * potionSwirl
  	* Multicolored potion effect particles
  * potionSwirlTransparent
  	* Multicolored potion effect particles that are slightly transparent
  * recordPlay
  	* A song from a record.
  * slime
  	* The particle shown when a slime jumps
  * smallSmoke
  	* Small gray particles
  * smoke
  	* A visual smoke effect.
  * snowShovel
  	* White particles
  * snowballBreak
  	* Snowball breaking
  * spell
  	* A puff of white potion swirls
  * splash
  	* Water particles
  * stepSound
  	* Sound of a block breaking.
  * tileBreak
  	* The particles generated while breaking a block.
  * tileDust
  	* The particles generated while sprinting a block This particle requires a Material and data value so that the client can select the correct texture.
  * trapdoorClose
  	* Sound of a trapdoor closing.
  * trapdoorToggle
  	* Sound of a trapdoor opening.
  * villagerPlantGrow
  	* Particles displayed when a villager grows a plant, data is the number of particles
  * villagerThundercloud
  	* The particle that appears when hitting a villager
  * voidFog
  	* Small gray particles
  * waterdrip
  	* The water drip particle that appears on blocks under water
  * witchMagic
  	* A puff of purple particles
  * witherBreakBlock
  	* The sound played when a wither breaks a block
  * witherShoot
  	* Sound of a wither shooting
  * zombieChewIronDoor
  	* Sound of zombies chewing on iron doors.
  * zombieChewWoodenDoor
  	* Sound of zombies chewing on wooden doors.
  * zombieConvertedVillager
  	* The sound played when a villager is converted by a zombie
  * zombieDestroyDoor
  	* Sound of zombies destroying a door.
  * zombieInfect
  	* The sound played when a zombie infects a target

## Enchantment Module

Provides access to all enchantments in Minecraft that can be applied to equipment.  Use the `addEnchant()` function to apply the enchantment to an item's ItemMeta. [Spigot JavaDocs: ItemMeta.addEnchant()](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/inventory/meta/ItemMeta.html#addEnchant(org.bukkit.enchantments.Enchantment,%20int,%20boolean))

### Usage

```javascript
enchantment.durability				// returns the Field org.bukkit.enchantments.Enchantment.DURABILITY
```

### Example

```javascript
// adds the "durability" enchantment to the "sword"
var sword = items.diamondSword();
var swordMeta = sword.itemMeta;
swordMeta.addEnchant(enchantment.durability, 1, true);
sword.itemMeta = swordMeta;
```

### List of enchantments:

  * arrowDamage
	* Provides extra damage when shooting arrows from bows
  * arrowFire
	* Sets entities on fire when hit by arrows shot from a bow
  * arrowInfinite
	* Provides infinite arrows when shooting a bow
  * arrowKnockback
	* Provides a knockback when an entity is hit by an arrow from a bow
  * damageAll
	* Increases damage against all targets
  * damageArthropods
	* Increases damage against arthropod targets
  * damageUndead
	* Increases damage against undead targets
  * depthStrider
	* Increases walking speed while in water
  * digSpeed
	* Increases the rate at which you mine/dig
  * durability
	* Decreases the rate at which a tool looses durability
  * fireAspect
	* When attacking a target, has a chance to set them on fire
  * frostWalker
	* Freezes any still water adjacent to ice / frost which player is walking on
  * knockback
	* All damage to other targets will knock them back when hit
  * lootBonusBlocks
	* Provides a chance of gaining extra loot when destroying blocks
  * lootBonusMobs
	* Provides a chance of gaining extra loot when killing monsters
  * luck
	* Decreases odds of catching worthless junk
  * lure
	* Increases rate of fish biting your hook
  * mending
	* Allows mending the item using experience orbs
  * oxygen
	* Decreases the rate of air loss whilst underwater
  * protectionEnvironmental
	* Provides protection against environmental damage
  * protectionExplosions
	* Provides protection against explosive damage
  * protectionFall
	* Provides protection against fall damage
  * protectionFire
	* Provides protection against fire damage
  * protectionProjectile
	* Provides protection against projectile damage
  * silkTouch
	* Allows blocks to drop themselves instead of fragments (for example, stone instead of cobblestone)
  * thorns
	* Damages the attacker
  * waterWorker
	* Increases the speed at which a player may mine underwater


## EntityTypeCheck Module

Provides a set of functions that can be used to check if an entity is of a certain type.  For a list of all entity types in Minecraft, refer to [Spigot JavaDocs: EntityType](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/entity/EntityType.html)

### Usage

```javascript
isChicken(entity)			// returns "true" if "entity" is a chicken
```

### Example

```javascript
// when a projectile hits something check to see if it is an "Arrow"
// if it is, create an explosion where it landed
function onProjectileHit(event) {
  var projectile = event.entity;
  var world = projectile.world;
  if (isArrow(projectile)) {
    projectile.remove();
    world.createExplosion(projectile.location, 5);
  }
}
events.projectileHit(onProjectileHit);
```

## EntityType Module

The entityType module provides access to all of the EntityType enums in `org.bukkit.entity.EntityType`.

[Spigot JavaDocs: EntityType](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/entity/EntityType.html)

Often used as a parameter of methods like `World.spawnEntity()` to specify the type of entity that should be spawned.

[Spigot JavaDocs: World.spawnEntity()](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/World.html#spawnEntity(org.bukkit.Location,%20org.bukkit.entity.EntityType))

### Usage

```javascript
entityType.chicken				// returns the enum org.bukkit.entity.EntityType.CHICKEN
```

### Example

```javascript
// spawns a "chicken" entity at the target location
// when the player right-clicks on a block
var world = server.worlds.get(0);
var onPlayerInteract = function(event) {
  if (event.action == action.rightClickBlock) {
    var clickedBlockLoc = event.clickedBlock.location;
    world.spawnEntity(clickedBlockLoc, entityType.chicken);
  }
}
events.playerInteract(onPlayerInteract);
```

## File Module

The `file` module provides functions for reading and writing persistent JSON data to files so that plugins that require it can maintain their state across server restarts or reloads.

### file.load()

Returns a string containing all text in the data file loaded.

#### Parameters

filename: string specifying the name of the file to load data from

#### Usage

```javascript
var rawData = file.load("data-file.json");
```

### file.write()

Saves the desired JSON data (as a string) to a file with the specified name.

#### Parameters

filename: string specifying the name of the file to write data to

data: stringifyed JSON data to be written to the file

#### Usage

```javascript
file.write("data-file.json", '{"data": []}');
```

### Example

```javascript
exports.setWarp = function(label) {

    var rawData;
    var data;
    var warpLocations;

    try {
        rawData = file.load("warping-data.json");
        data = JSON.parse(rawData);
        warpLocations = data[0].warpLocations;
        if (Object.keys(warpLocations).length === 0) {
            warpLocations = [];
        }
    }
    catch(err) {
        file.write("warping-data.json", '{"warpLocations": []}');
        warpLocations = [];
    }

    if (typeof(label) != "string") {
        echo("Usage: /js setWarp(<label>);".red());
        return;
    } else if (label == "list") {
        echo("Cannot store a location with the name 'list'".red());
        return;
    }

    warpLocations.push( {
                            "label":    label,
                            "x":        self.location.x,
                            "y":        self.location.y,
                            "z":        self.location.z,
                            "pitch":    self.location.pitch,
                            "yaw":      self.location.yaw
                        });

    data = {"warpLocations": warpLocations};
    var toSave = JSON.stringify(data);
    file.write("warping-data.json", toSave);

    echo("Warp location of ".green() + label.yellow() + " has been set!".green());
};

exports.warp = function(label) {

    var rawData;
    var data;
    var warpLocations;

    try {
        rawData = file.load("warping-data.json");
        data = JSON.parse(rawData);
        warpLocations = data[0].warpLocations;
    }
    catch(err) {
        file.write("warping-data.json", '{"warpLocations": []}');
        warpLocations = [];
    }

    var warpLabels = [];
    for (var i = 0; i < warpLocations.length; i++) {
        warpLabels[i] = warpLocations[i].label;
    }
    if (typeof(label) != "string") {
        echo("Usage: /js warp(<label>);   --or--   /js warp('list');".red());
        return;
    } else if (warpLabels.indexOf(label) == -1) {
        echo("The warp location for ".red() + label.yellow() + " has not been set!".red());
        return;
    }
    var warpData = {};
    for (var j = 0; j < warpLocations.length; j++) {
        if (warpLocations[j].label == label) {
            warpData = warpLocations[j];
        }
    }
    var loc = location(self.world, warpData.x, warpData.y, warpData.z);
    loc.yaw = warpData.yaw;
    loc.pitch = warpData.pitch;
    self.teleport(loc);
    echo("Teleported player to ".green() + label.yellow() + "!".green());
};
```

## FireworkEffect Module

Provides access to all firework effect types as well as a the `setFireworkEffect()` function which can be used to set the properties of a firework when launching it from a plugin.

### Usage

```javascript
fireworkEffect.type.burst		// refers to org.bukkit.FireworkEffect.Type.BURST
```

```javascript
var firework = location.world.spawnEntity(location, entityType.firework);
var properties = {
	                "flicker":      false,
	                "mainColor":    color.red,
	                "fadeColor":    color.blue,
	                "fireworkType": fireworkEffect.type.burst,
	                "trail":        true,
	                "power":        2
                 };
fireworkEffect.setFireworkEffect(firework, properties);
```

### Example

```javascript
// lauches a firework when the player right-clicks while holding
// a firework charge.  The firework effect is specified in "properties"
var launchFirework = function(location) {
    
    var firework = location.world.spawnEntity(location, entityType.firework);
    
    var properties = {
                        "flicker":      false,
                        "mainColor":    color.red,
                        "fadeColor":    color.blue,
                        "fireworkType": fireworkEffect.type.burst,
                        "trail":        true,
                        "power":        2
                    };
                    
    fireworkEffect.setFireworkEffect(firework, properties);
};

var onPlayerInteract = function(event) {
    if (event.action === action.rightClickAir) {
        var player = event.player;
        var location = player.location;
        if (player.itemInHand.type.equals(material.fireworkCharge)) {
            launchFirework(location);
        }
    }
};
events.playerInteract(onPlayerInteract);
```

## GameMode Module

Provies easy reference to each possible game mode.

### Usage

```javascript
player.gameMode = gameMode.survival;		// sets the game mode of "player" to "survival"
```

### Example

```javascript
// sets player gamemode to spectator when they join the server
var onPlayerJoin = function(event) {
	var player = event.player;
	player.gameMode = gameMode.spectator;
}
events.playerJoin(onPlayerJoin);
```

### List of game modes:

  * survival
  * creative
  * spectator
  * adventure

## ItemFlag Module

The `itemFlag` module provides access to all of [item flags](#https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/inventory/ItemFlag.html) that can be added to the ItemMeta of an ItemStack.  These flags are used to prevent certain attributes of the item from being displayed without removing their functionality.

Can be used with the `ItemMeta.addItemFlags()` function to add the desired item flag to an ItemStack.

[Spigot JavaDocs: ItemMeta.addItemFlags()](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/inventory/meta/ItemMeta.html#addItemFlags(org.bukkit.inventory.ItemFlag...))

### Usage

```javascript
itemFlag.hideEnchants				// refers to org.bukkit.inventory.ItemFlag.HIDE_ENCHANTS
```

### Example

```javascript
// hides the durability enchant from displaying on the sword's tooltip
var sword = items.diamondSword();
var swordMeta = sword.itemMeta;
swordMeta.addEnchant(enchantment.durability, 1, true);
swordMeta.addItemFlags(itemFlag.hideEnchants);
sword.itemMeta = swordMeta;
```

### List of item flags:

  * hideAttributes
	* Setting to show/hide Attributes like Damage
  * hideDestroys
	* Setting to show/hide what the ItemStack can break/destroy
  * hideEnchants
	* Setting to show/hide enchants
  * hidePlacedOn
	* Setting to show/hide where this ItemStack can be build/placed on
  * hidePotionEffects
	* Setting to show/hide potion effects on this ItemStack
  * hideUnbreakable
	* Setting to show/hide the unbreakable State

## Location Module

The `location` module provides a constructor function `location()` that returns an `org.bukkit.Location`.  This can be useful when you need to reference a location in the world based on coordinates (e.g. when spawning entities or teleporting).

### Parameters

world: 	(World) the location is in

x: 	(int, float) x position of location

y: 	(int, float) y position of location

z: 	(int, float) z position of location

pitch:	OPTIONAL - (int, float) horizonatal camera position for player

yaw:	OPTIONAL - (int, float) vertical camera position for player

### Usage

```javascript
var world = server.worlds.get(0);
var x = 10;
var y = 70;
var z = 10;
var newLoc = location(world, x, y, z);
```

### Example
```javascript
// teleports the player to the targetLoc
var world = server.worlds.get(0);
var x = 10;
var y = 70;
var z = 10;
var targetLoc = location(world, x, y, z);
teleport(player, targetLoc);
```

## NewPotionEffect Module

The `newPotionEffect` module provides a function `newPotionEffect()` that can be used to construct an `org.bukkit.potion.PotionEffect` when you want to apply an effect to a player or custom mob using the `LivingEntity.addPotionEffect()` function.

[Spigot JavaDocs: LivingEntity.addPotionEffect()](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/entity/LivingEntity.html#addPotionEffect(org.bukkit.potion.PotionEffect))

### Parameters

type: 		(string) the type of potion effect

duration:	(int, "max") the duration of the effect in seconds (or "max" for maximum duration

strength:	(int) the power level of the effect

### Usage

```javascript
var potionEffect = newPotionEffect("speed", "max", 3);
player.addPotionEffect(potionEffect);
```

### Example

```javascript
// When a skeleton spawns give it
// 2 potions effects (speed and strength)
var onCreatureSpawn = function(event) {

	if (isSkeleton(event.entity)) {

		var skelly = event.entity;

		var skellyEffect1 = newPotionEffect("speed", "max", 5);
		var skellyEffect2 = newPotionEffect("increase_damage", "max", 10);
		skelly.addPotionEffect(skellyEffect1);
		skelly.addPotionEffect(skellyEffect2);
		
	}
};
events.creatureSpawn(onCreatureSpawn);
```

### List of potion effects:

  * absorption
	* Increases the maximum health of an entity with health that cannot be regenerated, but is refilled every 30 seconds.
  * blindness
	* Blinds an entity.
  * confusion
	* Warps vision on the client.
  * damage_resistance
	* Decreases damage dealt to an entity.
  * fast_digging
	* Increases dig speed.
  * fire_resistance
	* Stops fire damage.
  * glowing
	* Outlines the entity so that it can be seen from afar.
  * harm
	* Hurts an entity.
  * heal
	* Heals an entity.
  * health_boost
	* Increases the maximum health of an entity.
  * hunger
	* Increases hunger.
  * increase_damage
	* Increases damage dealt.
  * invisibility
	* Grants invisibility.
  * jump
	* Increases jump height.
  * levitation
	* Causes the entity to float into the air.
  * luck
	* Loot table luck.
  * night_vision
	* Allows an entity to see in the dark.
  * poison
	* Deals damage to an entity over time.
  * regeneration
	* Regenerates health.
  * saturation
	* Increases the food level of an entity each tick.
  * slow
	* Decreases movement speed.
  * slow_digging
	* Decreases dig speed.
  * speed
	* Increases movement speed.
  * unluck
	* Loot table unluck.
  * water_breathing
	* Allows breathing underwater.
  * weakness
	* Decreases damage dealt by an entity.
  * wither
	* Deals damage to an entity over time and gives the health to the shooter.


## Scoreboard Module

The `scoreboard` module provides a suite of functions that can be used to create, modify, and display scoreboards to players on the Minecraft server.  An instance of the scoreboard object can display scores in 1-3 of the available slots.  This means if you want scores displayed in both the sidebar and the player list you don't need two different scoreboards, you can add both to the same scoreboard. To begin, create a scoreboard object using `scoreboard.newSB()`...

There are three different types of scoreboards in Minecraft: sidebar, player_list and below_name.

#### sidebar

![Sidebar Scoreboard](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/556/original/sidebarSB.png)

#### player_list

![PlayerList Scoreboard](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/557/original/playerlistSB.png)

#### below_name

![BelowName Scoreboard](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/558/original/belownameSB.png)

### scoreboard.newSB()

Returns a scoreboard object with the specified properties.  Need to use `scoreboard.setSB()` before it will be displayed.

#### Parameters

Each parameter is an array of strings of length 1-3. All array lengths must match.


displayNames: 	(array of strings) The display names for each scoreboard display slot

objectives: 	(array of strings) Keywords for the objectives to display in each display slot

displaySlot: 	(array of strings) The display slots for each part of the scoreboard

#### Usage

```javascript
// scoreboard with only one display slot with the
// display name "Snowball Hits", showing the value of "hits"
// in the "sidebar" position
var sb = scoreboard.newSB(["Snowball Hits"], ["hits"], ["sidebar"]);
```

```javascript
// scoreboard with all three display slots filled
// "Snowball Hits" displaying "hits" in the "player_list"
// "Flag Captures" displaying "captures" in the "sidebar"
// "Health" displaying "hp" in the "below_name" position
var sb = scoreboard.newSB(["Snowball Hits", "Flag Captures", "Health"], 
			  ["hits", "captures", "hp"], 
			  ["player_list", "sidebar", "below_name"]);
```

### scoreboard.setSB()

Sets the scoreboard that is being displayed for a specific player.

#### Parameters

player: 	(Player) The player to set the scoreboard for

scoreboard: 	(Scoreboard) The scoreboard to display

#### Usage

```javascript
scoreboard.setSB(player, sb);		// sets the scoreboard displayed for "player" to "sb"
```

### scoreboard.clearSB()

Clears all values from a given scoreboard.

#### Parameters

scoreboard: 	(Scoreboard) The scoreboard to clear

#### Usage

```javascript
scoreboard.clearSB(sb);			// clears the scoreboard "sb"
```

### scoreboard.setPlayerScore()

Sets a player's score for a specific display slot in a scoreboard.

#### Parameters

player:		(Player) The player whose score we want to set

scoreboard:	(Scoreboard) The scoreboard we want to modify

displaySlot: 	(string) The display slot we are setting the score for

score:		(int) The new score to display for the "player" on the "scoreboard" in the given "displaySlot"

#### Usage

```javascript
scoreboard.setPlayerScore(player, sb, "player_list", 0); 	// Sets the score for "player" to "0" in the "player_list" slot of "sb"
```

### scoreboard.addTeamToSB()

Adds team to scoreboard.

#### Parameters

team:		(string) The name of the team to add to the scoreboard

scoreboard:	(Scoreboard) The scoreboard to add the team to

#### Returns

Returns a Team object. The team can have players added to it and display options customized.

#### Usage

```javascript
var blueTeam = scoreboard.addTeamToSB("Blue Team", sb);		// Adds team named "Blue Team" to scoreboard and returns a Team
```

### scoreboard.addPlayerToTeam()

Adds a player to a team.

#### Parameters

player:		(Player) The player to add to the team

team:		(Team) The team to add the player to

#### Usage

```javascript
// Adds player to blueTeam. "blueTeam" must be a Team object generated using scoreboard.addTeamToSB()
scoreboard.addPlayerToTeam(player, blueTeam);
```

### scoreboard.setTeamPrefix()

Sets the prefix to display for members of a team.

#### Parameters

team:		(Team) The team to set the prefix for

prefix:		(string) The prefix you want displayed in front of the names of players on the given team

#### Usage

```javascript
scoreboard.setTeamPrefix(blueTeam, "BLUE: ".blue());		// Sets team prefix for the blueTeam to "BLUE: " printed in blue text
```

### scoreboard.setTeamDisplayName()

Sets the display name for a team.

#### Parameters

team:		(Team) The team to set the display name for

displayName:	(string) The desired display name for the given team

#### Usage

```javascript
scoreboard.setTeamDisplayName(blueTeam, "Blue Bandits".blue());		// Sets display name of blueTeam to "Blue Bandits" in blue text
```

### scoreboard.getTeam()

Returns the team of the given name on the given scoreboard.

#### Parameters

teamName:	(string) The name of the team you want to retrieve the Team object for

scoreboard:	(Scoreboard) The scoreboard you want to get the Team from

#### Returns

Returns the Team with the given name from the Scoreboard 

#### Usage

```javascript
var blueTeam = scoreboard.getTeam("Blue Team", sb);
```

### Example

```javascript
// scoreboard example
```

## SpawnReason Module

The `spawnReason` module provides access to all of [spawn reasons](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/event/entity/CreatureSpawnEvent.SpawnReason.html) that could result in an entity spawning into the world.  This is helpful when you want to only modify entities that were generated in a specific way rather than all entities of that type.  For example, you may want to modify cows that spawn from eggs but not all naturally generated cows.

### Usage

```javascript
spawnReason.spawnerEgg			// refers to org.bukkit.event.entity.CreatureSpawnEvent.SpawnReason.SPAWNER_EGG
```

### Example

```javascript
// When we spawn a skeleton using a spawner egg
// give it a custom name
var onCreatureSpawn = function(event) {

	if (isSkeleton(event.entity) && event.spawnReason == spawnReason.spawnerEgg) {
	
		var skelly = event.entity;
		skelly.customName = "Skeleton From Spawner Egg";
		skelly.customNameVisible = true;
		
	}
};
events.creatureSpawn(onCreatureSpawn);
```

## Particle Module

The `particle` module provides access to all of [particle types](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Particle.html) that can be used in Minecraft. This is useful when you are trying to add particle effects to plugins when certain things happen, such as a spell being cast.

The `World.spawnParticle()` function will often be used to spawn the desired particle into the world at a specific location.

[Spigot JavaDocs: World.spawnParticle()](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/World.html#spawnParticle(org.bukkit.Particle,%20org.bukkit.Location,%20int,%20double,%20double,%20double,%20double))

### Usage

```javascript
var world = server.worlds.get(0);
var loc = location(50,50,50);
world.spawnParticle(particle.spell, loc, 10);    // spawns *10* *spell* particles at *loc* in *world*
```

### Example

```javascript
var onEntityDamageByEntity = function(event) {
  if (!isPlayer(event.damager)) { return; }
  
  var player = event.damager;
  var playerLoc = player.location;
  var playerInv = player.inventory;
  var playerItem = playerInv.itemInMainHand;
  var damagedEntity = event.entity;
  var damagedEntityLoc = damagedEntity.location;

  if (playerItem.equals(magicWand_Vanishing)) {
    damagedEntity.remove();
    world.spawnParticle(particle.spell, damagedEntityLoc, 7);
    event.cancelled = true;
    return;
  }
};
events.entityDamageByEntity(onEntityDamageByEntity);
```

### List of particles:

Visual demonstration of particles: (http://minecraft.gamepedia.com/Particles)

  * barrier 
  * blockCrack 
  * blockDust 
  * cloud 
  * crit 
  * critMagic 
  * damageIndicator 
  * dragonBreath 
  * dripLava 
  * dripWater 
  * enchantmentTable 
  * endRod 
  * explosionHuge 
  * explosionLarge 
  * explosionNormal 
  * fallingDust 
  * fireworksSpark 
  * flame 
  * footstep 
  * heart 
  * itemCrack 
  * itemTake 
  * lava 
  * mobAppearance 
  * note 
  * portal 
  * redstone 
  * slime 
  * smokeLarge 
  * smokeNormal 
  * snowShovel 
  * snowball 
  * spell 
  * spellInstant 
  * spellMob 
  * spellMobAmbient 
  * spellWitch 
  * suspended 
  * suspendedDepth 
  * sweepAttack 
  * townAura 
  * villagerAngry 
  * villagerHappy 
  * waterBubble 
  * waterDrop 
  * waterSplash 
  * waterWake 


## Drone Module

The Drone object can be used to build things in Minecraft using JavaScript!

It uses a fluent interface which means all of the Drone's methods return `this` and can be chained together like so...

    var theDrone = new Drone(self);
    theDrone.up().left().box(blocks.oak).down().fwd(3).cylinder0(blocks.lava,8); 

### Constructing a Drone Object

Drones can be created in any of the following ways...
    
 1. Calling any one of the methods listed below will return a Drone object. For example...
         
        var d = box( blocks.oak )

   ... creates a 1x1x1 wooden block at the cross-hairs or player's location and returns a Drone object. This might look odd (if you're familiar with Java's Object-dot-method syntax) but all of the Drone class's methods are also global functions that return new Drone objects. This is short-hand for creating drones and is useful for playing around with Drones at the in-game command prompt. It's shorter than typing ...
    
        var d = new Drone(self).box( blocks.oak ) 
        
   ... All of the Drone's methods return `this` so you can chain operations together like this...
        
        var d = box( blocks.oak )
                  .up()
                  .box( blocks.oak ,3,1,3)
                  .down()
                  .fwd(2)
                  .box( blocks.oak )
                  .turn()
                  .fwd(2)
                  .box( blocks.oak )
                  .turn()
                  .fwd(2)
                  .box( blocks.oak );
    
 2. Using the following form...

        d = new Drone(self)
    
    ...will create a new Drone taking the current player as the parameter. If the player's cross-hairs are pointing at a block at the time then, that block's location becomes the drone's starting point.  If the cross-hairs are _not_ pointing at a block, then the drone's starting location will be 2 blocks directly in front of the player.  TIP: Building always happens right and front of the drone's position...
    
    Plan View:

        ^
        |
        |
        D---->
      
    For convenience you can use a _corner stone_ to begin building. The corner stone should be located just above ground level. If the cross-hair is point at or into ground level when you create a new Drone() with either a player or location given as a parameter, then building begins at the location the player was looking at or at the location. You can get around this by pointing at a 'corner stone' just above ground level or alternatively use the following statement...
    
        d = new Drone(self).up();
          
    ... which will move the drone up one block as soon as it's created.

    ![corner stone](img/cornerstone1.png)

 3. Or by using the following form...
    
        d = new Drone(x,y,z,direction,world);

    This will create a new Drone at the location you specified using x, y, z In minecraft, the X axis runs west to east and the Z axis runs north to south.  The direction parameter says what direction you want the drone to face: 0 = east, 1 = south, 2 = west, 3 = north.  If the direction parameter is omitted, the player's direction is used instead. Both the `direction` and `world` parameters are optional.

 4. Create a new Drone based on a Location object...

        d = new Drone(location);

    This is useful when you want to create a drone at a given `org.bukkit.Location` . The `Location` class is used throughout the bukkit API. For example, if you want to create a drone when a block is broken at the block's location you would do so like this...

        events.blockBreak( function( event ) { 
            var location = event.block.location;
            var drone = new Drone(location);
            // do more stuff with the drone here...
        });

#### Parameters

 * Player : If a player reference is given as the sole parameter then the block the player was looking at will be used as the starting point for the drone. If the player was not looking at a block then the player's location will be used as the starting point. If a `Player` object is provided as a paramter then it should be the only parameter.
 * location  : *NB* If a `Location` object is provided as a parameter, then it should be the only parameter.
 * x : The x coordinate of the Drone (x,y,z,direction and world are not needed if either a player or location parameter is provided)
 * y : The y coordinate of the Drone 
 * z : The z coordinate of the Drone 
 * direction : The direction in which the Drone is facing. Possible values are 0 (east), 1 (south), 2 (west) or 3 (north) 
 * world : The world in which the drone is created. 

### Drone Methods
 * [Drone.box() method](#dronebox-method)
 * [Drone.box0() method](#dronebox0-method)
 * [Drone.boxa() method](#droneboxa-method)
 * [Chaining](#chaining)
 * [Drone Properties](#drone-properties)
 * [Extending Drone](#extending-drone)
 * [Drone.extend() static method](#droneextend-static-method)
 * [Drone Constants](#drone-constants)
 * [Drone.times() Method](#dronetimes-method)
 * [Drone.arc() method](#dronearc-method)
 * [Drone.bed() method](#dronebed-method)
 * [Drone.blocktype() method](#droneblocktype-method)
 * [Copy & Paste using Drone](#copy--paste-using-drone)
 * [Drone.copy() method](#dronecopy-method)
 * [Drone.paste() method](#dronepaste-method)
 * [Drone.cylinder() method](#dronecylinder-method)
 * [Drone.cylinder0() method](#dronecylinder0-method)
 * [Drone.door() method](#dronedoor-method)
 * [Drone.door_iron() method](#dronedoor_iron-method)
 * [Drone.door2() method](#dronedoor2-method)
 * [Drone.door2_iron() method](#dronedoor2_iron-method)
 * [Drone.firework() method](#dronefirework-method)
 * [Drone.garden() method](#dronegarden-method)
 * [Drone.ladder() method](#droneladder-method)
 * [Drone Movement](#drone-movement)
 * [Drone Positional Info](#drone-positional-info)
 * [Drone Markers](#drone-markers)
 * [Drone.prism() method](#droneprism-method)
 * [Drone.prism0() method](#droneprism0-method)
 * [Drone.rand() method](#dronerand-method)
 * [Drone.wallsign() method](#dronewallsign-method)
 * [Drone.signpost() method](#dronesignpost-method)
 * [Drone.sign() method](#dronesign-method)
 * [Drone.sphere() method](#dronesphere-method)
 * [Drone.sphere0() method](#dronesphere0-method)
 * [Drone.hemisphere() method](#dronehemisphere-method)
 * [Drone.hemisphere0() method](#dronehemisphere0-method)
 * [Drone.stairs() function](#dronestairs-function)
 * [Drone Trees methods](#drone-trees-methods)
 * [Drone.castle() method](#dronecastle-method)
 * [Drone.chessboard() method](#dronechessboard-method)
 * [Drone.cottage() method](#dronecottage-method)
 * [Drone.cottage_road() method](#dronecottage_road-method)
 * [Drone.dancefloor() method](#dronedancefloor-method)
 * [Drone.fort() method](#dronefort-method)
 * [Drone.hangtorch() method](#dronehangtorch-method)
 * [Drone.lcdclock() method.](#dronelcdclock-method)
 * [Drone.logojs() method](#dronelogojs-method)
 * [Drone.maze() method](#dronemaze-method)
 * [Drone.rainbow() method](#dronerainbow-method)
 * [Drone.spiral_stairs() method](#dronespiral_stairs-method)
 * [Drone.temple() method](#dronetemple-method)

### Drone.box() method

the box() method is a convenience method for building things. (For the more performance-oriented method - see cuboid)

#### Parameters

 * b - the block id - e.g. 6 for an oak sapling or '6:2' for a birch sapling. Alternatively you can use any one of the `blocks` values e.g. `blocks.sapling.birch`
 * w (optional - default 1) - the width of the structure 
 * h (optional - default 1) - the height of the structure 
 * d (optional - default 1) - the depth of the structure - NB this is not how deep underground the structure lies - this is how far away (depth of field) from the drone the structure will extend.

#### Example

To create a black structure 4 blocks wide, 9 blocks tall and 1 block long...
    
    box(blocks.wool.black, 4, 9, 1);

... or the following code does the same but creates a variable that can be used for further methods...

    var drone = new Drone(self);
    drone.box(blocks.wool.black, 4, 9, 1);

![box example 1](img/boxex1.png)
    
### Drone.box0() method

Another convenience method - this one creates 4 walls with no floor or ceiling.

#### Parameters

 * block - the block id - e.g. 6 for an oak sapling or '6:2' for a birch sapling. Alternatively you can use any one of the `blocks` values e.g. `blocks.sapling.birch`
 * width (optional - default 1) - the width of the structure 
 * height (optional - default 1) - the height of the structure 
 * length (optional - default 1) - the length of the structure - how far
   away (depth of field) from the drone the structure will extend.

#### Example

To create a stone building with the insided hollowed out 7 wide by 3 tall by 6 long...

    box0( blocks.stone, 7, 3, 6);

![example box0](img/box0ex1.png)
   
### Drone.boxa() method

Construct a cuboid using an array of blocks. As the drone moves first along the width axis, then the height (y axis) then the length, each block is picked from the array and placed.

#### Parameters

 * blocks - An array of blocks - each block in the array will be placed in turn.
 * width
 * height
 * length

#### Example

Construct a rainbow-colored road 100 blocks long...

    var rainbowColors = [blocks.wool.red, blocks.wool.orange, blocks.wool.yellow, blocks.wool.lime,
                         blocks.wool.lightblue, blocks.wool.blue, blocks.wool.purple];
    
    boxa(rainbowColors,7,1,30);

![boxa example](img/boxaex1.png)

### Chaining

All of the Drone methods return a Drone object, which means methods can be 'chained' together so instead of writing this...

    drone = new Drone( self ); 
    drone.fwd( 3 );
    drone.left( 2 );
    drone.box( blocks.grass ); // create a grass block 
    drone.up();
    drone.box( blocks.grass ); // create another grass block
    drone.down();

...you could simply write ...
    
    var drone = new Drone(self).fwd(3).left(2).box(blocks.grass).up().box(blocks.grass).down();

... since each Drone method is also a global function that constructs a drone if none is supplied, you can shorten even further to just...
    
    fwd(3).left(2).box(blocks.grass).up().box(blocks.grass).down()

The Drone object uses a [Fluent Interface][fl] to make ScriptCraft scripts more concise and easier to write and read.  Minecraft's in-game command prompt is limited to about 80 characters so chaining drone commands together means more can be done before hitting the command prompt limit. For complex building you should save your commands in a new script file and load it using /js load()

[fl]: http://en.wikipedia.org/wiki/Fluent_interface

### Drone Properties

 * x - The Drone's position along the west-east axis (x increases as you move east)
 * y - The Drone's position along the vertical axis (y increses as you move up)
 * z - The Drone's position along the north-south axis (z increases as you move south)
 * dir - The Drone's direction 0 is east, 1 is south , 2 is west and 3 is north.

### Extending Drone

The Drone object can be easily extended - new buidling recipes/blueprints can be added and can become part of a Drone's chain using the *static* method `Drone.extend`. 

### Drone.extend() static method

Use this method to add new methods (which also become chainable global functions) to the Drone object.

#### Parameters

 * name - The name of the new method e.g. 'pyramid'. 
 * function - The method body.

Alternatively if you provide just a function as a parameter, then the function name will be used as the new method name. For example the following two approaches are both valid.

#### Example 1 Using name and function as parameters

    // submitted by [edonaldson][edonaldson]
    var Drone = require('drone'); 
    Drone.extend('pyramid', function( block,height) { 
        this.chkpt('pyramid');
        for ( var i = height; i > 0; i -= 2) {
            this.box(block, i, 1, i).up().right().fwd();
        }
        return this.move('pyramid');      
    });

#### Example 2 Using just a named function as a parameter

    var Drone = require('drone'); 
    function pyramid( block,height) { 
        this.chkpt('pyramid');
        for ( var i = height; i > 0; i -= 2) {
            this.box(block, i, 1, i).up().right().fwd();
        }
        return this.move('pyramid');      
    }
    Drone.extend( pyramid );

Once the method is defined (it can be defined in a new pyramid.js file) it can be used like so...

    var d = new Drone(self);
    d.pyramid(blocks.brick.stone, 12);

... or simply ...

    pyramid(blocks.brick.stone, 12);

[edonaldson]: https://github.com/edonaldson

### Drone Constants

#### Drone.PLAYER_STAIRS_FACING

An array which can be used when constructing stairs facing in the Drone's direction...

    var d = new Drone(self);
    d.box(blocks.stairs.oak + ':' + Drone.PLAYER_STAIRS_FACING[d.dir]);

... will construct a single oak stair block facing the drone.

#### Drone.PLAYER_SIGN_FACING

An array which can be used when placing signs so they face in a given direction. This is used internally by the Drone.sign() method. It should also be used for placing any of the following blocks...

 * chest 
 * ladder
 * furnace
 * dispenser

By default, chests, dispensers, signs, ladders and furnaces are placed facing towards the drone so to place a chest facing the Drone just use:

    drone.box( blocks.chest );

To place a chest facing _away_ from the Drone:

    drone.box( blocks.chest + ':' + Drone.PLAYER_SIGN_FACING[(drone.dir + 2) % 4]);

#### Drone.PLAYER_TORCH_FACING

Used when placing torches. By default torches will be placed facing up. If you want to place a torch so that it faces towards the drone:

    drone.box( blocks.torch + ':' + Drone.PLAYER_TORCH_FACING[drone.dir]);

If you want to place a torch so it faces _away_ from the drone:

    drone.box( blocks.torch + ':' + Drone.PLAYER_TORCH_FACING[(drone.dir + 2) % 4]);

### Drone.times() Method

The `times()` method makes building multiple copies of buildings
easy. It's possible to create rows or grids of buildings without
resorting to `for` or `while` loops.

#### Parameters

 * numTimes : The number of times you want to repeat the preceding statements.

#### Limitation

For now, don't use `times()` inside a Drone method implementation &ndash; only use it at the in-game prompt as a short-hand workaround for loops.

#### Example

Say you want to do the same thing over and over. You have a couple of options:

 * You can use a `for` loop &hellip;

    d = new Drone(); for ( var i = 0; i < 4; i++ ) {  d.cottage().right(8); }

While this will fit on the in-game prompt, it's awkward. You need to
declare a new Drone object first, then write a `for` loop to create the
4 cottages. It's also error prone &ndash; even the `for` loop is too much
syntax for what should really be simple.

 * You can use a `while` loop &hellip;
   
    d = new Drone(); var i=4; while (i--) { d.cottage().right(8); }

&hellip; which is slightly shorter but still too much syntax. Each of the
above statements is fine for creating a 1-dimensional array of
structures. But what if you want to create a 2-dimensional or
3-dimensional array of structures? Enter the `times()` method.

The `times()` method lets you repeat commands in a chain any number of
times. So to create 4 cottages in a row you would use the following
statement:

    cottage().right(8).times(4);

&hellip; which will build a cottage, then move right 8 blocks, then do it
again 4 times over so that at the end you will have 4 cottages in a
row. What's more, the `times()` method can be called more than once in
a chain. So if you wanted to create a *grid* of 20 houses ( 4 x 5 ),
you would do so using the following statement:

    cottage().right(8).times(4).fwd(8).left(32).times(5);

&hellip; breaking it down &hellip;

 1. The first 3 calls in the chain ( `cottage()`, `right(8)`, `times(4)` ) build a single row of 4 cottages.

 2. The last 3 calls in the chain ( `fwd(8)`, `left(32)`, `times(5)` ) move the drone forward 8 then left 32 blocks (4 x 8) to return to the original X coordinate, then everything in the chain is repeated again 5 times so that in the end, we have a grid of 20 cottages, 4 x 5.  Normally this would require a nested loop but the `times()` method does away with the need for loops when repeating builds.

Another example: This statement creates a row of trees 2 by 3:

    oak().right(10).times(2).left(20).fwd(10).times(3)

&hellip; You can see the results below.

![times example 1](img/times-trees.png)

### Drone.arc() method

The arc() method can be used to create 1 or more 90 degree arcs in the
horizontal or vertical planes. This method is called by cylinder() and
cylinder0() and the sphere() and sphere0() methods.

#### Parameters

arc() takes a single parameter - an object with the following named properties...

 * radius - The radius of the arc.
 * blockType - The type of block to use - this is the block Id only (no meta). See [Data Values][dv].
 * meta - The metadata value. See [Data Values][dv].
 * orientation (default: 'horizontal' ) - the orientation of the arc - can be 'vertical' or 'horizontal'.
 * stack (default: 1 ) - the height or length of the arc (depending on the orientation - if orientation is horizontal then this parameter refers to the height, if vertical then it refers to the length ).
 * strokeWidth (default: 1 ) - the width of the stroke (how many blocks) - if drawing nested arcs it's usually a good idea to set strokeWidth to at least 2 so that there are no gaps between each arc. The arc method uses a [bresenham algorithm][bres] to plot points along the circumference.
 * fill - If true (or present) then the arc will be filled in.
 * quadrants (default: `{topleft:true,topright:true,bottomleft:true,bottomright:true}` - An object with 4 properties indicating which of the 4 quadrants of a circle to draw. If the quadrants property is absent then all 4 quadrants are drawn.

#### Examples

To draw a 1/4 circle (top right quadrant only) with a radius of 10 and
stroke width of 2 blocks ...

    arc({blockType: blocks.iron, 
         meta: 0, 
         radius: 10,
         strokeWidth: 2,
         quadrants: { topright: true },
         orientation: 'vertical', 
         stack: 1,
         fill: false
         } );

![arc example 1](img/arcex1.png)

[bres]: http://en.wikipedia.org/wiki/Midpoint_circle_algorithm
[dv]: http://www.minecraftwiki.net/wiki/Data_values

### Drone.bed() method

Creates a bed. The foot of the bed will be at the drone's location and
the head of the bed will extend away from the drone.

#### Example
To create a bed at the in-game prompt, look at a block then type:

```javascript
/js bed()
```

Like most Drone methods, this returns the drone so it can be chained like so:

```javascript
this
  .fwd(3)
  .bed()
  .back(3)
```     
### Drone.blocktype() method

Creates the text out of blocks. Useful for large-scale in-game signs.

#### Parameters
 
 * message - The message to create - (use `\n` for newlines)
 * foregroundBlock (default: black wool) - The block to use for the foreground
 * backgroundBlock (default: none) - The block to use for the background

#### Example

To create a 2-line high message using glowstone...

    blocktype('Hello\nWorld', blocks.glowstone);

![blocktype example][imgbt1]

[imgbt1]: img/blocktype1.png

### Copy & Paste using Drone

A drone can be used to copy and paste areas of the game world.

#### Deprecated
As of January 10 2015 the copy-paste functions in Drone are no longer
supported. Copy/Paste is:

1. Difficult to do correctly in a way which works for both Minecraft 1.7 and 1.8 
   due to how blocks changed in 1.8
2. Not aligned with the purpose of ScriptCraft's Drone module which is to provide 
   a simple set of functions for scripting and in-game building.

### Drone.copy() method

Copies an area so it can be pasted elsewhere. The name can be used for
pasting the copied area elsewhere...

#### Parameters

 * name - the name to be given to the copied area (used by `paste`)
 * width - the width of the area to copy
 * height - the height of the area to copy
 * length - the length of the area (extending away from the drone) to copy

#### Example

    drone.copy('somethingCool',10,5,10 ).right(12 ).paste('somethingCool' );

### Drone.paste() method

Pastes a copied area to the current location.

#### Example

To copy a 10x5x10 area (using the drone's coordinates as the starting
point) into memory.  the copied area can be referenced using the name
'somethingCool'. The drone moves 12 blocks right then pastes the copy.

    drone.copy('somethingCool',10,5,10 )
         .right(12 )
         .paste('somethingCool' );

### Drone.cylinder() method

A convenience method for building cylinders. Building begins radius blocks to the right and forward.

#### Parameters

 * block - the block id - e.g. 6 for an oak sapling or '6:2' for a birch sapling. Alternatively you can use any one of the `blocks` values e.g. `blocks.sapling.birch`
 * radius 
 * height

#### Example

To create a cylinder of Iron 7 blocks in radius and 1 block high...

    cylinder(blocks.iron, 7 , 1);

![cylinder example](img/cylinderex1.png)

### Drone.cylinder0() method

A version of cylinder that hollows out the middle.

#### Example

To create a hollow cylinder of Iron 7 blocks in radius and 1 block high...

    cylinder0(blocks.iron, 7, 1);

![cylinder0 example](img/cylinder0ex1.png)

### Drone.door() method

create a door - if a parameter is supplied an Iron door is created otherwise a wooden door is created.

#### Parameters

 * doorType (optional - default wood) - If a parameter is provided then the door is Iron.

#### Example

To create a wooden door at the crosshairs/drone's location...

    var drone = new Drone(self);
    drone.door();

To create an iron door...

    drone.door( blocks.door_iron );

![iron door](img/doorex1.png)

### Drone.door_iron() method

create an Iron door.

### Drone.door2() method

Create double doors (left and right side)

#### Parameters

 * doorType (optional - default wood) - If a parameter is provided then the door is Iron.

#### Example

To create double-doors at the cross-hairs/drone's location...

    drone.door2();

![double doors](img/door2ex1.png)

### Drone.door2_iron() method

Create double iron doors

### Drone.firework() method

Launches a firework at the drone's location.

#### Example

To launch a firework:

    var drone = new Drone(self);
    drone.firework();

### Drone.garden() method

places random flowers and long grass (similar to the effect of placing bonemeal on grass)

#### Parameters

 * width - the width of the garden
 * length - how far from the drone the garden extends

#### Example

To create a garden 10 blocks wide by 5 blocks long...

    garden(10,5);

![garden example](img/gardenex1.png)

### Drone.ladder() method

Creates a ladder extending skyward.

#### Parameters

 * height (optional - default 1) 

#### Example

To create a ladder extending 10 blocks high:

    var drone = new Drone(self);
    drone.ladder(10)

At the in-game prompt, look at a block and then type:    

    /js ladder(10)

A ladder 10 blocks high will be created at the point you were looking at.

#### Since 
##### 3.0.3
### Drone Movement

Drones can move freely in minecraft's 3-D world. You control the
Drone's movement using any of the following methods..

 * up()
 * down()
 * left()
 * right()
 * fwd()
 * back()
 * turn()

... Each of these methods takes a single optional parameter
`numBlocks` - the number of blocks to move in the given direction. If
no parameter is given, the default is 1.

To change direction use the `turn()` method which also takes a single
optional parameter (numTurns) - the number of 90 degree turns to
make. Turns are always clock-wise. If the drone is facing north, then
drone.turn() will make the turn face east. If the drone is facing east
then drone.turn(2) will make the drone turn twice so that it is facing
west.

### Drone Positional Info

 * getLocation() - Returns a native Java Location object for the drone

### Drone Markers

Markers are useful when your Drone has to do a lot of work. You can
set a check-point and return to the check-point using the move()
method.  If your drone is about to undertake a lot of work -
e.g. building a road, skyscraper or forest you should set a
check-point before doing so if you want your drone to return to its
current location.

A 'start' checkpoint is automatically created when the Drone is first created.

Markers are created and returned to using the followng two methods...

 * chkpt - Saves the drone's current location so it can be returned to later.
 * move - moves the drone to a saved location. Alternatively you can provide a Java Location object or x,y,z and direction parameters.

#### Parameters

 * name - the name of the checkpoint to save or return to.

#### Example

    drone.chkpt('town-square');
    //
    // the drone can now go off on a long excursion
    //
    for ( i = 0; i< 100; i++) {  
        drone.fwd(12).box(6); 
    }
    //
    // return to the point before the excursion
    //
    drone.move('town-square');

### Drone.prism() method

Creates a prism. This is useful for roofs on houses.

#### Parameters

 * block - the block id - e.g. 6 for an oak sapling or '6:2' for a birch sapling. 
   Alternatively you can use any one of the `blocks` values e.g. `blocks.sapling.birch`
 * width - the width of the prism
 * length - the length of the prism (will be 2 time its height)

#### Example

    prism(blocks.oak,3,12);

![prism example](img/prismex1.png)

### Drone.prism0() method

A variation on `prism` which hollows out the inside of the prism. It
uses the same parameters as `prism`.

### Drone.rand() method

rand takes either an array (if each blockid has the same chance of occurring) or an object where each property is a blockid and the value is it's weight (an integer)

#### Example

place random blocks stone, mossy stone and cracked stone (each block has the same chance of being picked)

    rand( [blocks.brick.stone, blocks.brick.mossy, blocks.brick.cracked ],w,d,h) 

to place random blocks stone has a 50% chance of being picked, 

    var distribution = {};
    distribution[ blocks.brick.stone ] = 5;
    distribution[ blocks.brick.mossy ] = 3;
    distribution[ blocks.brick.cracked ] = 2;

    rand( distribution, width, height, depth) 

regular stone has a 50% chance, mossy stone has a 30% chance and cracked stone has just a 20% chance of being picked.

### Drone.wallsign() method

Creates a wall sign (A sign attached to a wall)

#### Parameters

 * message - can be a string or an array of strings

#### Example

    drone.wallsign(['Welcome','to','Scriptopia']);

![wall sign](img/signex2.png)

### Drone.signpost() method

Creates a free-standing signpost 

#### Parameters

 * message - can be a string or an array of strings

#### Example

    drone.signpost(['Hello','World']);

![ground sign](img/signex1.png)

### Drone.sign() method

Deprecated: Use signpost() or wallsign() methods instead.

Signs must use block 63 (stand-alone signs) or 68 (signs on walls)

#### Parameters

 * message -  can be a string or an array of strings. 
 * block - can be 63 or 68

#### Example

To create a free-standing sign...

    drone.sign(["Hello","World"], blocks.sign_post);

![ground sign](img/signex1.png)

... to create a wall mounted sign...

    drone.sign(["Welcome","to","Scriptopia"], blocks.sign );

![wall sign](img/signex2.png)

### Drone.sphere() method

Creates a sphere.

#### Parameters
 
 * block - The block the sphere will be made of.
 * radius - The radius of the sphere.

#### Example

To create a sphere of Iron with a radius of 10 blocks...

    sphere( blocks.iron, 10);

![sphere example](img/sphereex1.png)

Spheres are time-consuming to make. You *can* make large spheres (250 radius) but expect the
server to be very busy for a couple of minutes while doing so.

### Drone.sphere0() method

Creates an empty sphere.

#### Parameters
 
 * block - The block the sphere will be made of.
 * radius - The radius of the sphere.

#### Example

To create a sphere of Iron with a radius of 10 blocks...

    sphere0( blocks.iron, 10);

Spheres are time-consuming to make. You *can* make large spheres (250 radius) but expect the
server to be very busy for a couple of minutes while doing so.

### Drone.hemisphere() method

Creates a hemisphere. Hemispheres can be either north or south.

#### Parameters

 * block - the block the hemisphere will be made of.
 * radius - the radius of the hemisphere
 * northSouth - whether the hemisphere is 'north' or 'south'

#### Example

To create a wood 'north' hemisphere with a radius of 7 blocks...

    hemisphere(blocks.oak, 7, 'north');

![hemisphere example](img/hemisphereex1.png)

### Drone.hemisphere0() method

Creates a hollow hemisphere. Hemispheres can be either north or south.

#### Parameters

 * block - the block the hemisphere will be made of.
 * radius - the radius of the hemisphere
 * northSouth - whether the hemisphere is 'north' or 'south'

#### Example

To create a glass 'north' hemisphere with a radius of 20 blocks...

    hemisphere0(blocks.glass, 20, 'north');

![hemisphere example](img/hemisphereex2.png)

### Drone.stairs() function

The stairs() function will build a flight of stairs

#### Parameters

 * blockType - should be one of the following: 

   * blocks.stairs.oak
   * blocks.stairs.cobblestone
   * blocks.stairs.brick
   * blocks.stairs.stone
   * blocks.stairs.nether
   * blocks.stairs.sandstone
   * blocks.stairs.spruce
   * blocks.stairs.birch
   * blocks.stairs.jungle
   * blocks.stairs.quartz

 * width - The width of the staircase - default is 1
 * height - The height of the staircase - default is 1

#### Example

To build an oak staircase 3 blocks wide and 5 blocks tall:

    /js stairs(blocks.stairs.oak, 3, 5) 

Staircases do not have any blocks beneath them.

### Drone Trees methods

 * oak()
 * spruce()
 * birch()
 * jungle()

#### Example

To create 4 trees in a row, point the cross-hairs at the ground then type `/js ` and ...

    up( ).oak( ).right(8 ).spruce( ).right(8 ).birch( ).right(8 ).jungle( );

Trees won't always generate unless the conditions are right. You
should use the tree methods when the drone is directly above the
ground. Trees will usually grow if the drone's current location is
occupied by Air and is directly above an area of grass (That is why
the `up()` method is called first).

![tree example](img/treeex1.png)

None of the tree methods require parameters. Tree methods will only be
successful if the tree is placed on grass in a setting where trees can
grow.

### Drone.castle() method

Creates a Castle. A castle is just a big wide fort with 4 taller forts at each corner. 
See also Drone.fort() method.

#### Parameters
 
 * side - How many blocks wide and long the castle will be (default: 24. Must be greater than 19)
 * height - How tall the castle will be (default: 10. Must be geater than 7)

#### Example

At the in-game prompt you can create a castle by looking at a block and typing:

```javascript
/js castle()
```

Alternatively you can create a new Drone object from a Player or Location object and call the castle() method.

```javascript
var d = new Drone(player);
d.castle();
```
![castle example](img/castleex1.png)

### Drone.chessboard() method

Creates a tile pattern of given block types and size

#### Parameters

 * whiteBlock - (optional: default blocks.wool.white)
 * blackBlock - (optional: default blocks.wool.black)
 * width - width of the chessboard
 * length - length of the chessboard

#### Example

At the in-game prompt you can create a chessboard by looking at a block and typing:

```javascript
/js chessboard()
```

Alternatively you can create a new Drone object from a Player or Location object and call the chessboard() method.

```javascript
var d = new Drone(player);
d.chessboard();
```
![chessboard example](img/chessboardex1.png)

### Drone.cottage() method

Creates a simple but cosy dwelling.

#### Example

At the in-game prompt you can create a cottage by looking at a block and typing:

```javascript
/js cottage()
```

Alternatively you can create a new Drone object from a Player or Location object and call the cottage() method.

```javascript
var d = new Drone(player);
d.cottage();
```
![cottage example](img/cottageex1.png)

### Drone.cottage_road() method

Creates a tree-lined avenue with cottages on both sides.

#### Parameters
 
 * numberOfCottages: The number of cottages to build in total (optional: default 6)

#### Example

At the in-game prompt you can create a cottage road by looking at a block and typing:

```javascript
/js cottage_road()
```

Alternatively you can create a new Drone object from a Player or Location object and call the cottage_road() method.

```javascript
var d = new Drone(player);
d.cottage_road();
```
![cottage_road example](img/cottageroadex1.png)

### Drone.dancefloor() method
Create an animated dance floor of colored tiles some of which emit light.
The tiles change color every second creating a strobe-lit dance-floor effect.
See it in action here [http://www.youtube.com/watch?v=UEooBt6NTFo][ytdance]

#### Parameters 

 * width - how wide the dancefloor should be (optional: default 5)
 * length - how long the dancefloor should be (optional: default 5)
 * duration - the time duration for which the lights should change (optional: default 30 seconds)

#### Example

At the in-game prompt you can create a dancefloor by looking at a block and typing:

```javascript
/js dancefloor()
```

Alternatively you can create a new Drone object from a Player or Location object and call the dancefloor() method.

```javascript
var d = new Drone(player);
d.dancefloor();
```

[ytdance]: http://www.youtube.com/watch?v=UEooBt6NTFo
![dancefloor example](img/dancefloorex1.png)
### Drone.fort() method

Constructs a medieval fort.

#### Parameters
 
 * side - How many blocks whide and long the fort will be (default: 18 . Must be greater than 9)
 * height - How tall the fort will be (default: 6 . Must be greater than 3)

#### Example

At the in-game prompt you can create a fort by looking at a block and typing:

```javascript
/js fort()
```

Alternatively you can create a new Drone object from a Player or Location object and call the fort() method.

```javascript
var d = new Drone(player);
d.fort();
```
![fort example](img/fortex1.png)

### Drone.hangtorch() method

Adds a hanging torch to a wall. This method will try to hang a torch
against a wall. It will traverse backwards until it finds a block
adjacent to air and hang the torch. If it can't find a block next to
air it will log a message in the server.

#### Example

At the in-game prompt you can create a hanging torch by looking at a
block and typing:

```javascript
/js hangtorch()
```

Alternatively you can create a new Drone object from a Player or
Location object and call the hangtorch() method.

```javascript
var d = new Drone(player);
d.hangtorch();
```

### Drone.lcdclock() method.

Constructs a large LCD Clock. The clock will display the current time of day.
The clock can be stopped by calling the stopLCD() method of the Drone which created the clock.

#### Parameters

 * foregroundBlock (Optional - default is blocks.glowstone)
 * backgroundBlock (Optional - default is blocks.wool.black)
 * borderBlock (Optional - a border around the LCD display - default none)

#### Example

At the in-game prompt you can create a LCD clock by looking at a block and typing:

```javascript
/js var clock = lcdclock()
/js clock.stopLCD()
```

Alternatively you can create a new Drone object from a Player or Location object and call the lcdclock() method.

```javascript
var d = new Drone(player);
d.lcdclock();
d.stopLCD();
```
![lcdclock example](img/lcdclockex1.png)
### Drone.logojs() method

Constructs a large Javascript Logo (black JS on Yellow background)
See: https://raw.github.com/voodootikigod/logo.js/master/js.png

#### Parameters

 * foregroundBlock (Optional - default is blocks.wool.gray)
 * backgroundBlock (Optional - default is blocks.gold)

### Drone.maze() method

Maze generation based on http://rosettacode.org/wiki/Maze_generation#JavaScript

#### Parameters

 * width (optional - default 10)
 * length (optional - default 10)

#### Example

At the in-game prompt you can create a maze by looking at a block and typing:

```javascript
/js maze()
```

Alternatively you can create a new Drone object from a Player or Location object and call the maze() method.

```javascript
var d = new Drone(player);
d.maze();
```
![maze example](img/mazeex1.png)

### Drone.rainbow() method

Creates a Rainbow.

#### Parameters

 * radius (optional - default:18) - The radius of the rainbow

#### Example

At the in-game prompt you can create a rainbow by looking at a block and typing:
```javascript
/js rainbow()
```

Alternatively you can create a new Drone object from a Player or Location object and call the rainbow() method.

```javascript    
var d = new Drone(player);
d.rainbow(30);
```

![rainbow example](img/rainbowex1.png)

### Drone.spiral_stairs() method

Constructs a spiral staircase with slabs at each corner.

#### Parameters

 * stairBlock - The block to use for stairs, should be one of the following...
   - 'oak'
   - 'spruce'
   - 'birch'
   - 'jungle'
   - 'cobblestone'
   - 'brick'
   - 'stone'
   - 'nether'
   - 'sandstone'
   - 'quartz'
 * flights - The number of flights of stairs to build.

![Spiral Staircase](img/spiralstair1.png)

#### Example

To construct a spiral staircase 5 floors high made of oak...

    spiral_stairs('oak', 5);

### Drone.temple() method

Constructs a mayan temple.

#### Parameters
 
 * side - How many blocks wide and long the temple will be (default: 20)

#### Example

At the in-game prompt you can create a temple by looking at a block and typing:

```javascript
/js temple()
```

Alternatively you can create a new Drone object from a Player or Location object and call the temple() method.

```javascript
var d = new Drone(player);
d.temple();
```
![temple example](img/templeex1.png)

## Utilities Module

The `utils` module is a storehouse for various useful utility
functions which can be used by plugin and module authors. It contains
miscellaneous utility functions and classes to help with programming.

## Utility Methods

 * [utils.player() function](#utilsplayer-function)
 * [utils.world( worldName ) function](#utilsworld-worldname--function)
 * [utils.blockAt( Location ) function](#utilsblockat-location--function)
 * [utils.locationToJSON() function](#utilslocationtojson-function)
 * [utils.locationToString() function](#utilslocationtostring-function)
 * [utils.locationFromJSON() function](#utilslocationfromjson-function)
 * [utils.getPlayerPos() function](#utilsgetplayerpos-function)
 * [utils.getMousePos() function](#utilsgetmousepos-function)
 * [utils.foreach() function](#utilsforeach-function)
 * [utils.nicely() function](#utilsnicely-function)
 * [utils.time( world ) function](#utilstime-world--function)
 * [utils.time24( world ) function](#utilstime24-world--function)
 * [utils.find() function](#utilsfind-function)
 * [utils.serverAddress() function](#utilsserveraddress-function)
 * [utils.array() function](#utilsarray-function)
 * [utils.players() function](#utilsplayers-function)
 * [utils.playerNames() function](#utilsplayernames-function)

### utils.player() function

The utils.player() function will return a [Player][cmpl] object
with the given name. This function takes a single parameter
`playerName` which can be either a String or a [Player][cmpl] object -
if it's a Player object, then the same object is returned. If it's a
String, then it tries to find the player with that name.

#### Parameters

 * playerName : A String or Player object. If no parameter is provided
   then player() will try to return the `self` variable . It is 
   strongly recommended to provide a parameter.

#### Example

```javascript
var utils = require('utils');
var name = 'walterh';
var player = utils.player(name);
if ( player ) {
  echo(player, 'Got ' + name);
} else {
  console.log('No player named ' + name);
}
```

[bkpl]: http://jd.bukkit.org/dev/apidocs/org/bukkit/entity/Player.html
[cmpl]: https://ci.visualillusionsent.net/job/CanaryLib/javadoc/net/canarymod/api/entity/living/humanoid/Player.html
[cmloc]: https://ci.visualillusionsent.net/job/CanaryLib/javadoc/net/canarymod/api/world/position/Location.html 
[bkloc]: http://jd.bukkit.org/dev/apidocs/org/bukkit/Location.html

### utils.world( worldName ) function

Returns a World object matching the given name

### utils.blockAt( Location ) function

Returns the Block at the given location.

### utils.locationToJSON() function

utils.locationToJSON() returns a [Location][cmloc] object in JSON form...

    { world: 'world5',
      x: 56.9324,
      y: 103.9954,
      z: 43.1323,
      yaw: 0.0,
      pitch: 0.0
    }

This can be useful if you write a plugin that needs to store location data since bukkit's Location object is a Java object which cannot be serialized to JSON by default.

#### Parameters
 
 * location: An object of type [Location][cmloc]

#### Returns

A JSON object in the above form.
 
### utils.locationToString() function

The utils.locationToString() function returns a
[Location][cmloc] object in string form...

    '{"world":"world5",x:56.9324,y:103.9954,z:43.1323,yaw:0.0,pitch:0.0}'

... which can be useful if you write a plugin which uses Locations as
keys in a lookup table.

#### Example

```javascript    
var utils = require('utils');
...
var key = utils.locationToString(player.location);
lookupTable[key] = player.name;
```

### utils.locationFromJSON() function

This function reconstructs an [Location][cmloc] object from
a JSON representation. This is the counterpart to the
`locationToJSON()` function. It takes a JSON object of the form
returned by locationToJSON() and reconstructs and returns a bukkit
Location object.

### utils.getPlayerPos() function

This function returns the player's [Location][cmloc] (x, y, z, pitch
and yaw) for a named player.  If the "player" is in fact a
[BlockCommand][bkbcs] then the attached Block's location is returned.

#### Parameters

 * player : A [org.bukkit.command.CommandSender][bkbcs] (Player or BlockCommandSender) or player name (String).

#### Returns

A [Location][cmloc] object.

[bkbcs]: http://jd.bukkit.org/dev/apidocs/org/bukkit/command/BlockCommandSender.html
[bksndr]: http://jd.bukkit.org/dev/apidocs/index.html?org/bukkit/command/CommandSender.html
### utils.getMousePos() function

This function returns a [Location][cmloc] object (the
x,y,z) of the current block being targeted by the named player. This
is the location of the block the player is looking at (targeting).

#### Parameters

 * player : The player whose targeted location you wish to get.

#### Example

The following code will strike lightning at the location the player is looking at...

```javascript
var utils = require('utils');
var playerName = 'walterh';
var targetPos = utils.getMousePos(playerName);
if (targetPos){
  if (__plugin.canary){
    targetPos.world.makeLightningBolt(targetPos);
  }  
  if (__plugin.bukkit){ 
    targetPos.world.strikeLightning(targetPos);
  }
}
```

### utils.foreach() function

The utils.foreach() function is a utility function for iterating over
an array of objects (or a java.util.Collection of objects) and
processing each object in turn. Where utils.foreach() differs from
other similar functions found in javascript libraries, is that
utils.foreach can process the array immediately or can process it
*nicely* by processing one item at a time then delaying processing of
the next item for a given number of server ticks (there are 20 ticks
per second on the minecraft main thread).  This method relies on
Bukkit's [org.bukkit.scheduler][sched] package for scheduling
processing of arrays.

[sched]: http://jd.bukkit.org/beta/apidocs/org/bukkit/scheduler/package-summary.html

#### Parameters

 * array : The array to be processed - It can be a javascript array, a java array or java.util.Collection
 * callback : The function to be called to process each item in the
   array. The callback function should have the following signature
   `callback(item, index, object, array)`. That is the callback will
   be called with the following parameters....

   - item : The item in the array
   - index : The index at which the item can be found in the array.
   - object : Additional (optional) information passed into the foreach method.
   - array : The entire array.

 * context (optional) : An object which may be used by the callback.
 * delayInMilliseconds (optional, numeric) : If a delay is specified then the processing will be scheduled so that
   each item will be processed in turn with a delay between the completion of each
   item and the start of the next. This is recommended for any CPU-intensive process.
 * onDone (optional, function) : A function to be executed when all processing 
   is complete. This parameter is only used when the processing is delayed. (It's optional even if a 
   delay parameter is supplied).

If called with a delay parameter then foreach() will return
immediately after processing just the first item in the array (all
subsequent items are processed later). If your code relies on the
completion of the array processing, then provide an `onDone` parameter
and put the code there.

#### Example

The following example illustrates how to use foreach for immediate processing of an array...

```javascript
var utils = require('utils');
var players = utils.players();
utils.foreach (players, function( player ) { 
  echo( player , 'Hi ' + player);
});
```

... The `utils.foreach()` function can work with Arrays or any
Java-style collection. This is important because many objects in the
CanaryMod and Bukkit APIs use Java-style collections.
### utils.nicely() function

The utils.nicely() function is for performing background processing. utils.nicely() lets you
process with a specified delay between the completion of each `next()`
function and the start of the next `next()` function.
`utils.nicely()` is a recursive function - that is - it calls itself
(schedules itself actually) repeatedly until `hasNext` returns false.

#### Parameters

 * next : A function which will be called if processing is to be done. 
 * hasNext : A function which is called to determine if the `next`
   callback should be invoked. This should return a boolean value -
   true if the `next` function should be called (processing is not
   complete), false otherwise.
 * onDone : A function which is to be called when all processing is complete (hasNext returned false).
 * delayInMilliseconds : The delay between each call.

#### Example

See the source code to utils.foreach for an example of how utils.nicely is used.

### utils.time( world ) function

Returns the timeofday (in minecraft ticks) for the given world. This function is necessary because
canarymod and bukkit differ in how the timeofday is calculated. 

See http://minecraft.gamepedia.com/Day-night_cycle#Conversions

### utils.time24( world ) function

Returns the timeofday for the given world using 24 hour notation. (number of minutes)

See http://minecraft.gamepedia.com/Day-night_cycle#Conversions

#### Parameters

 * world : the name of the world or world object for which you want to get time

### utils.find() function

The utils.find() function will return a list of all files starting at
a given directory and recursiving trawling all sub-directories.

#### Parameters

 * dir : The starting path. Must be a string.
 * filter : (optional) A [FilenameFilter][fnfltr] object to return only files matching a given pattern.

[fnfltr]: http://docs.oracle.com/javase/6/docs/api/java/io/FilenameFilter.html

#### Example

```javascript
var utils = require('utils');
var jsFiles = utils.find('./', function(dir,name){
    return name.match(/\.js$/);
});  
```
### utils.serverAddress() function

The utils.serverAddress() function returns the IP(v4) address of the server.

```javascript
var utils = require('utils');
var serverAddress = utils.serverAddress();
console.log(serverAddress);
```
### utils.array() function

Converts Java collection objects to type Javascript array so they can avail of
all of Javascript's Array goodness.
 
#### Example

    var utils = require('utils');
    var worlds = utils.array(server.worldManager.getAllWorlds());
    
### utils.players() function

This function returns a javascript array of all online players on the
server.  You can optionally provide a function which will be invoked
with each player as a parameter.  For example, to give each player the
ability to shoot arrows which launch fireworks:

```javascript
require('utils').players( arrows.firework )
```

Any players with a bow will be able to launch fireworks by shooting.

### utils.playerNames() function

This function returns a javascript array of player names (as javascript strings)


## Using Spigot JavaDocs

[Spigot JavaDocs](https://hub.spigotmc.org/javadocs/spigot/)

You will quickly find that not everything can be accomplished without looking at the Spigot JavaDocs. For example, to fully understand the possiblities associated with a certain event you must look at the methods that are available for that type of event in Spigot.

Let's say you want to make a plugin that sends a player a message when they hit a block with an arrow telling them what type of block they hit. How would you start? Well, the first thing you need to do is find an event that will fire when the player hits something with an arrow. A quick search of the list of possible events should lead you to the ProjectileHit event:

![ProjectileHitEvent](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/564/original/projectilehitlink.png)

Clicking on this link will take you to the Spigot JavaDocs for the event

Before we continue with the JavaDocs for the ProjectileHitEvent, let's start writing some code:

```javascript
var onProjectileHit = function(event) {
	// executed when a projectile hits something
}
events.projectileHit(onProjectileHit);
```

This code will register the function `onProjectileHit` to be called when a ProjectileHitEvent is fired. As it is written here nothing will happen, but any code you write in place of the comment will be executed whenenever a projectile hits something.

On the ProjectileHitEvent page you will find a section labeled "Method Summary" containing the methods that can be called on a ProjectileHitEvent. Keep in mind that many methods will also be inherited by parent classes. Inherited methods will appear below the class-specific methods within the method summary section.

![ProjectileHitEvent Method Summary](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/565/original/projectilehitmethods.png)

In this example, we are interested in the `getEntity()` method that returns the Entity involved in this event. In this case we know the Entity will be a Projectile (the Projectile class extends the Entity class) because it triggered the ProjectileHitEvent.

We could write `event.getEntity()` to return the Projectile that caused the event to be fired, or we could use the cleaner syntax `event.entity` to accomplish the same thing (thanks to Rhino).

When using Rhino JavaScript there is a special syntax available for methods that begin with "get" or "set". If you are trying access a property using a method such as `event.getEntity()` you can access that property using the syntax `event.entity`. If you are trying to set the value of a property using a method such as `event.setCancelled(true)` you can use the syntax `event.cancelled = true`.

```javascript
var onProjectileHit = function(event) {
	var projectile = event.entity;
}
events.projectileHit(onProjectileHit);
```

Now we have a variable `projectile` that stores the Projectile that caused the ProjectileHitEvent to fire. If we want to get the Entity that fired the projectile, we will have to find the Method Summary in the JavaDocs for the Projectile interface:

![Projectile Interface](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/566/original/projectilemethods.png)

To get the player that fired the projectile we can use the `Projectile.getShooter()` method as shown here using Rhino syntax:

```javascript
var onProjectileHit = function(event) {
	var projectile = event.entity;
	var shooter = projectile.shooter;
}
events.projectileHit(onProjectileHit);
```

We now have the `shooter` variable storing the player that shot the projectile that caused the ProjectileHitEvent. Now that we have a way to reference the player that fired the projectile, we have the ability to send them a message. We will use the `echo(player, message)` function to do this. This code will send the message "You hit something!" to the player, but it won't yet tell them the specific thing they hit.

```javascript
var onProjectileHit = function(event) {
	var projectile = event.entity;
	var shooter = projectile.shooter;
	echo(shooter, "You hit something!");
}
events.projectileHit(onProjectileHit);
```

To find the object in the world that was hit by the projectile, we must refer again to the JavaDocs for the Projectile interface to see what methods are available to us. Looking through the list of methods in the method summary we find that the method `Projectile.getLocation()` is available as it is inherited from the Entity class. This will only get us the location of the projectile, but it is a necessary step towards finding what it collided with:

```javascript
var onProjectileHit = function(event) {
	var projectile = event.entity;
	var shooter = projectile.shooter;
	var projectileLocation = projectile.location;
	echo(shooter, "You hit something!");
}
events.projectileHit(onProjectileHit);
```

Now that we have the Location of the projectile, we must look at the [JavaDocs for the Location class](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/Location.html) to see what methods can be used on it. After a quick look at the method summary we can see that there is a method `Location.getBlock()` which returns the Block at the given Location. 

It is important to note here that this is not the only option. There are methods we could use that take a Location as a parameter such as `World.getBlockAt(Location loc)` which would accomplish the same thing, but require us to create a reference to the current World. Generally speaking if you do not see a method in the class you are working with that accomplishes what you need you can google something like "how to get block from location spigot" to find the proper method to use.

In this example we have the method we need though, so we can get the Block that was hit from the Location of the projectile using `projectileLocation.block`:

```javascript
var onProjectileHit = function(event) {
	var projectile = event.entity;
	var shooter = projectile.shooter;
	var projectileLocation = projectile.location;
	var hitBlock = projectileLocation.block;
	echo(shooter, "You hit something!");
}
events.projectileHit(onProjectileHit);
```

Now we simply need to get the type of the Block that was hit. A quick look at the [JavaDocs for the Block class](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/block/Block.html) will reveal the `Block.getType()` method that returns the Material of the Block.

```javascript
var onProjectileHit = function(event) {
	var projectile = event.entity;
	var shooter = projectile.shooter;
	var projectileLocation = projectile.location;
	var hitBlock = projectileLocation.block;
	var blockType = hitBlock.type;
	echo(shooter, "You hit " + blockType);
}
events.projectileHit(onProjectileHit);
```

Let's test it out and see what happens:

![Hit Air](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/567/original/hitair.png)

Oops! It says we hit AIR! This is because it is getting the block at the precise location of the arrow, which is always going to be inside an AIR block since it stops when it collides with a solid block.

To get the block that was actually struck by the arrow we will need to take into account its velocity. We can get the velocity of the projectile using `projectile.velocity`. Then we need to add the velocity of the arrow to the arrow's current location. To do this we will use the `Location.add(x, y, z)` method. Our updated code will look like this:

```javascript
var onProjectileHit = function(event) {
	var projectile = event.entity;
	var shooter = projectile.shooter;
	var projectileLocation = projectile.location;
	var projectileVelocity = projectile.velocity;
	var hitBlockLoc = projectileLocation.add(projectileVelocity.x, projectileVelocity.y, projectileVelocity.z);
	var hitBlock = hitBlockLoc.block;
	var blockType = hitBlock.type;
	echo(shooter, "You hit " + blockType);
}
events.projectileHit(onProjectileHit);
```

The result:

![Finished Plugin Demo](http://d14nx13ylsx7x8.cloudfront.net/comfy/cms/files/files/000/000/568/original/testingdemoplugin.png)
