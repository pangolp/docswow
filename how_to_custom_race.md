# [How to: Custom Races!](http://www.ac-web.org/forums/showthread.php?181048-How-to-Custom-Races!)

**1. Introduction**

Okay so now I've been trying to make custom races for a week or so because the only guide made was for an earlier patch and the people who knew what to do wouldn't respond to my questions or just simply ignored me. So now I'm sharing what I've found out on how to create your own custom race.

So what you have to understand for a start is that you can't have all custom races such as pigs or something else, it has to be a race that's actually defined, now you'll probably ask me: "which is defined?", well let's take a look:

**SharedDefines.h**

```c++
enum Races
{
	RACE_NONE = 0,
	RACE_HUMAN = 1,
	RACE_ORC = 2,
	RACE_DWARF = 3,
	RACE_NIGHTELF = 4,
	RACE_UNDEAD_PLAYER = 5,
	RACE_TAUREN = 6,
	RACE_GNOME = 7,
	RACE_TROLL = 8,
	//RACE_GOBLIN = 9,
	RACE_BLOODELF = 10,
	RACE_DRAENEI = 11,
	//RACE_FEL_ORC = 12
	//RACE_NAGA = 13,
	//RACE_BROKEN = 14,
	//RACE_SKELETON = 15,
	//RACE_VRYKUL = 16,
	//RACE_TUSKARR = 17,
	//RACE_FOREST_TROLL = 18,
	//RACE_TAUNKA = 19,
	//RACE_NORTHREND_SKELETON = 20,
	//RACE_ICE_TROLL = 21
};
```

That is the current races that is playable. What I'm going to teach you today is how to add two of them: The Goblin and the Fel Orc.

Before we start I'd like to say a couple of things:

This is not and easy guide, don't expect me to hand out information if you don't know what you're doing
I will provide information that I found worked for me
I don't care if anything I do is "hacky".

**Getting The Files**
Okay, we'll start editing the DBC files, the ones you need are:

 - ChrRaces.dbc
 - CharBaseInfo.dbc
 - CharStartOutfit.dbc
 - SkillLineAbility.dbc
 - SkillRaceClassInfo.dbc
 - Faction.dbc

You can extract them from the core/dbc, it's the same as in the client. You'll also have to get some Lua and XML files from the blizzard patches so that you can edit the character creation screen. Those files are:

```
patch-enXX-3.mpq\Interface\GlueXML\GlueStrings.lua
patch-enXX-3.mpq\Interface\GlueXML\GlueParent.lua

patch-enXX-2.mpq\Interface\GlueXML\CharacterCreate.lua
patch-enXX-2.mpq\Interface\GlueXML\CharacterCreate.xml
```

Okay, great so some of you will maybe ask how to extract those, what you do is get your favorite MPQ Editor, you can use whichever you like (google it if you don't have one) and open the patches with it, go to the path the I wrote above in the MPQ file and drag it somewhere in a folder for this project of yours.

Now that we got all our files we need to arrange them, make a new folder called "RacePatch" on your desktop which should look like this:

```
RacePatch\Interface\GlueXML\GlueStrings.lua
RacePatch\Interface\GlueXML\GlueParent.lua
RacePatch\Interface\GlueXML\CharacterCreate.lua
RacePatch\Interface\GlueXML\CharacterCreate.xml

RacePatch\DBFilesClient\ChrRaces.dbc
RacePatch\DBFilesClient\CharBaseInfo.dbc
RacePatch\DBFilesClient\CharStartOutfit.dbc
RacePatch\DBFilesClient\SkillLineAbility.dbc
RacePatch\DBFilesClient\SkillRaceClassInfo.dbc
RacePatch\DBFilesClient\Faction.dbc
```

Now that we got our desired files smoothly lined up we can go on to the editing of them.

**Editing The DBC Files**

For this, unless stated otherwise, use your favorite DBC editor.

Firstly open up your **ChrRaces.dbc**, you'll see all the races I listed above shows up, now what you want to do is fine these 2 lines:

```csv
9,1,1,0x0,6894,6895,"Go",7,7,15007,0x448,"Goblin",0,0x2,,,"Goblin",,,,,,,,,,,,,,0xFF01FE,,,"Goblin",,,,,,,,,,,,,,0xFF01CC,,,"Gobelin",,,,,,,,,,,,,,0xFF01CC,"NORMAL","NONE","NORMAL",0,
12,5,1,0x0,16981,16980,"Fo",7,7,15007,0x448,"FelOrc",0,0x2,,,"Fel Orc",,,,,,,,,,,,,,0xFF01FE,,,"Fel Orc",,,,,,,,,,,,,,0xFF01CC,,,"Fel Orc",,,,,,,,,,,,,,0xFF01CC,"NORMAL","NORMAL","NORMAL",0,
```

Or something along the lines of that..

Now you want to edit is as follows:
 - 2nd column, it must change by 12 for the race is playable.
 - 4th column defines the faction of the race, see Faction.dbc
 - 8th column, 1 for horde and 7 for alliance. -Taken from Modcraft, see credits


Now you want to save and close that. And you want to open up **CharBaseInfo.dbc**, this file determines the class/race combos for your races. So what you want to do is for now only add 2 lines:

```csv
9, 1
12, 1
```

This makes it so that Goblin (9) can be warrior and so can Fel Orc (12).
--NOTE: If you're having trouble opening this DBC file Taliis does the job ;)

Now that we're done with that we'll move on to the Lua/XML files, and this is where the fun begins.

**Editing the Interface Files**

Okay so the first thing you want to do is get open the GlueStrings file, in there you'll find many values that you can edit and such, but let's keep it simple and add what we need:

Find

```lua
ABILITY_INFO_BLOODELF1 = "- Enchanting skill increased.";
```

And on top of that add this:

```lua
ABILITY_INFO_GOBLIN1 = "- Goblin Racial 1";
ABILITY_INFO_GOBLIN2 = "- Goblin Racial 2";
ABILITY_INFO_GOBLIN3 = "- Goblin Racial 3";
ABILITY_INFO_GOBLIN4 = "- Goblin Racial 4";
ABILITY_INFO_FELORC1 = "- Fel Orc Racial 1";
ABILITY_INFO_FELORC2 = "- Fel Orc Racial 2";
ABILITY_INFO_FELORC3 = "- Fel Orc Racial 3";
ABILITY_INFO_FELORC4 = "- Fel Orc Racial 4";
```

Next find:

```lua
RACE_CHANGE_IN_PROGRESS = "Updating Race...";
```

after that line insert these:

```lua
RACE_INFO_GOBLIN = "Information about Goblin males.";
RACE_INFO_GOBLIN_FEMALE = "Information about Goblin females.";
RACE_INFO_FELORC = "Information about Fel Orc males.";
RACE_INFO_FELORC_FEMALE = "Information about Fel Orc females.";
```

And you're done with **GlueStrings.lua**, close it down (remember to save ofc) and we'll move on to **GlueParent.lua**

So as you now see this is what the ModCraft tutorial didn't cover, and it's here that many people fail, so therefore I'll complete this part :-)

Firstly you want to open up **GlueParent.lua** (of course) and then search for

```lua
GlueAmbienceTracks["CHARACTERSELECT"] = "GlueScreenIntro";
```

After this line add these 2:

```lua
GlueAmbienceTracks["GOBLIN"] = "GlueScreenIntro";
GlueAmbienceTracks["FELORC"] = "GlueScreenIntro";
```

Now you want to go to around:

```lua
CHARACTERSELECT = {
	{1, 0, 0.00000, 0.00000, -1.00000, 1.0, 0.15000, 0.15000, 0.15000, 1.0, 0.00000, 0.00000, 0.00000},
	{1, 0, -0.74919, 0.35208, -0.56103, 1.0, 0.00000, 0.00000, 0.00000, 1.0, 0.44706, 0.54510, 0.73725},
	{1, 0, 0.53162, -0.84340, 0.07780, 1.0, 0.00000, 0.00000, 0.00000, 2.0, 0.55, 0.338625, 0.148825},
},
```

and after that you should add:

```lua
GOBLIN = {
	{1, 0, 0.00000, 0.00000, -1.00000, 1.0, 0.15000, 0.15000, 0.15000, 1.0, 0.00000, 0.00000, 0.00000},
	{1, 0, -0.74919, 0.35208, -0.56103, 1.0, 0.00000, 0.00000, 0.00000, 1.0, 0.44706, 0.54510, 0.73725},
	{1, 0, 0.53162, -0.84340, 0.07780, 1.0, 0.00000, 0.00000, 0.00000, 2.0, 0.55, 0.338625, 0.148825},
},

FELORC = {
	{1, 0, 0.00000, 0.00000, -1.00000, 1.0, 0.15000, 0.15000, 0.15000, 1.0, 0.00000, 0.00000, 0.00000},
	{1, 0, -0.74919, 0.35208, -0.56103, 1.0, 0.00000, 0.00000, 0.00000, 1.0, 0.44706, 0.54510, 0.73725},
	{1, 0, 0.53162, -0.84340, 0.07780, 1.0, 0.00000, 0.00000, 0.00000, 2.0, 0.55, 0.338625, 0.148825},
},
```

This will add the light so the models are lightened up and nice :-)

After this you want to go to:

function SetBackgroundModel(model, name)
and replace that whole function with:

function SetBackgroundModel(model, name)
local nameupper = strupper(name);

if (name == "Goblin" or name == "GOBLIN") then
name = "Orc";
end

if (name == "FelOrc" or name == "FELORC") then
name = "Orc";
end

local path = "Interface\\Glues\\Models\\UI_"..name.."\\UI_"..name..".m2";
if ( model == CharacterCreate ) then
SetCharCustomizeBackground(path);
else
SetCharSelectBackground(path);
end
PlayGlueAmbience(GlueAmbienceTracks[nameupper], 4.0);
SetLighting(model, nameupper)
end

This will make the goblin and the Fel Orc appear as if they had the orc character creation background.

You're now done with the GlueParent.lua and can move on to the CharacterCreate.lua, firstly open it and go to
MAX_RACES = 10; and edit that to
MAX_RACES = 12;

Find

["DRAENEI_MALE"] = {0.5, 0.625, 0, 0.25},
["DRAENEI_FEMALE"] = {0.5, 0.625, 0.5, 0.75},

After that add:

["GOBLIN_MALE"] = {0.625, 0.625, 0, 0.25},
["GOBLIN_FEMALE"] = {0.625, 0.625, 0, 0.25},

["FELORC_MALE"] = {0.5, 0.625, 0, 0.25},
["FELORC_FEMALE"] = {0.5, 0.625, 0, 0.25},
-This will add the Draenei icons to the character creation screen since I haven't quite figured out the icon placement coord system.

Next you'll close down Charactercreate.lua and open up CharacterCreate.xml, you'll want to find:

<CheckButton name="CharacterCreateRaceButton1" inherits="CharacterCreateRaceButtonTemplate" id="1">
<Anchors>
<Anchor point="TOP" relativePoint="TOP" x="-50" y="-61"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton2" inherits="CharacterCreateRaceButtonTemplate" id="2">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton1" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton3" inherits="CharacterCreateRaceButtonTemplate" id="3">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton2" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton4" inherits="CharacterCreateRaceButtonTemplate" id="4">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton3" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton5" inherits="CharacterCreateRaceButtonTemplate" id="5">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton4" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton6" inherits="CharacterCreateRaceButtonTemplate" id="6">
<Anchors>
<Anchor point="TOP" relativePoint="TOP" x="50" y="-61"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton7" inherits="CharacterCreateRaceButtonTemplate" id="7">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton6" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton8" inherits="CharacterCreateRaceButtonTemplate" id="8">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton7" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton9" inherits="CharacterCreateRaceButtonTemplate" id="9">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton8" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton10" inherits="CharacterCreateRaceButtonTemplate" id="10">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton9" relativePoint="BOTTOMLEFT" x="0" y="-21"/>
</Anchors>
</CheckButton>

and replace it with
<CheckButton name="CharacterCreateRaceButton1" inherits="CharacterCreateRaceButtonTemplate" id="1">
<Anchors>
<Anchor point="TOP" relativePoint="TOP" x="-50" y="-50"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton2" inherits="CharacterCreateRaceButtonTemplate" id="2">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton1" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton3" inherits="CharacterCreateRaceButtonTemplate" id="3">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton2" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton4" inherits="CharacterCreateRaceButtonTemplate" id="4">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton3" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton5" inherits="CharacterCreateRaceButtonTemplate" id="5">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton4" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton7" inherits="CharacterCreateRaceButtonTemplate" id="7">
<Anchors>
<Anchor point="TOP" relativePoint="TOP" x="50" y="-50"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton8" inherits="CharacterCreateRaceButtonTemplate" id="8">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton7" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton9" inherits="CharacterCreateRaceButtonTemplate" id="9">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton8" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton10" inherits="CharacterCreateRaceButtonTemplate" id="10">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton9" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton12" inherits="CharacterCreateRaceButtonTemplate" id="12">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton10" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton11" inherits="CharacterCreateRaceButtonTemplate" id="11">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton12" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>
<CheckButton name="CharacterCreateRaceButton6" inherits="CharacterCreateRaceButtonTemplate" id="6">
<Anchors>
<Anchor point="TOPLEFT" relativeTo="CharacterCreateRaceButton5" relativePoint="BOTTOMLEFT" x="0" y="-10"/>
</Anchors>
</CheckButton>

Now if you did it right all your files should be correctly edited, you now want to copy those file into a patch, I assume you already know how to make a custom patch so I'll skip over that.. What you want to do now is add your files to a patch (all the files in the directory RacePatch or whatever you called it)

Now you could basically go into the game but it would say your files are corrupt, you need this:

and then copy the DBC files only to your core, you should now have this: http://modcraft.superparanoid.de/viewtopic.php?f=59&t=883 download and run as the instructions says.

Now you should have something like this if you did it right: http://img11.imageshack.us/img11/5614/27966a4a76ee4d6db776d6c.png

Done with client side modifications :-)

5. Server-Side Modifications

Now in this last part we want to edit the server side so you can actually play these races :-)

Go to SharedDefines.h in your core and open it up

Find:

enum Races
{
RACE_NONE = 0,
RACE_HUMAN = 1,
RACE_ORC = 2,
RACE_DWARF = 3,
RACE_NIGHTELF = 4,
RACE_UNDEAD_PLAYER = 5,
RACE_TAUREN = 6,
RACE_GNOME = 7,
RACE_TROLL = 8,
//RACE_GOBLIN = 9,
RACE_BLOODELF = 10,
RACE_DRAENEI = 11,
//RACE_FEL_ORC = 12
//RACE_NAGA = 13,
//RACE_BROKEN = 14,
//RACE_SKELETON = 15,
//RACE_VRYKUL = 16,
//RACE_TUSKARR = 17,
//RACE_FOREST_TROLL = 18,
//RACE_TAUNKA = 19,
//RACE_NORTHREND_SKELETON = 20,
//RACE_ICE_TROLL = 21
};

Replace by:

enum Races
{
RACE_NONE = 0,
RACE_HUMAN = 1,
RACE_ORC = 2,
RACE_DWARF = 3,
RACE_NIGHTELF = 4,
RACE_UNDEAD_PLAYER = 5,
RACE_TAUREN = 6,
RACE_GNOME = 7,
RACE_TROLL = 8,
RACE_GOBLIN = 9,
RACE_BLOODELF = 10,
RACE_DRAENEI = 11,
RACE_FEL_ORC = 12
//RACE_NAGA = 13,
//RACE_BROKEN = 14,
//RACE_SKELETON = 15,
//RACE_VRYKUL = 16,
//RACE_TUSKARR = 17,
//RACE_FOREST_TROLL = 18,
//RACE_TAUNKA = 19,
//RACE_NORTHREND_SKELETON = 20,
//RACE_ICE_TROLL = 21
};

Now find:

#define MAX_RACES 12

and replace with

#define MAX_RACES 13

The max_races variable is basically just the race number + 1 so if you had let's say Tuskarr for you highest number race it would be 17+1=18, pretty simple, ah?

Now find
#define RACEMASK_ALL_PLAYABLE \
((1<<(RACE_HUMAN-1)) |(1<<(RACE_ORC-1)) |(1<<(RACE_DWARF-1)) | \
(1<<(RACE_NIGHTELF-1))|(1<<(RACE_UNDEAD_PLAYER-1))|(1<<(RACE_TAUREN-1)) | \
(1<<(RACE_GNOME-1)) |(1<<(RACE_TROLL-1)) |(1<<(RACE_BLOODELF-1))| \
(1<<(RACE_DRAENEI-1)))

and replace it with:

#define RACEMASK_ALL_PLAYABLE \
((1<<(RACE_HUMAN-1)) |(1<<(RACE_ORC-1)) |(1<<(RACE_DWARF-1)) | \
(1<<(RACE_NIGHTELF-1))|(1<<(RACE_UNDEAD_PLAYER-1))|(1<<(RACE_TAUREN-1)) | \
(1<<(RACE_GNOME-1)) |(1<<(RACE_TROLL-1)) |(1<<(RACE_GOBLIN-1))| \
(1<<(RACE_BLOODELF-1))|(1<<(RACE_DRAENEI-1)) |(1<<(RACE_FEL_ORC-1)) )

Now you're done with the core mods.. Moving on to..

6. SQL Modifications:

Some querys you'd have to run (copy pasted from modcraft):
Adding spells to the Goblin Warrior (copied from the Human Warrior) :

SET @NEW_RACE = 9; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 1; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_spell` WHERE race = @NEW_RACE AND class = @NEW_CLASS ;
INSERT INTO `playercreateinfo_spell` (`race`, `class`, `Spell`, `Note`)
SELECT @NEW_RACE, @NEW_CLASS, `Spell`, `Note` FROM `playercreateinfo_spell` WHERE race = @COPY_RACE AND class = @NEW_CLASS;

Continuation:

6. SQL Modifications:




Some querys you'd have to run (copy pasted from modcraft):
Adding spells to the Goblin Warrior (copied from the Human Warrior) :*

SET @NEW_RACE = 9; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 1; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_spell` WHERE race = @NEW_RACE AND class = @NEW_CLASS ;
INSERT INTO `playercreateinfo_spell` (`race`, `class`, `Spell`, `Note`)
SELECT @NEW_RACE, @NEW_CLASS, `Spell`, `Note` FROM `playercreateinfo_spell` WHERE race = @COPY_RACE AND class = @NEW_CLASS;


Adding spells to the Fel Orcs Warrior (copied from the Orc Warrior) :

SET @NEW_RACE = 12; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 2; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_spell` WHERE race = @NEW_RACE AND class = @NEW_CLASS ;
INSERT INTO `playercreateinfo_spell` (`race`, `class`, `Spell`, `Note`)
SELECT @NEW_RACE, @NEW_CLASS, `Spell`, `Note` FROM `playercreateinfo_spell` WHERE race = @COPY_RACE AND class = @NEW_CLASS;


Buttons Action Goblin Warrior (copied on Human Warrior) :

SET @NEW_RACE = 9; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 1; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_action` WHERE race = @NEW_RACE AND class = @NEW_CLASS ;
INSERT INTO `playercreateinfo_action` (`race`, `class`, `button`, `action`, `type`)
SELECT @NEW_RACE, @NEW_CLASS, `button`, `action`, `type` FROM `playercreateinfo_action` WHERE race = @COPY_RACE AND class = @NEW_CLASS;

Buttons Action Fel Orcs Warrior (copied on Orc Warrior) :

SET @NEW_RACE = 12; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 2; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_action` WHERE race = @NEW_RACE AND class = @NEW_CLASS ;
INSERT INTO `playercreateinfo_action` (`race`, `class`, `button`, `action`, `type`)
SELECT @NEW_RACE, @NEW_CLASS, `button`, `action`, `type` FROM `playercreateinfo_action` WHERE race = @COPY_RACE AND class = @NEW_CLASS;

Starting location of Goblin Warrior (= Humans) :

INSERT INTO `playercreateinfo` (`race`, `class`, `map`, `zone`, `position_x`, `position_y`, `position_z`) VALUES ('9','1','0','12','-8949.95','-132.493','83.5312');

Starting location of Fel Orc Warrior (= Orcs)

INSERT INTO `playercreateinfo` (`race`, `class`, `map`, `zone`, `position_x`, `position_y`, `position_z`) VALUES ('12','1','1','14','-618.518','-4251.67','38.718');

Levels for the Goblin Warrior (copied on Human Warrior)

SET @NEW_RACE = 9; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 1; -- ID of the race where we copy datas.
DELETE FROM `player_levelstats` WHERE race = @NEW_RACE AND class = @NEW_CLASS ;
INSERT INTO `player_levelstats` (`race`, `class`, `level`, `str`, `agi`, `sta`, `inte`, `spi`)
SELECT @NEW_RACE, @NEW_CLASS, `level`, `str`, `agi`, `sta`, `inte`, `spi` FROM `player_levelstats` WHERE race = @COPY_RACE AND class = @NEW_CLASS;

Levels for the Fel Orc Warrior (copied on Orc Warrior)

SET @NEW_RACE = 12; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 2; -- ID of the race where we copy datas.
DELETE FROM `player_levelstats` WHERE race = @NEW_RACE AND class = @NEW_CLASS ;
INSERT INTO `player_levelstats` (`race`, `class`, `level`, `str`, `agi`, `sta`, `inte`, `spi`)
SELECT @NEW_RACE, @NEW_CLASS, `level`, `str`, `agi`, `sta`, `inte`, `spi` FROM `player_levelstats` WHERE race = @COPY_RACE AND class = @NEW_CLASS;

7. Misc

Now you're nearly done, your character can be setup and it'll be actually playable :-) Now I'm just gonna copy some stuff from the modcraft guide that might be useful here at this last post:

Display helmets
Open ChrRaces.dbc :
At the 7th column, you have the abbreviation of the race, eg Go for GOBLIN. This abbreviation is actually only used for display helmet. You can modify an abbreviation of existing race for the helmets appear, for example by replacing Hu.

Problem: The helmets are misplaced, such as too forward, too high, etc. ... To fix this, simply move the Attachment Point No. 11 (which corresponds to the placement of the helmet) on two M2 model race (GoblinMale.m2 and GoblinFemale.m2 for example). You can use Mod-It for this.

There are other more "clean" way to fix helmets but longer.

Languages and skills
Basically, you can not learn language or skills. Indeed, the game uses a verification which is configured for each Skills. We will have to modify all the skills you'll want that the race will be use.

We'll take the example of the Common Language (Skill n°98).
Open SkillLineAbility.dbc :
Find :
Quote:590,98,668,1101,0,,,1,0,0x2,0,0,,,

We must consider the 4th column, the 1101 value. Transform this number into binary (eg with http://www.michelcarrare.com/demos/converter.php) You will find in our case:
10001001101
Every "1" allows the Skill for the race with ID equal to the position :
IDRace : 11 | 10 | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1

1 0 0 0 1 0 0 1 1 0 1


It is necessary to put 1 for the ID of your race. For the goblin, its ID 9, so we have:
IDRace : 11 | 10 | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1

1 0 1 0 1 0 0 1 1 0 1

Then just reprocess this new value in decimal :
(bin)10101001101 = (dec)1357
So, we have this :
590,98,668,1357,0,,,1,0,0x2,0,0,,,


Hey! And if my ID is 14 to the race? How can I do?

Well it is the same way, by adding 0:
IDRace : 14 | 13 | 12 | 11 | 10 | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1

1 0 0 1 0 0 0 1 0 0 1 1 0 1


Open SkillRaceClassInfo.dbc :
Find :
Quote:40,98,1101,1535,0x80,0x0,0,0x0,

You see the 1101 in the 3rd column? Do as above.

Reminder
You must do this for all Skills that you want to allow.

Information
This technique can be used on your existing races, particularly for languages.

Allowing Reputations
Open Faction.dbc :
For example, suppose we want allow reputation Stormwind for Goblin, find :

72,19,1100,690,0x1,0x0,0,0,0x0,0x0,3100,-42000,4000,0,0x111,6,0x11,0x0,469,1.0,0.25,7,5,,,"Hurlevent",,,,,,,,,,,,,,0xFF01FE,,,"Cette capitale de l’Alliance est l’un des derniers bastions de la puissance des humains. Elle est gouvernée par le roi Varian Wrynn, revenu après une disparition mystérieuse.",,,,,,,,,,,,,,0xFF01FE,


Well take this part of the line :
1100, 690, 0x1, 0x0,[...] 3100, -42000, 4000,0,

Races 1 | Races 2 | Races 3 | Races 4 | Rep for races 1 | Rep for races 2 | Rep for races 3 ...
You understand? 1100 corresponds to all races of the alliance except humans who begin the game with 3,100 reputation. 690 races is the hord, they begin with -42,000 and finally 1 for humans starting with 4000.
So if we want Goblins (ID 9) can start with 3100 (and that reputation is allowed to be used for this race and saved by the server), you must edit 1100 by 1356 (detailed explanations in the activation of languages and skills).
We must do this for all Reputations. No worries, using "Replace All" with your favorite text editor, it will save time.
For example: 1791 means all races of the alliance. So if the goblin is in the alliance, we must add to all the "1791" be replaced 1791 by 2047. Then do manual handling to the capitals (as Stormwind).


Now you should be pretty much done, thank you for reading my guide hope you found it useful :-)

Credits:
Me
http://modcraft.superparanoid.de/viewtopic.php?f=26&t=165
