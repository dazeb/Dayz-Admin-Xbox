# Dayz-Admin-Xbox
An admin guide for DayZ on Xbox x/s

Here is the guide converted to markdown format for improved readability:

# How to code your DayZ files

## Introduction

The purpose of this tutorial is to get you started in coding DayZ with some basic knowledge before you dive in.

A Basic Understanding Of The Most Common Used Files And What They Are Used For.

| Root Folder | Definition |
|-------------|------------|
| mapgroupproto.xml | Holds each building model with loot spawn points and loot totals for said building. |
| cfgeventspawns.xml | Coordinates on your spawn points for events. |
| cfglimitsdefinition.xml | Holds the names of Categories and Usage Tags that are used with items & buildings for possible loot drops. |
| cfgplayerspawnpoints.xml | Selects where your character will spawn in the map. |
| cfgrandompresets.xml | Holds Preset information. Mainly used for items that can be found on Zombies. |
| cfgspawnabletypes.xml | Keeps information on what items are attached and or contained within another item. |
| mapgrouppos.xml | All buildings in the map that can spawn loot are listed here as well as their coordinates. |

| DB Folder | Definition |
|-----------|------------|
| types.xml | A List of all items in game and the stats for said items. Also controls what will go into the loot table. |
| economy.xml | Controls if a variable should be saved, loaded, spawned etc. |
| events.xml | Allows you to spawn in objects, items, buildings etc. |
| globals.xml | Controls values on a global scale effecting all. |
| messages.xml | Allows you to place text messages while the game is running. |

| Env Folder | Definition |
|------------|------------|
| bear_territories.xml | Controls the animal spawn points and how many will be in that area. |
| cattle_territories.xml | Controls the animal spawn points and how many will be in that area. |
| domestic_animals_territories.xml | Controls the animal spawn points and how many will be in that area. |
| hare_territories.xml | Controls the animal spawn points and how many will be in that area. |
| hen_territories.xml | Controls the animal spawn points and how many will be in that area. |
| pig_territories.xml | Controls the animal spawn points and how many will be in that area. |
| red_deer_territories.xml | Controls the animal spawn points and how many will be in that area. |
| roe_deer_territories.xml | Controls the animal spawn points and how many will be in that area. |
| sheep_goat_territories.xml | Controls the animal spawn points and how many will be in that area. |
| wild_boar_territories.xml | Controls the animal spawn points and how many will be in that area. |
| wolf_territories.xml | Controls the animal spawn points and how many will be in that area. |
| zombie_territories.xml | Controls the zombie spawn points and how many will be in that area. |

## Helpful Tools

If you do not know where or how to get coordinates like X,Z I would suggest using the iZurvive DayZ Map Tool. 
It can be found here: https://www.izurvive.com/

A good editor program would be Notepad++. It can be found here: https://notepad-plus-plus.org/downloads/
I would also install the validator and compare features. You can get info on how to do this from our Tools section.

If you are looking for a building, but do not know the name I would suggest using DayZ Interactive Loot Map which can be found here: https://dayz.xam.nu/

## Getting Started, Making Events

The first thing most people want to do is create events. So in this chapter we will go over the basics for doing this.

To create an event there are three main areas you will want to go over. 
The first is the spawn point (place you want the event to appear at).
This can be found in cfgeventspawn.xml
You will need to name your event so you can call it later. All event names need to start with one of the following:

- Vehicle - any and all cars fit into this type
- Static - any and all buildings found in mapgroupproto.xml
- Loot - any and all items found in types.xml
- Item - any and all items found in types.xml
- Infected - Zombies fit into this type
- Animal - any and all animals

Example:
```xml
<event name="StaticBuilding1">
  <pos x="11955.0" z="12550.0" a="113.000000" />
</event>
```

In the pos line you see three coordinates: X, Z, and A
The X & Z coordinates can be obtained from programs like iZurvive.
The A coordinate is for rotation. This works on a 0-360 scale.
It is a good practice to have 6 digits after the decimal point to avoid the building from randomly rotating on restarts.

You can have multiple spawn points by simply adding more pos lines if you want the same object in more than one area.

What do you want to spawn in? For that you can look in mapgroupproto.xml which lists all the buildings and wrecks in the game. Or maybe you want to spawn in an item, for that we look in types.xml which lists all the loot and items in the game. To find the class name of the object, look at the beginning of each entry. You will see `name=`, the name after that is the class name and that is the name you will use in your actual event as shown below on the `child` line where it says `type=`.

Now for the actual event. For this we will need to open up the events.xml. Remember the custom name we set above, that will need to be used here so the two will work together.

For this example we will be spawning in a lamp post, you can of course use any building.

```xml
<event name="StaticBuilding1">
  <nominal>1</nominal>
  <min>1</min>
  <max>1</max>
  <lifetime>3888000</lifetime>
  <restock>0</restock>
  <saferadius>0</saferadius>
  <distanceradius>0</distanceradius>
  <cleanupradius>100</cleanupradius>
  <flags deletable="0" init_random="0" remove_damaged="0"/>
  <position>fixed</position>
  <limit>mixed</limit>
  <active>1</active>
  <children>
    <child lootmax="0" lootmin="0" max="1" min="1" type="Land_Power_Pole_Conc1_Amp"/>
  </children>
</event>
```

For definitions of each of the tags used above, see the [Definitions](#definitions) section.

## Spawning Items Anywhere

This is a little trick you can use to spawn your items in the air. You can even make it so it looks like the item is on a wall for example. To do this we will need to use a few files.

The first is mapgrouppos.xml. This file is used to set the coordinates for the item(s).
You will need to use a custom name so it can be called later. For this example, we will use `Gun1` as we will be spawning in an M4A1.

Example Using 1 Weapon:
```xml
<group name="Gun1" pos="12156.0 140.400000 12627.00" rpy="-0.000000 0.000000 0.000000" a="0.0" />
```

Example Using 2 Weapons:
```xml
<group name="Gun1" pos="12156.0 140.400000 12627.00" rpy="-0.000000 0.000000 0.000000" a="0.0" />
```

As you can see, we have many coordinates listed. Here is a breakdown of each one:

- `pos` cords: 3 sets of numbers
  - 1st = X cord
  - 2nd = Elevation
  - 3rd = Z cord

If you need to find the Elevation, there are 2 good ways you can do this:
1. Have a friend hit you.
2. Kill yourself in-game.

Both of these will show in your logs, which take about 5 minutes to register. The order of the coordinates in the logs is like this: X, Z, E.

- `rpy` cords: 3 sets of numbers
  - 1st = roll angle
  - 2nd = pitch angle
  - 3rd = yaw angle

After you have set your coordinates, you can test to see if you need to alter it a bit to better fit what you are going for. But before we can test, we still need to add one more bit of code.

Open up the mapgroupproto.xml. Remember the custom name we used above, that will be used here as well so the two can work together.

Example Using 1 Weapon:
```xml
<group name="Gun1" lootmax="1">
  <usage name="Military" />
  <container name="loot" lootmax="1">
    <category name="weapons"/>
  </container>
  <dispatch>
    <proxy type="M4A1" pos="-1.0 2.0 1.5" rpy="-225.0 90.0 0.0" />
  </dispatch>
</group>
```

Example Using 2 Weapons:
```xml
<group name="Gun1" lootmax="2">
  <usage name="Military" />
  <container name="loot" lootmax="2">
    <category name="weapons"/>
  </container>
  <dispatch>
    <proxy type="M4A1" pos="-1.0 2.0 1.5" rpy="-225.0 90.0 0.0" />
    <proxy type="M4A1" pos="1.0 2.0 1.5" rpy="-225.0 90.0 0.0" />
  </dispatch>
</group>
```

Notice in the 2nd example we increased `lootmax` to 2 and in the dispatch area we now have 2 `proxy` lines.
The `lootmax` number needs to match the total of `proxy` lines (1 for each item to spawn).
In the `pos` coordinates, we set the second weapon off a bit from the first so it spawns next to the first weapon.
Using this method you could use many items. Set the `lootmax` to the total number of `proxy` lines etc.

By adjusting the Elevation coordinate you can make the item higher or lower, using the X and Z coordinates you can adjust the position to get it to fit just how you want.

If you use any item from types.xml, be sure to look to see what category and usage tag it is set to. The tags need to be listed above to make the match for the item(s) in question.

Using this method we can use pretty much anything from types.xml. However, this method does not work with buildings.

## Changing - Adding Animal Spawns

There are two basic methods for spawning in animals. One is to have them spawn in without AI, thus making them more like a pet. This can be used in a premade Zoo, just for aesthetics, or maybe just for a scare tactic. The other method uses the AI and thus will attack players if able.
The class names for animals can be found in types.xml.
For this example, we will be using a Bear.

For pet animals (No AI):

```xml
<!-- cfgeventspawns.xml New Entry -->
<event name="ItemAnimal1">
  <pos x="3872.40" z="10962.24" a="180.0" />
</event>
```
Replace the X and Z cords to an area you wish to see the bear at.

```xml
<!-- events.xml New Entry -->
<event name="ItemAnimal1">
  <nominal>1</nominal>
  <min>1</min>
  <max>1</max>
  <lifetime>180</lifetime>
  <restock>0</restock>
  <saferadius>200</saferadius>
  <distanceradius>0</distanceradius>
  <cleanupradius>0</cleanupradius>
  <flags deletable="0" init_random="0" remove_damaged="1"/>
  <position>fixed</position>
  <limit>custom</limit>
  <active>1</active>
  <children>
    <child lootmax="0" lootmin="0" max="1" min="1" type="Animal_UrsusArctos"/>
  </children>
</event>
```

For animals that will attack or at least use the AI:

It seems a territory section is randomly chosen, and if it's chosen, then min and max is considered.

So put this line in every territory section of the bear_territories.xml (or whichever animal you are using), and it spawns every time.
Replace the X and Z cords to an area you wish to see the bear at.

```xml
<!-- bear_territories.xml New Entry -->
<zone name="Graze" smin="0" smax="0" dmin="1" dmax="1" x="5316.82" z="9818.90" r="1"/>
```

If using another type of animal, simply open up the territory file for that animal instead.

## Changing - Adding Zombie Spawns

Zombie Hordes in zombie_territories.xml

There are 10 types of zombies. They are:

- InfectedReligious
- InfectedFirefighter
- InfectedArmy
- InfectedPolice
- InfectedSolitude
- InfectedPrisoner
- InfectedIndustrial
- InfectedVillage
- InfectedMedic
- InfectedCity

In the zombie file you will see lines like this:
```xml
<zone name="InfectedReligious" smin="0" smax="0" dmin="1" dmax="3" x="11133.1" z="4428.73" r="60"/>
```

You can copy this line as a template.
Replace the name with the type of zombies you want to spawn in.
`name="InfectedReligious"` can be changed to `name="InfectedCity"` for example.

Next, leave the `smin` and `smax` at 0. You want to edit the `dmin` and `dmax` numbers.
For a large horde you could do something like this:

```xml
<zone name="InfectedReligious" smin="0" smax="0" dmin="200" dmax="300" x="3583.1" z="9884.73" r="60"/>
```

This is telling the game to spawn in 200 minimal up to a maximum of 300 zombies.

The X and Z numbers correspond to the location at which you want the zombies to spawn at.

R is the radius (in meters) of where they can spawn. Locations are commented on the side.
`smin`/`smax` = static amount of zombies, they will always spawn even if a player isn't near.

Now where to put your new zombie line of code. Open the zombie_territories.xml file...
At the top of the file you will see these lines:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<territory-type>

    <territory color="1291845632">
    <!-- you want to place your code here, under the first territory color line. -->
    
</territory-type>
```

Save and upload your new file and have a blast!

## Making your own Custom Zombie and Presets

Using this method you can make many different types of presets. 
One for medical, one for food/drinks just as an example.
I have provided this one to have weapons.

```xml
<!-- cfgrandompresets.xml New Entry -->

<!-- chance here would be 35%, you can edit to your liking -->
<attachments chance="0.35" name="ZombieItemDropSwat">
  <!-- You can use any item found in types.xml to replace below -->
  <item name="AK101" chance="0.20" />
  <item name="AKM" chance="0.20" />
  <item name="FAL" chance="0.20" />
  <item name="M4A1" chance="0.20" />
  <item name="SVD" chance="0.20" />
  <!-- Total of the chances above should come to 100 if you add or remove items -->
</attachments>
```

```xml
<!-- cfgspawnabletypes.xml Replace Entry -->
<!-- You can choose any of the zombies found in this file to add your new preset. For this example I have chosen one of the police zombies. -->

<type name="ZmbM_PolicemanSpecForce">
  <cargo preset="foodArmy" />
  <attachments>
    <item name="BallisticHelmet_Black" />
</attachments>
<attachments>
<item name="HighCapacityVest_Black" />
</attachments>
<attachments>
<item name="AliceBag_Black" />
</attachments>

<!-- this calls the preset from above --> <!-- you can add this preset line to any zombie you wish --> <attachments preset="ZombieItemDropSwat" /> </type> ```

Helmet_Black" />
  </attachments>
  <attachments>
    <item name="HighCapacityVest_Black" />
  </attachments>
  <attachments>
    <item name="AliceBag_Black" />
  </attachments>
  <!-- this calls the preset from above -->
  <!-- you can add this preset line to any zombie you wish -->
  <attachments preset="ZombieItemDropSwat" /> 
</type>
```

Now that creates a custom zombie appearance, but if you want to do more, say you want this custom zombie to be at helicopter crash sites for example, well we can do that too.

We'll use the same zombie we have been working on, so now that part is done we need to open events and add the following:

```xml
<!-- events.xml New Entry -->

<event name="InfectedHeliGuards">
  <nominal>200</nominal>
  <min>200</min>
  <max>250</max>
  <lifetime>3</lifetime>
  <restock>0</restock>
  <saferadius>50</saferadius>
  <distanceradius>50</distanceradius>
  <cleanupradius>100</cleanupradius>
  <flags deletable="0" init_random="0" remove_damaged="1"/>
  <position>player</position>
  <limit>custom</limit>
  <active>1</active>
  <children>
    <child lootmax="5" lootmin="0" max="20" min="20" type="ZmbM_PolicemanSpecForce"/>
  </children>
</event>
```

Now search for "InfectedPolice", you will see this:

```xml
<event name="InfectedPolice">
  <nominal>50</nominal>
  <min>25</min>
  <max>100</max>
  <lifetime>3</lifetime>
  <restock>0</restock>
  <saferadius>100</saferadius>
  <distanceradius>50</distanceradius>
  <cleanupradius>100</cleanupradius>
  <flags deletable="0" init_random="0" remove_damaged="1"/>
  <position>player</position>
  <limit>custom</limit>
  <active>1</active>
  <children>
    <child lootmax="5" lootmin="0" max="0" min="40" type="ZmbF_PoliceWomanNormal"/>
    <child lootmax="5" lootmin="0" max="0" min="40" type="ZmbM_PolicemanFat"/>
    <child lootmax="5" lootmin="0" max="0" min="10" type="ZmbM_PolicemanSpecForce"/>
  </children>
</event>
```

What we want to do is comment out the "ZmbM_PolicemanSpecForce" line so only our crash sites will use them. Or you can leave them in to have them in both.

If you want to leave them in, skip ahead to the next part. Otherwise, replace the above with this:

```xml
<event name="InfectedPolice">
  <nominal>50</nominal>
  <min>25</min>
  <max>100</max>
  <lifetime>3</lifetime>
  <restock>0</restock>
  <saferadius>100</saferadius>
  <distanceradius>50</distanceradius>
  <cleanupradius>100</cleanupradius>
  <flags deletable="0" init_random="0" remove_damaged="1"/>
  <position>player</position>
  <limit>custom</limit>
  <active>1</active>
  <children>
    <child lootmax="5" lootmin="0" max="0" min="40" type="ZmbF_PoliceWomanNormal"/>
    <child lootmax="5" lootmin="0" max="0" min="40" type="ZmbM_PolicemanFat"/>
    <!-- <child lootmax="5" lootmin="0" max="0" min="10" type="ZmbM_PolicemanSpecForce"/> -->
  </children>
</event>
```

Now to add your custom zombies to the crash site itself. Search for "StaticHeliCrash", you will see this:

```xml
<event name="StaticHeliCrash">
  <nominal>140</nominal>
  <min>0</min>
  <max>0</max>
  <lifetime>2500</lifetime>
  <restock>0</restock>
  <saferadius>500</saferadius>
  <distanceradius>500</distanceradius>
  <cleanupradius>1000</cleanupradius>
  <secondary>InfectedArmy</secondary><!-- Zombie to spawn at crash site -->
  <flags deletable="1" init_random="0" remove_damaged="0"/>
  <position>fixed</position>
  <limit>child</limit>
  <active>1</active>
  <children>
    <child lootmax="20" lootmin="10" max="3" min="1" type="Wreck_Mi8"/>
    <child lootmax="20" lootmin="10" max="3" min="1" type="Wreck_UH1Y"/>
  </children>
</event>
```

It's the `<secondary>InfectedArmy</secondary>` line we need to change. To use your new custom zombies, change that to `<secondary>InfectedHeliGuards</secondary>` so the event looks like this:

```xml
<event name="StaticHeliCrash">
  <nominal>140</nominal>
  <min>0</min>
  <max>0</max>
  <lifetime>2500</lifetime>
  <restock>0</restock>
  <saferadius>500</saferadius>
  <distanceradius>500</distanceradius>
  <cleanupradius>1000</cleanupradius>
  <secondary>InfectedHeliGuards</secondary><!-- Zombie to spawn at crash site -->
  <flags deletable="1" init_random="0" remove_damaged="0"/>
  <position>fixed</position>
  <limit>child</limit>
  <active>1</active>
  <children>
    <child lootmax="20" lootmin="10" max="3" min="1" type="Wreck_Mi8"/>
    <child lootmax="20" lootmin="10" max="3" min="1" type="Wreck_UH1Y"/>
  </children>
</event>
```

That's it, you are done. You now have a custom zombie with different clothes, presets, and it will spawn at the helicopter crash sites!

## Understanding How Loot In Buildings Work

How to know how much loot a building can spawn in and what you can do to max it out.

First, let's start off with the event to spawn in the building.
In cfgeventspawns.xml, you set your coordinates for where you want the building at like so:

```xml
<event name="StaticNewsStand">
  <pos x="13971.520508" z="2826.796875" a="210.000000" />
</event>
```

In events.xml, we place our building code, like so:

```xml
<event name="StaticNewsStand">
  <nominal>1</nominal>
  <min>1</min>
  <max>1</max>
  <lifetime>3888000</lifetime>
  <restock>0</restock>
  <saferadius>0</saferadius>
  <distanceradius>0</distanceradius>
  <cleanupradius>100</cleanupradius>
  <flags deletable="0" init_random="0" remove_damaged="0"/>
  <position>fixed</position>
  <limit>mixed</limit>
  <active>1</active>
  <children>
    <child lootmax="10" lootmin="10" max="1" min="1" type="land_city_stand_news2"/>
  </children>
</event>
```

You will notice that on the child line the `lootmin` and `lootmax` are set to 10. This tells the event that the building can have 10 loot spawns. But how do we know it can have 10? For that, we go on to the next step.

In this example we will be using the News Stand Building.
In mapgroupproto.xml you will see this building:

```xml
<group name="Land_City_Stand_News2">
  <usage name="Town" />
  <container name="lootFloor" lootmax="3">
    <category name="tools" />
    <category name="containers" />
    <category name="clothes" />
    <tag name="floor" />
    <point pos="-1.394749 -1.274980 -1.467541" range="0.386719" height="0.746793" />
    <point pos="-2.859683 -1.276101 -0.142190" range="0.410156" height="1.025390" />
    <point pos="-2.789347 -1.275743 -1.474118" range="0.460938" height="0.777977" />
    <point pos="-1.553518 -1.275761 1.417546" range="0.545654" height="1.364135" />
  </container>
  <container name="lootshelves" lootmax="3">
    <category name="tools" />
    <category name="containers" />
    <category name="clothes" />
    <category name="food" />
    <category name="books" />
    <tag name="shelves" />
    <point pos="-3.032413 -0.339241 0.825155" range="0.244886" height="0.612215" />
    <point pos="-2.816331 -0.310748 -0.840539" range="0.340625" height="0.851563" />
    <point pos="-2.897347 -0.320963 -1.596292" range="0.340625" height="0.851563" />
    <point pos="-2.801010 -0.309422 1.398674" range="0.375000" height="0.937500" />
  </container>
  <container name="lootweapons" lootmax="2">
    <category name="weapons" />
    <category name="explosives" />
    <point pos="-3.126306 -0.686172 0.003602" range="0.100000" height="0.250000" />
    <point pos="-1.320649 -0.383808 -1.663223" range="0.269287" height="0.673217" />
  </container>
</group>
```

Now to understand the loot functions, we will break this down and edit it a bit to make it work better.
What are the spawn points? They are lines of code that look like this:
```xml
<point pos="-1.394749 -1.274980 -1.467541" range="0.386719" height="0.746793" />
```

This area controls how much can spawn on the floor. Where it shows `lootmax="3"`, that is telling it to use 3 out of the 4 possible spawn points listed below.

```xml
<container name="lootFloor" lootmax="3"><!-- total of spawn points to use -->
  <category name="tools" />
  <category name="containers" />
  <category name="clothes" />
  <tag name="floor" />
  <point pos="-1.394749 -1.274980 -1.467541" range="0.386719" height="0.746793" /><!-- 1st spawn point -->
  <point pos="-2.859683 -1.276101 -0.142190" range="0.410156" height="1.025390" /><!-- 2nd spawn point -->
  <point pos="-2.789347 -1.275743 -1.474118" range="0.460938" height="0.777977" /><!-- 3rd spawn point -->
  <point pos="-1.553518 -1.275761 1.417546" range="0.545654" height="1.364135" /><!-- 4th spawn point -->
</container>
```

The next is for the shelves and last is for the weapon placements. 

Now let's maximize this to allow you to use the most out of your building.
First, we are going to add up all the spawn points for each area. In the `lootFloor` section, we will change the total to 4 so we are using all of the spawn points it has:

```xml
<container name="lootFloor" lootmax="4">
  <category name="tools" />
  <category name="containers" />
  <category name="clothes" />
  <tag name="floor" />
  <point pos="-1.394749 -1.274980 -1.467541" range="0.386719" height="0.746793" />
  <point pos="-2.859683 -1.276101 -0.142190" range="0.410156" height="1.025390" />
  <point pos="-2.789347 -1.275743 -1.474118" range="0.460938" height="0.777977" />
  <point pos="-1.553518 -1.275761 1.417546" range="0.545654" height="1.364135" />
</container>
```

Next, we will do the same for the next 2 areas so when done it looks like this:

```xml
<container name="lootshelves" lootmax="4">
  <category name="tools" />
  <category name="containers" />
  <category name="clothes" />
  <category name="food" />
  <category name="books" />
  <tag name="shelves" />
  <point pos="-3.032413 -0.339241 0.825155" range="0.244886" height="0.612215" />
  <point pos="-2.816331 -0.310748 -0.840539" range="0.340625" height="0.851563" />
  <point pos="-2.897347 -0.320963 -1.596292" range="0.340625" height="0.851563" />
  <point pos="-2.801010 -0.309422 1.398674" range="0.375000" height="0.937500" />
</container>
<container name="lootweapons" lootmax="2">
  <category name="weapons" />
  <category name="explosives" />
  <point pos="-3.126306 -0.686172 0.003602" range="0.100000" height="0.250000" />
  <point pos="-1.320649 -0.383808 -1.663223" range="0.269287" height="0.673217" />
</container>
```

Now that we have maxed out each area, let's count them all up. Look at each `lootmax` number which is now 4, 4, 2. So we have a total of 10 now. That total goes at the top like so:

```xml
<group name="Land_City_Stand_News2" lootmax="10">
```

The final code should look like this:

```xml
<group name="Land_City_Stand_News2" lootmax="10">
  <usage name="Town" />
  <container name="lootFloor" lootmax="4">
    <category name="tools" />
    <category name="containers" />
    <category name="clothes" />
    <tag name="floor" />
    <point pos="-1.394749 -1.274980 -1.467541" range="0.386719" height="0.746793" />
    <point pos="-2.859683 -1.276101 -0.142190" range="0.410156" height="1.025390" />
    <point pos="-2.789347 -1.275743 -1.474118" range="0.460938" height="0.777977" />
    <point pos="-1.553518-1.275761 1.417546" range="0.545654" height="1.364135" />
  </container>
  <container name="lootshelves" lootmax="4">
    <category name="tools" />
    <category name="containers" />
    <category name="clothes" />
    <category name="food" />
    <category name="books" />
    <tag name="shelves" />
    <point pos="-3.032413 -0.339241 0.825155" range="0.244886" height="0.612215" />
    <point pos="-2.816331 -0.310748 -0.840539" range="0.340625" height="0.851563" />
    <point pos="-2.897347 -0.320963 -1.596292" range="0.340625" height="0.851563" />
    <point pos="-2.801010 -0.309422 1.398674" range="0.375000" height="0.937500" />
  </container>
  <container name="lootweapons" lootmax="2">
    <category name="weapons" />
    <category name="explosives" />
    <point pos="-3.126306 -0.686172 0.003602" range="0.100000" height="0.250000" />
    <point pos="-1.320649 -0.383808 -1.663223" range="0.269287" height="0.673217" />
  </container>
</group>
```

If you look at the top of the mapgroupproto.xml, you will see these 2 lines of code:
```xml
<default name="group" lootmax="6" />
<default name="container" lootmax="4" />
```

As you look through the file and examine the buildings, you will notice that some of these do not have `lootmax` tags. The ones that do not have these will use the above defaults.
If you want to use more than the defaults, you will have to add the `lootmax="#"`. You can either use all the spawn points shown for that area or just some of them.

Now you can even choose to edit the categories and usage tags it has to better suit your needs.
I hope this helps you better understand how these tie in together and what you can do with them.

## Making Custom Categories For Loot Drops

Making new usage tags and categories to customize your loot drops.

In this example, we will be making a new entry for medical supplies.
This will allow us to only have medical items drop in areas we want without all those other items along with them, as medical by default was in the group tools.

```xml
<!-- cfglimitsdefinitions.xml New Entry -->

To make a new entry just add this:

<categories>
  <category name="tools"/>
  <category name="containers"/>
  <category name="clothes"/>
  <category name="vehiclesparts"/>
  <category name="food"/>
  <category name="weapons"/>
  <category name="books"/>
  <category name="explosives"/>
  <category name="MedicalCat"/> <!-- New Entry -->
</categories>
```

```xml
<!-- types.xml Replace Entry -->

You will want to search (ctrl+f) for medic. This will show you the items that can drop in an area for medical. Here is one we will use as an example:

<type name="BandageDressing">
  <nominal>55</nominal>
  <lifetime>2700</lifetime>
  <restock>0</restock>
  <min>55</min>
  <quantmin>-1</quantmin>
  <quantmax>-1</quantmax>
  <cost>100</cost>
  <flags count_in_cargo="0" count_in_hoarder="0" count_in_map="1" count_in_player="0" crafted="0" deloot="0"/>
  <category name="tools"/>
  <tag name="floor"/>
  <tag name="shelves"/>
  <tag name="ground"/>
  <usage name="Medic"/>
</type>
```

The line that shows `<category name="tools"/>`, we will change that to `<category name="MedicalCat"/>` so we end up with this:

```xml
<type name="BandageDressing">
  <nominal>55</nominal>
  <lifetime>2700</lifetime>
  <restock>0</restock>
  <min>55</min>
  <quantmin>-1</quantmin>
  <quantmax>-1</quantmax>
  <cost>100</cost>
  <flags count_in_cargo="0" count_in_hoarder="0" count_in_map="1" count_in_player="0" crafted="0" deloot="0"/>
  <category name="MedicalCat"/>
  <tag name="floor"/>
  <tag name="shelves"/>
  <tag name="ground"/>
  <usage name="Medic"/>
</type>
```

It is important that you replace the old category with your new one, as items can only be assigned to 1 category.

You can keep searching for 'medic' to find more items you want to add your new category to. When you change all the ones you want to use, we will move on to the next step.

```xml
<!-- mapgroupproto.xml Replace Entry -->

In this file you will see all the buildings and objects that have loot drops. What we want to do is put our new category to use. So let's find a building to use.

In this example we will be modifying the C130.
We only want medical and food to spawn for this object. To do this you want your file to look like shown below:

<group name="Land_Wreck_C130J" lootmax="20">
  <usage name="Military"/>
  <usage name="Police"/>
  <usage name="Medic"/>
  <usage name="Firefighter"/>
  <usage name="Industrial"/>
  <usage name="Farm"/>
  <usage name="Coast"/>
  <usage name="Town"/>
  <usage name="Village"/>
  <usage name="Hunting"/>
  <usage name="Office"/>
  <usage name="School"/>
  <usage name="Prison"/>
  <container name="lootFloor" lootmax="20">
    <category name="MedicalCat"/>
    <category name="food"/>
    <point pos="4.293945 -1.541862 3.939941" range="1.100000" height="1.145172" />
    <point pos="5.630615 -1.221848 2.478027" range="1.203125" height="1.058037" />
    <point pos="5.128174 -1.250969 2.282227" range="1.237500" height="1.899963" />
    <point pos="4.601563 -1.275345 2.026855" range="1.237500" height="1.900002" />
    <point pos="4.976563 -1.479630 4.021972" range="1.340625" height="1.139282" />
    <point pos="6.462646 -1.856918 -0.474609" range="1.595988" height="1.900002" />
    <point pos="4.273926 -1.856918 5.096679" range="1.718352" height="1.900002" />
    <point pos="3.722656 -1.856918 -1.948730" range="1.821779" height="1.900002" />
    <point pos="7.276123 -1.856918 6.080566" range="1.827611" height="1.900002" />
    <point pos="5.192871 -1.856918 -1.159668" range="1.846799" height="1.900002" />
    <point pos="-2.588379 -1.856918 -0.039551" range="1.898411" height="1.900002" />
    <point pos="5.540283 -1.856918 6.118164" range="1.908636" height="1.900002" />
    <point pos="2.502686 -1.856918 4.775879" range="1.942924" height="1.900002" />
    <point pos="7.894042 -1.856918 2.567383" range="2.068359" height="1.900002" />
    <point pos="8.488282 -1.856918 4.621094" range="2.069595" height="1.900002" />
    <point pos="-0.299072 -1.856918 -1.951172" range="2.199951" height="1.900002" />
    <point pos="-0.732910 -1.856918 2.940430" range="2.199951" height="1.900002" />            
  </container>
</group>
```

As you can see, we have used all the usage tags from the cfglimitsdefinition.xml and only 2 categories, the `MedicalCat` we just made and the `food` category. Because we have only chosen these two categories, the C130 in this case will only look for the medical items you set and any food items in the game.

The C130 by default does not have loot drop spawns (the `<point pos` lines), I have included them for you.

Again, you can go through all the buildings and any that you want to use your medical supplies, just add your new category (`<category name="MedicalCat"/>`) to the building/object.

You can also remove usage tags and categories to trim them down and make them even more customized. But never trim below 1 usage and 1 category, otherwise nothing will spawn in said building/object.

## Using Tags

First off, there are a few things you should know about XML files.
Tags: Every tag has a start and an end, depending on the file you have open you will see different types of tags, but the principle is the same. 

Here are some examples:
| Start Tag | End Tag |
|-----------|---------|
| `<type>` | `</type>` |
| `<event>` | `</event>` |
| `<group>` | `</group>` |
| `<container>` | `</container>` |
| `<attachments>` | `</attachments>` |
| `<cargo>` | `</cargo>` |

Tags as shown above will have the coding after the start tag. When it reaches the end tag it tells the game this is the end of this code.

Other forms of tags that don't have an end would be like this:
```xml
<usage name="Town" /> 
<category name="tools" />
```
Tags like these use the `/>` at the end to close them off, as there is no coding in between. We will go over these types of tags later on in more detail.

## Definitions

### 1. Basic Definitions

| Term | Definition |
|------|------------|
| Name | Class Name of Item to Spawn |
| Nominal | Number of items spawned in the world at any given time. (Ideal Value) Must be more or equal to min value |
| Lifetime | Time (In Seconds) before this type of items gets deleted in the world (If no players interact with it). Lifetime 604800 (seven days), 3888000 (45 days) |
| Restock | If Value=0, Respawn item type in bulk to reach Nominal Value. If !=0, then Value=Time in seconds to respawn 1 additional item type, until Nominal Value is reached. |
| Max | The maximum amount of spawns of this type on the server that is occurring (or possibility of it occurring) on the server |
| Min | Minimum number of items of this type in world. Once number falls below minimum, the Restock process begins. (Must be less or equal to nominal value) |
| QuantMin | Minimum % Value for quantity (Rags #, Mag Ammo Value, Ammo Counts). Use -1 if Not Applicable. (Less or equal to quantmax value) See note below |
| QuantMax | Maximum % Value for quantity (Rags #, Mag Ammo Value, Ammo Counts). Use -1 if Not Applicable. (More or equal to quantmin value) See note below |
| Cost | Priority of Item Spawning in CE queue (100 is default) |
| Flags | Flags direct the spawner, in what case it must take min and nominal values in to consideration for every item counting for spawning |
| Limit Options | `child` will randomly pick a single child item to spawn at each event. `mixed` will pick a single child item, starting from the top at each event until the child max is reached. `parent` will spawn all of the child items at each event adhering to the child's min/max values. `custom` uses external sources to determine the `limits`, like territory files. |

#### Child Line Options

| Option | Definition |
|--------|------------|
| Lootmin | Minimum amount of loot object can spawn. See object in mapgroupproto for value. |
| Lootmax | Maximum amount of loot object can spawn. See object in mapgroupproto for value. |
| Min | Minimum number to spawn at a given location as set in cfgeventspawns. |
| Max | Maximum number to spawn at a given location as set in cfgeventspawns. |

| Flag | Definition |
|------|------------|
| count_in_cargo | Count items stored in containers as part of Nominal Value (0=False, 1=True) |
| count_in_hoarder | Tents, barrels, underground stashes (0=False, 1=True) |
| count_in_map | Count items in map as part of Nominal Value (0=False, 1=True) |
| count_in_player | Count items stored in Player Inventory as part of Nominal Value (0=False, 1=True) |
| crafted | Is the item a crafted item (0=False, 1=True) |
| deloot | Dynamic event loot objects - helicrashes in majority of cases only by default |

| Term | Definition |
|------|------------|
| Category Name | Useful for sorting, Internal Category. Name must start with (Vehicle, Static, Loot, Item, Infected, Animal). |
| Safe Radius | Is how far away you need to be for the spawn not to happen or close down |
| Distance Radius | Distance away from other similar zone of this type. Infected City spawn zones cannot spawn within 50m of each other. Not really a problem |
| Cleanup Radius | Allows objects/zombies to be cleared from an area for however long you are within the area. (Example - Let me explain: Let's say the values we use are 300. When you are 300m from a village, zombies then spawn immediately. Once we are within that 300m, no more zombies will spawn so long as we are within 300m of the zone. The zombies that are within this "saferadius" will not despawn unless you move out of this 300m radius.) If you want zombies that respawn endlessly, you should set this saferadius to 0. This allows zombies to respawn immediately. |
| Tag Name (optional) | MUST be AFTER the category |
| Usage Name (optional) | Internal Category used in the mpmissions\dayzOffline.chernarusplus\cfgRandomPresets.xml file. |
| Value Name (optional) | Used to specify Central Loot Economy Spawning Locations |

Note: MUST BE be -1 for all normal items types OR from 0 to 100 (value in percents) for items that could have something inside (subclass inside the item), for example for bullets inside weapon mags, water bottle, canteen, can of soda. Note: you cannot set quantmin 20 quantmax -1, it will break things!!

### 2. Globals Definitions

Definitions for the Globals.xml File

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| AnimalMaxCount | Integer | 200 | Maximal limit of spawned animals (not ambient) across all zones in map |
| CleanupAvoidance | Integer | 100 | (m) Distance from player required for item deletion |
| CleanupLifetimeDeadAnimal | Integer | 1200 | (sec) Default lifetime for dead animals |
| CleanupLifetimeDeadInfected | Integer | 330 | (sec) Default lifetime for dead infected |
| CleanupLifetimeDeadPlayer | Integer | 3600 | (sec) Default lifetime for dead player |
| CleanupLifetimeDefault | Integer | 45 | (sec) Default lifetime for entities with no specific economy setup, but damage >= 1.0(ie. dead) |
| CleanupLifetimeLimit | Integer | 50 | How many items can be deleted at once during standard cleanup |
| CleanupLifetimeRuined | Integer | 330 | (sec) Default lifetime for ruined loot |
| FlagRefreshFrequency | Integer | 432000 | (sec) Items lifetime will be refreshed with this frequency. |
| FlagRefreshMaxDuration | Integer | 3456000 | (sec) How long the flag will be refreshing items. |
| IdleModeCountdown | Integer | 60 | (sec) Activate economy idle mode on empty server after given time |
| IdleModeStartup | Integer | 1 | Enable economy idle mode on server startup |
| InitialSpawn | Integer | 100 | (%) How much loot will be spawned on server initial start (without storage). |
| LootProxyPlacement | Integer | 1 | Allow dispatch containers to receive the loot. |
| RespawnAttempt | Integer | 2 | How many attempts are performed during single item respawn |
| RespawnLimit | Integer | 20 | How many items of one type can be spawned at once |
| RespawnTypes | Integer | 12 | How many different types can be respawned at once |
| RestartSpawn | Integer | 0 | (%) How much loot should be respawned during restart to nomimal |
| SpawnInitial | Integer | 1200 | How many initial test are allowed for item spawn |
| TimeHopping | Integer | 60 | (sec) penalty time for server hoppers |
| TimeLogin | Integer | 15 | (sec) default login time |
| TimeLogout | Integer | 15 | (sec) default logout time |
| TimePenalty | Integer | 20 | (sec) penalty time for player that is still in play session |
| ZombieMaxCount | Integer | 1000 | Maximal limit of spawned zombies across all zones in map |
| ZoneSpawnDist | Integer | 300 | (m) Distance to invoke infected spawn in nearby zone (dynamic infected) |

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| WorldWetTempUpdate | Integer | 1 | Allow update of wetness and temperature values on all items in the world. |
| FoodDecay | Integer | 1 | Allow decay on food (requires WorldWetTempUpdate set to 1). |

### 3. Persistence Definitions with charts

Persistence - in a nutshell is the ability for things to be carried over across server restarts, be it a weapon, a body, a tent, a vehicle, whatever. Items that are persistent do not disappear during restarts. All inventory items are persistent across restarts. As tents, vehicles are not in game there is no way to define that. Character save data including location and inventory is persistent. Servers are still persistent for 45 days.
		
When players refer to an item, object or structure as being "Persistent", what they mean is the item, object or structure does not despawn or move during a server restart and has a very long Item Cleanup time, or the amount of time before an object is despawned by the Central Loot Economy. There are a couple of exceptions to these rules:

- Ruined Items are cleaned up regardless of their Persistence
- Vehicles will occasionally move or slide during a server restart depending on its location
		
The length of time an item, object or structure is in the world before the Central Loot Economy despawns it, varies based on the object itself. If, at any point, the object is interacted with in any way, its Item Cleanup timer will reset. Essentially, Persistent objects that are regularly used or interacted with will never despawn (assuming they do not become ruined or the server is not wiped).

See chart below:
| Items | Persistence |
|-------|-------------|
| Oil Barrels, Sea Chests, Wooden Cases | 45 Days |
| Tents | 45 Days |
| Ammo Boxes, Protective Cases | 45 Days |
| Fences, Watchtowers, Wire Mesh Barriers | 45 Days |
| Underground Stashes | 45 Days |
| Battery Chargers, Cable Reels, Generators, Spotlights | 7 Days |

### 4. Item cleanup Definitions with charts

Item Cleanup - Items or objects that are "non-persistence" are also managed by the Central Loot Economy. "Non-persistence" items will not automatically despawn if walked away from, rather these items just have a much shorter Item Cleanup time and/or may be removed upon a server restart.

| Items | Time |
|-------|------|
| Car Battery, Truck Battery | 12 Hours |
| Backpacks* | 6 Hours |
| Garden Plot, Garden Plants | 6 Hours |
| All Other Items | 15 Minutes - 4 Hours |

### 5. Tier layouts

Tiers - coastal areas tier 1, central part tier2, north left part of the map tier 3, NW Forest 4. For Chernarus there are 4 tiers, for Livonia ONLY 3 (three)!!!!!	

### 6. Economy

For economy.xml:

```xml
<dynamic init="1" load="1" respawn="1" save="1"/>   event spawns
<animals init="1" load="0" respawn="1" save="0"/>  bears, wolves etc.
<zombies init="1" load="0" respawn="1" save="0"/>  infected
<vehicles init="1" load="1" respawn="1" save="1"/> cars
<randoms init="0" load="0" respawn="1" save="0"/>  dynamic events like helis
<custom init="1" load="1" respawn="1" save="1"/>   imported using mapgroouppos and init
<building init="1" load="1" respawn="1" save="1"/> buildings in the map
<player init="1" load="1" respawn="1" save="1"/>   player information
```

1 is on, 0 is off. `init` = initial load up, `load` = object spawns, `respawn` = well respawn, and `save` = save to Persistence files.

### 7. EconomyCore Definitions

For cfgEconomyCore.xml:

cfgEconomyCore - It is used to configure classes included in the central economy, persistence backups, infected dynamic zones, CE logging and updaters.

Root Classes - Root classes are parent classes of entities which will be used by the central economy. This allows you to specifically define what you want to be a part of the economy and what should stay omitted.

#### Attributes:

- `name` - name of entity type
- `reportMemoryLOD` - default is yes, this allows to turn off console messages about missing memory LODs
- `act` - sets entity type (none/character/car)

If "act" type of entity is set to none (or is missing), the entity is considered to be loot. Make sure to properly define entities as character/car where needed since economy sets them up in a different way.

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| world_segments | Integer | 12 | Defines how in many segments world will be split by CE - this affects save, load, cleanup and other processing events - it's performance wide for huge maps (note that default value is for Chernarus map!) |
| backup_period | Integer | 60 | Period of regular backup creation (minutes) |
| backup_count | Integer | 12 | Count of backups to keep - folders |
| backup_startup | Boolean | false | Process backup at startup of server |
| dyn_radius | Float | 20 | Default value for dynamic infected zone - size of the zone (meters) |
| dyn_smin | Float | 0 | Default value for dynamic infected zone - minimal static count |
| dyn_smax | Float | 0 | Default value for dynamic infected zone - maximal static count |
| dyn_dmin | Float | 0 | Default value for dynamic infected zone - minimal dynamic count |
| dyn_dmax | Float | 5 | Default value for dynamic infected zone - maximal dynamic count |
| save_events_startup | Boolean | on | If disabled, no data/events.bin is created at startup (usefull for minimal hive setup) |
| save_types_startup | Boolean | on | If disabled, no data/types.bin is created at startup (usefull for minimal hive setup) |
| log_hivewarning | Boolean | on | enable/ disable some of the hive warning messages in console |
| log_storageinfo | Boolean | off | enable/ disable periodic storage info messages in console (if CE stores files) |
| log_missionfilewarning | Boolean | on | enable/ disable console warning messages about mission files (typically sandbox mode does not require them all) |
| log_celoop | Boolean | off | enable/ disable logging of CE loop timing and basic statistical info |
| log_ce_dynamicevent | Boolean | off | enable/ disable logging of CE specific - dynamic events specific |
| log_ce_vehicle | Boolean | off | enable/ disable logging of CE specific - vehicle specific |
| log_ce_lootspawn | Boolean | off | enable/ disable logging of CE specific - loot spawn specific |
| log_ce_lootcleanup | Boolean | off | enable/ disable logging of CE specific - cleanup specific (not just loot actually) |
| log_ce_lootrespawn | Boolean | off | enable/ disable logging of CE specific - loot respawn specific |
| log_ce_statistics | Boolean | off | enable/ disable logging of CE specific - statistical data |
| log_ce_zombie | Boolean | off | enable/ disable logging of CE specific - infected related |
| log_ce_animal | Boolean | off | enable/ disable logging of CE specific - animal related |

#### Persistence backups

Chernarus is a large map which contains a vast amount of objects which need to be controlled by the central economy. In order to do so in a performance-friendly fashion, the map is split into segments which are then individually saved in periodic intervals. Since most of the server uptime a segment of Chernarus is in the process of being saved, a server crash or other technical difficulties may corrupt the segment and in turn compromise the persistence. For this reason, an automated backup system has been created which can be configured by the following variables of the cfgEconomyCore.xml:

- `world_segments`: Defines the number of world segments. Default value "12" is set with Chernarus in mind, if you are running another map with a different size/loot economy/amount of entities, you may want to adjust the amount of segments to its specifications.

- `backup_period`: This configures how often are persistence backups created. Every X minutes, a new backup of the current persistence will be created, overriding the oldest backup based on the `backup_count` variable. Minimum value "15", default is "60".

- `backup_count`: This determines the amount of kept persistence backups. If set to "3" with a `backup_period` of "20", it will keep 3 backups made over period of one hour, and after 80 minutes it will override the oldest one. Default value is "12".

- `backup_startup`: This will create a backup on the server immediately after the server finishes load operations and initial/additional respawn. Default is "false".

## Tips, Tricks and FAQ

Q: What can I use for an event name?
A: NAME must START with (Vehicle, Static, Loot, Item, Infected, Animal). For example: StaticBuilding.

Q: How can I change Items/Weapons from spawning in damaged?
A: In cfgspawnabletypes.xml near the top of the page use these values in `<damage min="0.1" max="0.2" />`. Also in events.xml set in Flags, `remove_damaged="1"`.

Q: I want my weapons/car to spawn in with attachments already on them.
A: In cfgspawnabletypes.xml add this to your weapon/car:
```xml
<attachments chance="1.00">
  <item name="Name of Attachment Here" chance="1.00" />
</attachments>
```

Q: How do I increase/decrease the amount of Zombies on my map?
A: In zombies_territories.xml add/minus the values found in `dmin` / `dmax`.

Q: How do I increase/decrease the amount of Animals on my map?
A: In "AnimalNameHere"_territories.xml add/minus the values found in `dmin` / `dmax`.

Q: How can I improve the FPS on my server?
A: 
1. In globals.xml lower the amount in `ZoneSpawnDist`. 
2. Keep total number of event spawns down as they add stress to the server.
3. Keep overall nominals down. If you add to one item, try and take away from another.

Q: I keep getting an error in cfgspawnabletypes.xml saying "--" is not permitted.
A: Those are comment lines by the developers. Some xml checkers see those as an error. You can safely remove them to avoid the error from popping up.

Q: How can I get more loot on my map (weapons, items etc.)?
A: In types.xml try increasing the nominal and min amount for said item(s).

Q: How can I spawn items into a container?
A: In cfgspawnabletypes.xml look for the container you wish to use and add this:
```xml
<cargo chance="1">
  <item name="ItemNameToSpawnIn" />
</cargo>
```

Q: Some files are grayed out. How can I make changes to them?
A: Download the file, make your changes, then send it to the server.

Q: How do I remove items from spawning in the game?
A: Set the nominal and min to zero in the types.xml.

Q: How can I spawn in full clips/mags, max rice, cereal etc.?
A: Set the `quantmin` and `quantmax` to 100 in types.xml.

Q: I added a new Category to the cfglimitsdefinition.xml but I do not see the change take effect.
A: Only 1 Category can be set to an item. Make sure you are replacing the one that was there. The category also needs to be added to the building(s) you want said items in. Look in mapgroupproto.xml for buildings.

Q: How can I make it so new characters have starting gear?
A: 
- Console: You can have the items spawn in a container, an NPC or on the ground near your player spawn points.
- PC Users: You can edit the init.c file to set up starting gear.

For console users: At one time we could edit the init.c, it caused problems on the console causing crashes and other unforeseen issues, so the devs patched it preventing us from making those changes. Some of the lucky ones that used it prior to the patch still benefit from it. But no one can make new files using that method until they fix the issues and release the script files back to console.

Q: How can I turn off the weather?
A:
- Console: At this time we can't.
- PC Users: That requires editing the init.c file.

Q: How do I get different colored weapons and/or mags?
A: Search in types.xml for "_black", "_green", "_camo". Any items you come across that have a nominal value of zero you want to change, just up the nominal, min and max values. Set crafting to 0.

Q: How can I increase the item respawn rate?
A: Set the min and max to equal the nominal, set restock to zero. In globals.xml lower the value of `RestartSpawn`.

Q: How can I have weapons spawn anywhere on the map?
A: In types.xml remove the Tier # tags. You can also add usages to the weapons to include any and all types of buildings.

Q: Can I change the name of the files?
A: No. The game will only recognize the original file names. The only time you would want to rename a file is for backup reasons.

Q: Why is my Object not spawning in (Well, NPC, Building etc.)?
A: There can be a few reasons why:
1. Run your files through an xml checker, it is possible there is an error in the code.
2. Verify you have entered in all the code needed correctly.
3. A lot of objects do not like to spawn in if they are too close to another object. Make sure the area you want to spawn in is clear of any object, loot, people etc. You might have to move your spawn point a little to get it to spawn in correctly.

Q: How do I ban a player or add to the white list?
A: Add the player UID in a new line (44 characters long id that you can find in .adm or .rpt logs.) Try the following format:
```
Name1
Name2
Name3
```

Q: Why do my cars spawn in on their side, upside down or multiple in one spot?
A: The server needs time to load in a bit first.

I found that if you set `RestartSpawn` to 150 and `SpawnInitial` to 600 in globals.xml, this lets them spawn in smoothly. Setting below these numbers can result in what is mentioned above.

Q: What is CLE, CE. How does this affect the game?
A: The Central Loot Economy (often abbreviated "CLE" or "CE") is DayZ's unique management system for loot spawning and clean-up. It is a complex system of tags, categories, zones, maximums, minimums, averages, and so on. The CLE dictates exactly how many of each item can be present on a single instance of the game at a time (one server), with important factors like randomization and rarity baked in. These values can be adjusted at any time, without requiring players or servers to install a game update.

In addition to controlling how much of each item is present, it also "cleans up" the map by removing items which have become ruined or which have gone untouched for a set amount of time. This function prevents the game from becoming cluttered with useless items, serves players with a continually refreshed pool of available gear, and acts as one method of preventing individual players from having too much influence over the entire server's economy. For more information about how long items stay in place without activity, please see Persistence (Found in working terms in the file section).

Q: My buildings keep rotating on restarts, how can I stop this?
A: It's not foolproof but try using this format `X=#.# Y=#.# A=#.000000` (6 digits after the decimal point).

Q: How can I stop my cars from spawning in as loot?
A: The easiest way I have found is to make a custom category in cfglimitsdefinitions.xml and name it "vehicles", then in types.xml place this category on all your cars (CivilianSedan, Hatchback_02, OffroadHatchback, Sedan_02). By default, these cars do not have a category tag, so just add it at the bottom in between the flags line and `</type>`.

Q: Trying to get offline mode but there isn't any mission file.
A: Go into `C:\Program Files (x86)\Steam\steamapps\common\DayZ`, if you do not have a "missions" folder, you will need to "create a new folder" and make sure it is named "missions". Then paste the website's "DayZCommunityOfflineMode.ChernarusPlus" folder in there.

Q: Can I have multiple Sea Chests with different loot?
A: No, all the Sea Chests would have the same loot. You can however use different backpack colors to hold loot, as each color is really a different model.

Q: What are Server Hopping Spawn Points?
A: There are 99 server hopper spawn points, different from regular spawn points. The server_hopper ones are coastal and inland.

If you change official servers, you respawn to a random server hopper spawn point. Always. No such thing as a home server and no time element. This is because they have a shared player db/hive.

It prevents base hopping and loot cycling across servers.

Private/community servers don't need this since they each have their own player db.

Q: I'm trying to spawn a barrel in with items but it spawns in empty.
A: You cannot spawn items into a barrel. Try using a Sea Chest, Bags, NPCs or a Car.

Q: Why can't I add the Payday mask?
A: They were removed due to copyright.

Q: Why can't I add buildings using the mapgrouppos.xml?
A: mapgrouppos.xml is used to keep track of each building's placement for loot purposes. It is not used to place buildings into the map. You need the init.c file to actually place the buildings in. Currently the init.c file is not available for console.

Q: I stopped the server, edited my files, and restarted, but why do I still have items/loot just chilling and staying around? Why is it not despawning, I set them to 10 seconds.
A: Immediate changes or any changes to your files, do NOT affect already spawned items. Either wipe your server or just wait until the old items despawn.

Q: Why is my server stopping and restarting on its own (Restart loop)?
A: There are 2 main reasons this can happen:
1. An error in one or more of your files
2. Nitrado is having issues

If you feel it is on Nitrado's end, send them a ticket to get it resolved.

---

Original File Created by Bhaalshad. For more help files for DayZ come visit us at DonSibley.Games
If you upload this to another Discord or webpage, please leave the filename intact and don't take credit as your own. Thank you and enjoy :)
