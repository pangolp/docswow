# [How to: Custom Races!](http://www.ac-web.org/forums/showthread.php?181048-How-to-Custom-Races!)

**1. Introducción**

Bien, así que ahora he estado tratando de hacer razas personalizadas durante una semana más o menos porque la única guía hecha fue para un parche anterior y las personas que sabían qué hacer no respondían a mis preguntas o simplemente me ignoraban. Así que ahora comparto lo que he descubierto sobre cómo crear tu propia raza personalizada.

Así que lo que tienes que entender para empezar es que no puedes tener todas las razas personalizadas como los cerdos o algo así, tiene que ser una raza que esté realmente definida, ahora probablemente me preguntarás: "¿cuál está definida?", bueno echemos un vistazo:

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

Esas son las razas actuales que se pueden jugar. Lo que voy a enseñarles hoy es cómo sumar dos de ellas: El Goblin y el Orco Fel.

Antes de empezar me gustaría decir un par de cosas:

Esta no es una guía fácil, no esperes que te dé información si no sabes lo que haces.
Proporcionaré información que encontré que funcionaba para mí...
No me importa si algo de lo que hago es "hacky".

**Obtener los archivos**
Bien, empezaremos a editar los archivos DBC, los que necesitas son:

 - ChrRaces.dbc
 - CharBaseInfo.dbc
 - CharStartOutfit.dbc
 - SkillLineAbility.dbc
 - SkillRaceClassInfo.dbc
 - Faction.dbc

Puedes extraerlos del núcleo/dbc, es lo mismo que en el cliente. También tendrás que obtener algunos archivos Lua y XML de los parches de blizzard para poder editar la pantalla de creación de personajes. Esos archivos son:

```
patch-enXX-3.mpq\Interface\GlueXML\GlueStrings.lua
patch-enXX-3.mpq\Interface\GlueXML\GlueParent.lua

patch-enXX-2.mpq\Interface\GlueXML\CharacterCreate.lua
patch-enXX-2.mpq\Interface\GlueXML\CharacterCreate.xml
```

Vale, genial, así que algunos de vosotros preguntaréis cómo extraerlos, lo que hacéis es conseguir vuestro editor de MPQ favorito, podéis usar el que queráis (buscadlo en Google si no lo tenéis) y abrir los parches con él, id a la ruta que escribí arriba en el archivo MPQ y arrastradlo a algún lugar de una carpeta para este proyecto vuestro.


```
Interface\GlueXML\GlueStrings.lua
Interface\GlueXML\GlueParent.lua
Interface\GlueXML\CharacterCreate.lua
Interface\GlueXML\CharacterCreate.xml

DBFilesClient\ChrRaces.dbc
DBFilesClient\CharBaseInfo.dbc
DBFilesClient\CharStartOutfit.dbc
DBFilesClient\SkillLineAbility.dbc
DBFilesClient\SkillRaceClassInfo.dbc
DBFilesClient\Faction.dbc
```

Ahora que tenemos nuestros archivos deseados bien alineados podemos pasar a la edición de los mismos.

**Edición de los archivos DBC**

Para esto, a menos que se indique lo contrario, usa tu editor favorito de la DBC.

Primero abre tu **ChrRaces.dbc**, verás que aparecen todas las razas que he listado arriba, ahora lo que quieres hacer es afinar estas 2 líneas:

```csv
9,1,1,0x0,6894,6895,"Go",7,7,15007,0x448,"Goblin",0,0x2,,,"Goblin",,,,,,,,,,,,,,0xFF01FE,,,"Goblin",,,,,,,,,,,,,,0xFF01CC,,,"Gobelin",,,,,,,,,,,,,,0xFF01CC,"NORMAL","NONE","NORMAL",0,
12,5,1,0x0,16981,16980,"Fo",1,7,15007,0x448,"FelOrc",0,0x1,,,"Fel Orc",,,,,,,,,,,,,,0xFF01FE,,,"Fel Orc",,,,,,,,,,,,,,0xFF01CC,,,"Fel Orc",,,,,,,,,,,,,,0xFF01CC,"NORMAL","NORMAL","NORMAL",0,
```

O algo parecido a eso...

Ahora quieres editar es como sigue:
 - 2ª columna, debe cambiar a las 12 para que la raza sea jugable.
 - La 4ª columna define la facción de la raza, ver **Faction.dbc**
 - Octava columna, 1 para la horda y 7 para la alianza.

Guardar y cerrar.
Abrir **CharBaseInfo.dbc**, este archivo determina los combos de clase/raza para tus razas. Así que lo que quieres hacer es por ahora sólo añadir 2 líneas:

```csv
9, 1
12, 1
```

Esto hace que el Goblin (9) pueda ser un guerrero y también el Orco Fel (12).
**Nota:** Si tienes problemas para abrir este archivo DBC Taliis hace el trabajo

Ahora que hemos terminado con eso pasaremos a los archivos Lua/XML, y aquí es donde comienza la diversión.

**Edición de los archivos de la interfaz**

Bien, lo primero que quieres hacer es abrir el archivo de **GlueStrings**, ahí encontrarás muchos valores que puedes editar y demás, pero mantengámoslo simple y agreguemos lo que necesitemos:

Busca:

```lua
ABILITY_INFO_BLOODELF1 = "- Enchanting skill increased.";
```

Y encima de eso añade esto:

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

Luego:

```lua
RACE_CHANGE_IN_PROGRESS = "Updating Race...";
```

después de esa línea inserta estas:

```lua
RACE_INFO_GOBLIN = "Information about Goblin males.";
RACE_INFO_GOBLIN_FEMALE = "Information about Goblin females.";
RACE_INFO_FELORC = "Information about Fel Orc males.";
RACE_INFO_FELORC_FEMALE = "Information about Fel Orc females.";
```

Y ya has terminado con **GlueStrings.lua**, ciérralo (recuerda guardar) y pasaremos a **GlueParent.lua**

Como veis, esto es lo que el tutorial de ModCraft no cubría, y es aquí donde mucha gente falla, por lo que completaré esta parte.

Primero quieres abrir **GlueParent.lua** y luego buscar

```lua
GlueAmbienceTracks["CHARACTERSELECT"] = "GlueScreenIntro";
```

Después de esta línea añada estos 2:

```lua
GlueAmbienceTracks["GOBLIN"] = "GlueScreenIntro";
GlueAmbienceTracks["FELORC"] = "GlueScreenIntro";
```

Ahora quieres ir por ahí:

```lua
CHARACTERSELECT = {
	{1, 0, 0.00000, 0.00000, -1.00000, 1.0, 0.15000, 0.15000, 0.15000, 1.0, 0.00000, 0.00000, 0.00000},
	{1, 0, -0.74919, 0.35208, -0.56103, 1.0, 0.00000, 0.00000, 0.00000, 1.0, 0.44706, 0.54510, 0.73725},
	{1, 0, 0.53162, -0.84340, 0.07780, 1.0, 0.00000, 0.00000, 0.00000, 2.0, 0.55, 0.338625, 0.148825},
},
```

y después de eso deberías añadir:

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

Esto añadirá la luz para que las maquetas estén más iluminadas y sean más bonitas. Después de esto quieres ir a:

```lua
function SetBackgroundModel(model, name)
```

y reemplazar toda esa función con:

```lua
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
```

Esto hará que el duende y el Orco aparezcan como si tuvieran el fondo de creación del personaje orco.

Ya has terminado con el GlueParent.lua y puedes pasar al CharacterCreate.lua, primero ábrelo y ve a

```lua
MAX_RACES = 10;
```
y editar eso para
```lua
MAX_RACES = 12;
```

Encuentra

```lua
["DRAENEI_MALE"] = {0.5, 0.625, 0, 0.25},
["DRAENEI_FEMALE"] = {0.5, 0.625, 0.5, 0.75},
```

Después de eso agregue:

```lua
["GOBLIN_MALE"] = {0.625, 0.625, 0, 0.25},
["GOBLIN_FEMALE"] = {0.625, 0.625, 0, 0.25},

["FELORC_MALE"] = {0.5, 0.625, 0, 0.25},
["FELORC_FEMALE"] = {0.5, 0.625, 0, 0.25},
```

Esto añadirá los iconos Draenei a la pantalla de creación de personajes, ya que no he descubierto el sistema de coordinación de la colocación de los iconos.
A continuación cerrará Charactercreate.lua y abrirá **CharacterCreate.xml**, que querrá encontrar:

```xml
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
```
y reemplazarlo con
```xml
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
```

Si lo hiciste bien, todos tus archivos deben ser editados correctamente, ahora quieres copiar esos archivos en un parche, asumo que ya sabes cómo hacer un parche personalizado, así que me saltaré eso... Lo que quieres hacer ahora es añadir tus archivos a un parche (todos los archivos del directorio RacePatch o como quiera que lo llames)

Ahora, básicamente podrías entrar en el juego pero diría que tus archivos están corruptos, necesitas esto:

y luego copiar los archivos DBC sólo en tu núcleo, ahora deberías tener esto: http://modcraft.superparanoid.de/viewtopic.php?f=59&t=883 descargar y ejecutar como dicen las instrucciones.

Hecho con modificaciones del lado del cliente :-)

**Server-Side Modifications**

Now in this last part we want to edit the server side so you can actually play these races

Go to **SharedDefines.h** in your core and open it up

Find:

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

Replace by:

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
```

Now find:

```c++
#define MAX_RACES 12
```
and replace with
```c++
#define MAX_RACES 13
```

The max_races variable is basically just the race number + 1 so if you had let's say Tuskarr for you highest number race it would be 17+1=18, pretty simple, ah?

Now find
```c++
#define RACEMASK_ALL_PLAYABLE \
((1<<(RACE_HUMAN-1)) |(1<<(RACE_ORC-1)) |(1<<(RACE_DWARF-1)) | \
(1<<(RACE_NIGHTELF-1))|(1<<(RACE_UNDEAD_PLAYER-1))|(1<<(RACE_TAUREN-1)) | \
(1<<(RACE_GNOME-1)) |(1<<(RACE_TROLL-1)) |(1<<(RACE_BLOODELF-1))| \
(1<<(RACE_DRAENEI-1)))
```

and replace it with:

```c++
#define RACEMASK_ALL_PLAYABLE \
((1<<(RACE_HUMAN-1)) |(1<<(RACE_ORC-1)) |(1<<(RACE_DWARF-1)) | \
(1<<(RACE_NIGHTELF-1))|(1<<(RACE_UNDEAD_PLAYER-1))|(1<<(RACE_TAUREN-1)) | \
(1<<(RACE_GNOME-1)) |(1<<(RACE_TROLL-1)) |(1<<(RACE_GOBLIN-1))| \
(1<<(RACE_BLOODELF-1))|(1<<(RACE_DRAENEI-1)) |(1<<(RACE_FEL_ORC-1)) )
```

Ahora ya has terminado con las modificaciones del núcleo... Pasando a...

**Modificaciones del SQL:**

Algunas consultas que tendrías que ejecutar (copiar pegado de modcraft):
Añadiendo hechizos al Guerrero Duende (copiado del Guerrero Humano):

```sql
SET @NEW_RACE = 9; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 1; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_spell` WHERE `race`=@NEW_RACE AND `class`=@NEW_CLASS ;
INSERT INTO `playercreateinfo_spell` (`race`, `class`, `Spell`, `Note`)
SELECT @NEW_RACE, @NEW_CLASS, `Spell`, `Note` FROM `playercreateinfo_spell` WHERE `race`=@COPY_RACE AND `class`=@NEW_CLASS;
```

Añadiendo hechizos al Guerrero Orcos Felinos (copiado del Guerrero Orcos):

```sql
SET @NEW_RACE = 12; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 2; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_spell` WHERE `race`=@NEW_RACE AND `class`=@NEW_CLASS ;
INSERT INTO `playercreateinfo_spell` (`race`, `class`, `Spell`, `Note`)
SELECT @NEW_RACE, @NEW_CLASS, `Spell`, `Note` FROM `playercreateinfo_spell` WHERE `race`=@COPY_RACE AND `class`=@NEW_CLASS;
```

Botones de acción del goblin, copiados del guerrero humano.

```sql
SET @NEW_RACE = 9; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 1; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_action` WHERE `race`=@NEW_RACE AND `class`=@NEW_CLASS ;
INSERT INTO `playercreateinfo_action` (`race`, `class`, `button`, `action`, `type`)
SELECT @NEW_RACE, @NEW_CLASS, `button`, `action`, `type` FROM `playercreateinfo_action` WHERE `race`=@COPY_RACE AND `class`=@NEW_CLASS;
```

Los botones del ORC FEL, copiados del ORC traduccional.

```sql
SET @NEW_RACE = 12; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 2; -- ID of the race where we copy datas.
DELETE FROM `playercreateinfo_action` WHERE `race`=@NEW_RACE AND `class`=@NEW_CLASS ;
INSERT INTO `playercreateinfo_action` (`race`, `class`, `button`, `action`, `type`)
SELECT @NEW_RACE, @NEW_CLASS, `button`, `action`, `type` FROM `playercreateinfo_action` WHERE `race`=@COPY_RACE AND `class`=@NEW_CLASS;
```

Ubicación inicial de Guerrero Duende (=Humanos):

```sql
INSERT INTO `playercreateinfo` (`race`, `class`, `map`, `zone`, `position_x`, `position_y`, `position_z`) VALUES
('9','1','0','12','-8949.95','-132.493','83.5312');
```
Ubicación inicial del Guerrero Orco Fel (= Orcos)

```sql
INSERT INTO `playercreateinfo` (`race`, `class`, `map`, `zone`, `position_x`, `position_y`, `position_z`) VALUES
('12','1','1','14','-618.518','-4251.67','38.718');
```

Niveles para el Guerrero Duende (copiado en Human Warrior)

```sql
SET @NEW_RACE = 9; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 1; -- ID of the race where we copy datas.
DELETE FROM `player_levelstats` WHERE `race`=@NEW_RACE AND `class`=@NEW_CLASS;
INSERT INTO `player_levelstats` (`race`, `class`, `level`, `str`, `agi`, `sta`, `inte`, `spi`)
SELECT @NEW_RACE, @NEW_CLASS, `level`, `str`, `agi`, `sta`, `inte`, `spi` FROM `player_levelstats` WHERE `race`=@COPY_RACE AND `class`=@NEW_CLASS;
```

Niveles para el Guerrero Orco Fel (copiado en Guerrero Orco)

```sql
SET @NEW_RACE = 12; -- ID of adding race.
SET @NEW_CLASS = 1; -- ID of class of the new race.
SET @COPY_RACE = 2; -- ID of the race where we copy datas.
DELETE FROM `player_levelstats` WHERE `race`=@NEW_RACE AND `class`=@NEW_CLASS ;
INSERT INTO `player_levelstats` (`race`, `class`, `level`, `str`, `agi`, `sta`, `inte`, `spi`)
SELECT @NEW_RACE, @NEW_CLASS, `level`, `str`, `agi`, `sta`, `inte`, `spi` FROM `player_levelstats` WHERE `race`=@COPY_RACE AND `class`=@NEW_CLASS;
```

**Misc**

Ahora que casi has terminado, tu personaje puede ser configurado y será realmente jugable.
Ahora voy a copiar algunas cosas de la guía de modcraft que pueden ser útiles aquí en este último post:

Abre **ChrRaces.dbc**:

En la séptima columna, tienes la abreviatura de la raza, por ejemplo, "Go for GOBLIN". Esta abreviatura en realidad sólo se usa para el casco de exhibición. Puedes modificar una abreviatura de la raza existente para que aparezcan los cascos, por ejemplo reemplazando Hu.

Problema: Los cascos están mal colocados, como demasiado adelante, demasiado alto, etc. ... Para solucionarlo, basta con desplazar el punto de fijación nº 11 (que corresponde a la colocación del casco) en dos carreras del modelo M2 (GoblinMale.m2 y GoblinFemale.m2 por ejemplo). Para ello puede utilizar el Mod-It.

Hay otras formas más "limpias" de arreglar los cascos, pero más largas.

**Languages and skills**

Básicamente, no puedes aprender el lenguaje o las habilidades. De hecho, el juego utiliza una verificación que está configurada para cada habilidad. Tendremos que modificar todas las habilidades que quieras que se usen en la raza.

Tomaremos el ejemplo del Lenguaje Común (Habilidad n°98).
Abre **SkillLineAbility.dbc**:

Encuentra:
```csv
590,98,668,1101,0,,,1,0,0x2,0,0,,,
```

Debemos considerar la cuarta columna, el valor 1101. Transformar este número en binario (por ejemplo, con https://es.calcuworld.com/calculadoras-matematicas/calculadora-binaria/)

Al convertirlo, obtendremos el siguiente valor.
10001001101

Cada "1" permite la habilidad para la carrera con una identificación igual a la posición:

| Draenei | Bloodelf | Goblin | Troll | Gnome | Tauren | Undead | Nightelf | Dwarf | Orc | Human |
|:-------:|:--------:|:------:|:-----:|:-----:|:------:|:------:|:--------:|:-----:|:---:|:-----:|
|    1    |     0    |    0   |   0   |   1   |    0   |    0   |     1    |   1   |  0  |   1   |


Es necesario poner 1 para la identificación de su raza. Para el duende, su ID 9, así que tenemos:

| Draenei | Bloodelf | Goblin | Troll | Gnome | Tauren | Undead | Nightelf | Dwarf | Orc | Human |
|:-------:|:--------:|:------:|:-----:|:-----:|:------:|:------:|:--------:|:-----:|:---:|:-----:|
|    1    |     0    |    1   |   0   |   1   |    0   |    0   |     1    |   1   |  0  |   1   |

Entonces reprocesa este nuevo valor en decimal:

(bin)10101001101 = (dec)1357

Entonces, tenemos esto:

```csv
590,98,668,1357,0,,,1,0,0x2,0,0,,,
```

¡Oye! ¿Y si mi identificación es 14 para la carrera? ¿Cómo puedo hacerlo?

Bueno, es de la misma manera, añadiendo 0:

| 14 | 13 | 12 | 11 | 10 | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1 |
|:--:|:--:|:--:|:--:|:--:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|  1 |  0 |  0 |  1 |  0 | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 0 | 1 |


Open **SkillRaceClassInfo.dbc**:

Find:

```csv
Quote:40,98,1101,1535,0x80,0x0,0,0x0,
```

You see the 1101 in the 3rd column? Do as above.

Reminder
You must do this for all Skills that you want to allow.

Information
This technique can be used on your existing races, particularly for languages.

Allowing Reputations

Open **Faction.dbc**:
For example, suppose we want allow reputation Stormwind for Goblin, find:

```csv
72,19,1100,690,0x1,0x0,0,0,0x0,0x0,3100,-42000,4000,0,0x111,6,0x11,0x0,469,1.0,0.25,7,5,,,"Hurlevent",,,,,,,,,,,,,,0xFF01FE,,,"Cette capitale de l’Alliance est l’un des derniers bastions de la puissance des humains. Elle est gouvernée par le roi Varian Wrynn, revenu après une disparition mystérieuse.",,,,,,,,,,,,,,0xFF01FE,
```


Well take this part of the line:

1100, 690, 0x1, 0x0,[...] 3100, -42000, 4000,0,

Races 1 | Races 2 | Races 3 | Races 4 | Rep for races 1 | Rep for races 2 | Rep for races 3 ...

You understand? 1100 corresponds to all races of the alliance except humans who begin the game with 3,100 reputation. 690 races is the hord, they begin with -42,000 and finally 1 for humans starting with 4000.
So if we want Goblins (ID 9) can start with 3100 (and that reputation is allowed to be used for this race and saved by the server), you must edit 1100 by 1356 (detailed explanations in the activation of languages and skills).
We must do this for all Reputations. No worries, using "Replace All" with your favorite text editor, it will save time.
For example: 1791 means all races of the alliance. So if the goblin is in the alliance, we must add to all the "1791" be replaced 1791 by 2047. Then do manual handling to the capitals (as Stormwind).


Now you should be pretty much done, thank you for reading my guide hope you found it useful :-)

Credits:
 - Me (Mathix)
 - http://modcraft.superparanoid.de/viewtopic.php?f=26&t=165
