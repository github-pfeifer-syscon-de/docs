
# Wow

## Arygos(Ali)

| Name | | Class | iLvl || Professions (pre Shadowl.)||Renown
| --- | --- | --- | --- | --- | --- | --- | --- |
|[Dolmina](https://worldofwarcraft.com/en-gb/character/eu/arygos/dolmina)|Lightf. D.|Retr.Pala|99|Eng*,Min(Enc,Her)|
|[Katnissx](https://worldofwarcraft.com/en-gb/character/eu/arygos/Katnissx)|Hum.|Mark.Hunt.|212|Alc*,Her|80 Kyr.|
|[Seralja](https://worldofwarcraft.com/en-gb/character/eu/arygos/Seralja)|Night E.|Hav.Deam.H.|99|Enc,Tai(Min,Her)|
|[Lalra](https://worldofwarcraft.com/en-gb/character/eu/arygos/lalra)|Kul Tir.|Bal.Drui.|135|Ins,Her|
|[Tooph](https://worldofwarcraft.com/en-gb/character/eu/arygos/tooph)|Darkir. Dw.|Enh.Sham.|96|Juw*,Min(Bla,Min)|
|[Têgidti](https://worldofwarcraft.com/en-gb/character/eu/arygos/T%C3%AAgidti)|Drae.|Arms Wari.|(67) 98|Jew(*),Min|
|[Pénny](https://worldofwarcraft.com/en-gb/character/eu/arygos/P%C3%A9nny)|Hum.|Retr.Pala.|(68) 96|Bla*,Min|
|[Liarà](https://worldofwarcraft.com/en-gb/character/eu/arygos/Liar%C3%A0)|Night E.|Bal.Drui.|(79) 98|Alc(*),Her|
|[Dopamina](https://worldofwarcraft.com/en-gb/character/eu/arygos/Dopamina)|Night E.|Arc.Mage.|(60) 99|Enc(*),Tai|
|[Jangtze](https://worldofwarcraft.com/en-gb/character/eu/arygos/Jangtze)|Pand.|Windw.Monk|(62) 94|Leth,Ski (Enc,Tai)|
|[Scantha](https://worldofwarcraft.com/en-gb/character/eu/arygos/Scantha)|Night E.|Mark.Hunt.|(62) 95|Let(*),Ski|
|[Casandrå](https://worldofwarcraft.com/en-gb/character/eu/arygos/Casandr%C3%A5)|Drae.|Elem.Sham.|(66) 97|Eng(*),Min|
|[Lelò](https://worldofwarcraft.com/en-gb/character/eu/arygos/Lel%C3%B2)|Hum.|Shad.Prie.|(62) 96|Ins*,Her|
|[Safinâ](https://worldofwarcraft.com/en-gb/character/eu/arygos/Safin%C3%A2)|Worg.|Warl|(68) 92|Alc,Her|
|[Smilia](https://worldofwarcraft.com/en-gb/character/eu/arygos/Smilia)|Worg.|Outl.Roug.|(64) 91|Let,Ski|
|[Necaraia](https://worldofwarcraft.com/en-gb/character/eu/arygos/Necaraia)|Void E.|Shad.Prie.|92|Enc*,Tail*|


(*) Indicates preferred skiling for profession

## Moonglade(Hor)

| Name | | Class | iLvl | Professions | Renown
| --- | --- | --- | --- | --- | --- |
|[Foxli](https://worldofwarcraft.com/en-gb/character/eu/moonglade/foxli)|Vulp.|Elem.Sham|353|Ins,Herb.|80 NF|
|[Dopaminla](https://worldofwarcraft.com/en-gb/character/eu/moonglade/dopaminla)|Pand.|Windw.Monk|342|Let,Ski|42 Necr.|
|[Zukala](https://worldofwarcraft.com/en-gb/character/eu/moonglade/zukala)|Zand.Troll|Bal.Druid|202|Enc,Tail|72 Vent.|
|[Tooph](https://worldofwarcraft.com/en-gb/character/eu/moonglade/tooph)|Vulp.|Mark.Hunt.|350|Jew,Min|17 Kyr.|
|[Kylire](https://worldofwarcraft.com/en-gb/character/eu/moonglade/kylire)|Nightb.|Shad.Priest|345|Alc,Herb.|38 NF|
|[Terica](https://worldofwarcraft.com/en-gb/character/eu/moonglade/terica)|Magh.Orc|Arms Wari.|220|Eng.,Min|11 NF|
|[Kagura](https://worldofwarcraft.com/en-gb/character/eu/moonglade/kagura)|Blood E.|Retr.Pala|187|Bla,Min|21 Kyr.|
|[Dakala](https://worldofwarcraft.com/en-gb/character/eu/moonglade/dakala)|Blood E.|Hav.Daem.H.|355|Min,Eng.|20 Vent.|

## Addon

### Waypoint

Thanks to Astrumqt there is lighweight WaypointsNative Addon, i tried to improved it a bit:

[Media:WaypointNativ.zip](Media:WaypointNativ.zip.md)

### Quests

This is a bit crude addon as help to play loremaster (i coud not find any simple addon to do this), mainly it shows a notficion in green if you accept a quest the is for loremaster (at the moment only some Outland/Nortrend/Pandaria regions are included you might need to get the quests number from e.g. https://www.wowhead.com/quests/outland/blades-edge-mountains/side:1?filter=44;1;0 "the ids" are needed, extract these with your prefered language).
To show what quests that you currently track/watch are on the storyline use:
```
/quests 
```
to get some indication of your progess and still open quests of the storyline use:
```
/quests open
```

[Media:Quests.zip](Media:Quests.zip.md)

This stands and falls with the provided quest infos (my hopes that the wow-api woud give these infos where in vain (of course someone else woud have constructed an Addon in this case)). 
To obtain the info what quests are required can be a bit frustrating, upto Outland these infos are quite accurate, with Pandaria they get a bit confusing (on Wowhead look for the achievement and a required criterias, and look for a second page in some cases). But still there is some creativity required, so dont use these infos as complete list, but as a guide to get to the next quest hub (if you pointed to a quest-giver the also offered quests are most likely also belong to the storyline), (breadcrumb quests are missing mostly). And identify a quest-giver with the infos the Addon provides, works most certainly.
You may decide:

a) just do what is obvious. This may required to go back to the quest area (when the ! popup appears on the map)

b) do the quests for a area at once they might not be required, but its afterall faster, as the most tasks somehow overlap (going into a cave to find something in the end, and realize if you get back the quest in parallel requested you to kill the mobs there... can be anoying at least for me)

Also included is a quest logging function that writes the quests your accept and turn into "Account/YOURNAME/SavedVariables/Quests.lua" (if you do many quests this grows so, you may put this info somewhere else after some time, or comment out the event handler or disable the Addon when not needed) (the idea was to get some better infos what quests are required for a criteria...).

Here are my infos for some Northrend areas (if you want to do some datamining ;)

[Media:QuestInfos.zip](Media:QuestInfos.zip.md)

This is definitly not "completed" but i hope it helps or gives you an idea for your own addon.

(c) Quest infos Blizzard/Wowhead

### GamePad

Since Bfa there is a nativ option to use a gamepad in game. 

helpful are the console variables from [https://wowpedia.fandom.com/wiki/Console_variables/Complete_list https://wowpedia.fandom.com/wiki/Console_variables/Complete_list](https://wowpedia.fandom.com/wiki/Console_variables/Complete_list https://wowpedia.fandom.com/wiki/Console_variables/Complete_list.md) look for GamePad  use with 
```
 
/console NAME VALUE
```

It allows for example to active the the more wsad like navigation by setting 

```
/console GamePadTankTurnSpeed 1
/console GamePadAnalogMovement 1
```

the greater the value for turnspeed gets the  more sensitive the navigation becomes.

The Consoleport Addon might be helpful but it changes the whole UI (and it doesn't help you with some controller that isn't on the list).

The most obvious customization are the keybindings, if you happy with these no need to read further (or you never changed a config file with a editor before).

To support a aged PS3 controller use the .inf files provided from [https://vigem.org/projects/DsHidMini/ https://vigem.org/projects/DsHidMini/](https://vigem.org/projects/DsHidMini/ https://vigem.org/projects/DsHidMini/.md) otherwise the device is recognized but in limbo mode (it's shown but it will not allow any input).

The support for controllers is fine if you use a well known controller, if not the tool from [https://www.generalarcade.com/gamepadtool/ https://www.generalarcade.com/gamepadtool/](https://www.generalarcade.com/gamepadtool/ https://www.generalarcade.com/gamepadtool/.md) might be helpful. At least for my aged controller it corrected the navigation direction.
The created string goes into WTF/gamecontrollerdb.txt, but the model of the controller is somewhat simple. 

To understand the mapping (especially the more complex json kind) the following Addon might be helpful:

[Media:GamePad.zip](Media:GamePad.zip.md)

To modify it, use the WTF/GamePadMapping_Default.json as template copy it and give it your own name e.g. WTF/GamePadMapping_Modified.json. One important part is to add to the raw mapping containing vendor+product a name so if you active the addon with 

/gamepad 

you may see the name you choose above (the name will also be displayed with the connect message within the chat frame). If the json is somewhat wrong it will revert to the default config (use a json validator will be most helpful). With the displayed values it shoud be easier to get around the effective mapping and tailor it to your needs. Especially the modifier key PADxTRIGGER and the axis assigment have (as far as i can see) no other way to be modified. 

The Button mapping is straightforward it will just assign the supported names to a raw button number e.g. 

```
    "rawIndex": 3,
    "button": "PAD1"
```

For the axis mapping there is a inbetween symbolic name in raw assigned e.g.

```
    "rawIndex": 0,
    "axis": "RStickX"
```

these axes are further customized within "axisConfigs" in global settings e.g.

```
     "axis": "RStickX",
     "deadzone": 0.05
```

in "stickConfigs" the axes are bound to a in game function e.g.

```
     "stick": "Camera",
     "axisX": "RStickX",
     "axisY": "RStickY",
     "deadzoneX": 0.05,
     "deadzoneY": 0.2,
     "deadzone": 0.25
```

or Movement, ...

On extra case are "buttons" that have analog values as it seems that was for some time quite common (on DS3 lower shoulder buttons) these are assigned in "common" section to a function like this:

```
      "axis": "LTrigger",
      "deadzone": 0.12,
      "buttonThreshold": 0.5,
      "buttonPos": "PADLTRIGGER"
```

## Loremaster
### Chromie

At first it looks nice, to take the offer from chromie and explore some old expansion. The Levelups occure as fast as if you are questing newer content. 

With the scale content to player you can go anywhere, without asking chromie. The thing that changes is what dungeons you are able to join with LFG. But most of the time the message apears "Do you want the expand the search to other expansions?". 

But one thing will that isn't that much adapted to the speed of leveling is the gold you earn while questing. You might say i don't want to get rich, but flying isn't cheap, and if you don't have a leveled up char with sacks of gold around it may take some time to get ready for take off. The newer expansions are better in that aspect, and the quest rewards are better also.

#### Starter account

For completing the "Loremaster ..." achievements, they can be done mostly with a free start account (in many areas you get "stuck" e.g. nagrand requires lvl25 for some quests :( but shows you the promissing ! on the map). 

If you think i have done this before (i played these areas in 201x), they all have been scaled to super simple, no scary underground "dungeon" like encounters, and the mobs have just 600~700 health altogether not too dangerous.

But i suggest to get your equip with legion or higher with the new scaled items these are lvl 61, and you get them relativly easy (the further your advance in a chain of quests, the better are the rewards). The classic rewards might still be of some use as you get rings with your primary stat (but some simple items may take a 1000 quests or so until you get them with a lvl 57(in classaic thats epic).).

Playing a tank class is useful, as this avoids being dismounted while going through a group of mobs (and youre less likely going to die, when you face 5 or more mobs in cases when you need to stop).

* wowpedia.fandom.com  has the best structure to identify the storyline, and distinguish main and side quests (but this is a guide no ultimate rule, required side quests are not always marked as such (the quality varies from area to area, but the eastern kingdom worked best))
* iceyveins
* wowhead has a simple list of required quests, it is best used if you get stuck, the comments are always helpful ( https://www.wowhead.com/quests/outland/blades-edge-mountains/side:1?filter=44;1;0 i found it most useful: filter your faction + loremaster see above for a simple addon)
* if a NPC did offer you some quest from the storyline the following quests offered will also belong to it (seems true for 99%)
* the exclamation mark on the large map is always a indicator for the next quest (and only for the next chapter, see quest log)

#### Gold limit

Anonying is the gold limit, but as always there is a way to bend the rule:

* Buy expensive profession reagents e.g. "luminous fluxus", you can either use them once you have a subscription, or sell them (ceratainly with a loss), but better than being unable to sell the quest rewards

#### Areas

##### Badland
* After all, a not so bad storyline
##### Desolace
* The most classic storyline

##### Shadowmoon valley

* The most zigzag in and out of dungeons (over all of outland)

##### Northrend
* The storyline with the most elite: (Not speaking of Zul'Drak and the Arena (half of the bones on the gound must be mine..., the first oponents are soloable, the forth requires a heal (i tried multiboxing) the last vlad-something is altogether nasty. Final resolve: Level to 35, leave timewalking then he stops make you farming rep costs (and as a last twist, if you defeat him and leave the arena the quest fails (but using your homestone (in my case after some time) worked, nice to return this quests on timewalking and get the right reward not just a toy ;).

#### Suggestion on chromie/timewalking

* use timewalking allows leveling some alts
* some quests are flagged for five players, some may still be soloed, others not
* do them without timewalking
* on turning the quests in, ensure you are timewalking, so you get the full reward, and XP
* and there is the answer why chomie is located so impractical

# A brief history of expansions

## Classic

If you don't know the history of Wow, careful spoiler...

And i must admit i'am just a casual player (if you are not, most of this might not be of interest to you). 

## The burning crusade

Takes place in Outland, fight the burning Legion.

New races Draenei and Blood Elves.

## Wrath of the Lich King

Takes place in Northend, defeat the Lich King, and his followers.

New class Death Knight.

## Cataclysm

Mostly the existing area of Klaimdor and the Easter kingdom where modified.
Return of the evil dragon aspect Deathwing the Destroyer. 

New races Worgen and Goblins.

## Mists of Pandaria

Takes place on the new continent Pandaria. Fight the Sha and the fallen leader of the horde Hellscream.

Added Race Pandaran, and class Monk.

## Warlords of Draenor

New Word Draenor, once the home of the Orc race (Mag'har orcs).
Fight the iron horde.

## Legion

Takes place on the Broken Isles.
Added class daemon hunter.
Fight the legion once again.

### Cinematics

With the intro to teleport Dalaran from Dead wind pass you see the city attacked by legion ships. But why is it attacked here, shoud this not happen after the city was teleported ?

### Professions

If you are not into completing everything, just don't do professions in Legion.

Sometimes i don't really understand the concept. 

With legacy Wow you need(ed) to go to a teacher to learn each new spell, this was removed some time ago, so nice.

With legion this was reintroduced for professions where you need to see a teacher to learn and get send back somewhere (not speaking of alchemy where you can only create potions in Dalaran...).

### Azuna

Save some dragon, meet the lost prince.

For all those who came to know Queen Ashara before here she is (again). E.g. before seen briefly while questing in Darkshore.

### Valshara

Some replacement for the lost Night-Elve home (lost in Bfa)?

But not as peaceful as the former Tree.

Night-Elves lost cultures:
* one part lived in Azuna but their prince led them into a pact with Queen Azhara, that finally led to their extinction (they are all ghosts now)
* one part lived in Suramar they created a shield against the legion, and lived in isolation, the missing sunlight led them into dependency from the crystals
* Valshara gets populated by Night-Elves ancients who had their home in Teldrasil before
* in Bfa we meet in Nazathar also some extinct Night-Elve culture

### Stormheim

Resembles some scandinavic traditions but were they all so bloodthirsty? 

(At least the movie "King Arthur" suggests something similar).

### Highmountain

Nice home of the Highmountain Tauren.

But why are while questing nearly 75% of the enemies harpies?

One quest that probably is worth to visit highmountain for, is "Addies ink stained stachel" at 40 52.3, killing the black goat will get you a 26 slot bag :). After Bfa Nesingwary get's his (well earned) retirement.

### Suramar

Fantastic looking, nice stories... but lengtly (I always feelt a bit arkward about collecting crystals and offering them for a "fix"). 

Best played at Level 50 when you don't need to worry too much about dying.

### Broken shore

The storyline isn't much of story, just many going there and here ... 

The following expansions do better than offering quests like kill 50 demons, go there, come back, now kill 100...

### Argus

Gives a bit of space feeling, some nice secrets e.g. [https://warcraft-secrets.com/guides/legion-secrets Legion secrets ](https://warcraft-secrets.com/guides/legion-secrets Legion secrets .md) wowhead offeres them also if you know where to search.

## Battle for Azeroth

Add races, Void Elves, Lightforged Draenei, Dark iron dwarfes, Kul tiran, Mecha gnomes, Nightborn, High mountain Tauren, Mag'har orcs, Zandalari and Vulpera.

Enemies on all sides, one is N'zoth.

A setting that has been inspired by the age of the pirates, and it's a better version of Stranglethorn that also has Trolls,  Pirates and Naga (King Rastakhan, and even some phrases reapear). A few years can do wonders.

Without falling for someone like "Jack Sparrow". While "Flynn Fairwind" still resembles some attributes of a Adventurer. 

But why is the story so much fixed on getting in and out of prisons ?

### Professions

The idea of recycling is quite nice, but on the other hand if the materials for crafting were also increased, it leaves no advantage at all. 

I just managed to get tailoring at some level where crafting one item for my char did offer some improvements (even with nine Alts and recycling i spend still thousands gold just to get some skill points). Professions are only nice for Alts to get better items from daylie quests and these items are replaced after some hours... (probably the content is meanwhile scaled for a faster success, but professions weren't (i was playing Bfa while Shadowlands was out) )

Nazoth visions, within the heart chamber offer something like single player instances. But even with some item level ~100 they are still not simple, as there is always the time limit as your sanity runs out, and for a success you have to kill the end boss (you need to calculate time for that while doing the other objectives).

That might be a historic note: To get the required reputation,  for flying took me four weeks (i know i've been sloppy while leveling, and stayed for some levels in Legion, as it is more fun leveling when you can fly, and the rewards with item level 50 to 52-54 did not make such a big difference, be careful using the "D4KiR Move and Improve", nice addon that allows reconfiguring your frames easily, but the item level displayed is way off for legion artifact weapons (it might have been matching the weapon before the artifact powers wer disabled, -> better check by shift)).

### Kul Tiras

Represents some aspects of the empires of that age ? 

With a leader that doesn't knows what is really going on.

### Zandalar

For me it pictures some aspects the culture of middle and south America at that age.

One major threat to the Zandalar Empire are the blood trolls. 

Their white skin, makes them a historic quote, that is more than just some coincidence.

At first i was thinking "Zandalar forever" did relate to the duration of the Empire, but while playing the storyline i realized it is related to that, always one more revolt...

### Nazathar

The under the sea adventure. 

And here she appears again Queen Ashara, this time with more "legs".

Does every villain keeps reappearing until they are defeated in a raid?

But stop, some like "Illidan" or "Bovar Fordragon" get a second life as hero...

## Shadowlands

A setting that tries to go beyond the visible world, but beside the background story looks amazingly similar to other parts of Wow (but that's ok, as i don't want to download more extra gigabytes, or exchange my hardware for every expansion). 

One remarkable quote is "And everyday arriving new brokers".

### Leveling

At first it feelt a bit anoying to be forced to decide if you want to play the storyline or the threads of fate.

The storyline offers a on rails experience the pros and cons are (about reintroducing some previously ditched flaws see above, this time areas that work fine just for some levels, combined with the requirement to do everything in the given order): 

+ you never have to search what to do next

+ the quest givers are never far and some even come around

+ often multiple quest can be done in one place

- inis are excluded from the scale content to player as well

- if you get level ups too fast (rested,inis,sidequests) the quests won't offer much xp

Threads of fate:

+ the xp from quests is higher, than playing the storyline

+ you gain some renown levels beforehand 

+ accepting all area aiding quest, will allow you with lvl 60 to advance these without much effort and they still reward you some weapon (e.g. nice if you need two) and a anima container.

- the needed mobs might not spawn as fast as they should, others player (other fraction) might hit them right before you do, some drop rates are like "classic"

- you might not get the right items, as some rewards are only offered at the end of a series of quests (better are scenarios, these offer some rewards that you can see before)

- you can't go back to the storyline, and these quests are locked

Playing the storyline until it isn't rewarding worked best for me.

### Professions

One nice aspect about Shadowlands is that professions are not so hard to skill (as some limit the prices for some materials are now higher (go back where you have some reputation for buying saves you some gold)).

The concept of collecting "Soul ash" from Thorgast to craft "epic" items makes professions worth raising.

Also nice, you can select the level, but the curve is rather steep. At least for me and wearing anything below mail armor, and choosing the level closer than 10 points to my item level, didn't work nicely (expect some deaths as some groups of elites may be hard to control). The rare mobs are at least for me out of reach in these scenarios. Selecting some lower level may be easier than have to abandon a run, halfway through (which offers no reward at all). From a selected item level 150 to 160 there are added diffculties, like you have to keep moving or will be struck by something. The difficulty ranges from wearing cloth and need time to cast = worst, to wearing mail or better and you are free to move at any time (usually doing melee damage) = best. 

Lastly the auctions for materials where merged from all realms, this makes materials worth buying, but is bad if you want to sell (keep on eye on how much you spend to craft something the prices for auctions might be lower).

### The Maw

Some scenario where you start with all sort of hinderances.

From Legion Helya appears again, but only half as dangerous as she was.

### Bastion

The names, clothing and mythology (e.g. ferry souls) are greek.

Were the greek so much into bathing (beside archimede)?

Vespers make me think about a second breakfeast (makes only sense if you speak some german).

As connection to former expansions Uther/(Arthas) make their appearance.

### Maldraxxus

Some replacement for "Undercity" without the under. 

Each and everyone is getting on each others throat.

In Draenor, Draka stands at the fire in your Garsion. (Has she always been there? Or did they adjust the matrix? (hope it's not as bad as 1984.))

### Ardenweald

This lets me think i'am in the disney movie "The ice queen".

But the looks of the trees are amazing.

Part of the story about Ysera in Legion gets replayed and carried further.

The Drust from Bfa reappears.

### Revendreth

At first i was a bit shocked. Really vampires???

As i was young there were black&white movies with vampires. And now its obvious i'am getting old...

First you work for the one bloodsucker, than for the other. Dear story writers do you want to tell use something?

Here Kael'thas Sunstrider makes a reappearance, and that might explain why shadowlands is what it is, it allows any character from former expansions to reappear.

### Zereth Mortis

Inspired by Star Wars you even get your own ball droid.

# Dragonflight

Nice new Gui editing option that will make Move and Improve obsolet. But be careful do not change to much as there not all optional frames editable and some hidden constraints that work only nicely if the given layout is not changed to much. But the provided "Modern" layout contains most customisations i did make to my Gui :).

While questing you will realize that there is some changes in questing it now more based on expaining what is going on (the story previously more or less in the background will be explained in some places if you take some time), and last it is less scanning the quest and jump to target area on the map (kill X because of ...). And many NPC's are able to explain what they are doing. You will even will be asked if you get the names for the clans in the Ohn'ahran Plains right (choosing it wrong has no "bad" consequences...).

## Macro

They did break the macro api now my favorite "Sell crap macro" is broken, here is my fix, due to the size limitation the message what was selled is gone:

```
/script for bag = 0,4,1 do for slot = 1, C_Container.GetContainerNumSlots(bag), 1 do local name = C_Container.GetContainerItemLink(bag,slot); if name and string.find(name,"ff9d9d9d") then  C_Container.UseContainerItem(bag,slot) end; end;end;
```

(meanwhile integrated into Blizzard Ui...)

## Dragon riding

Thus is the fligh simulator for Wow (standard flying mounts wont work)

- Nice: to improve your abilities you have to collected glyphes, these count for the whole account (suggestion: use handy notes + dragon glyph notes to farm the gylphs)

- Nice: You can test your skills by racing

- Bad: Flying is momentum based (you can travel only some limted distance, and reach a limit height), so you will spend much time, just waiting for your vigor to recharge (gets better when you completed the basic storyline). But one thing i still miss is the ability to hover... (bad while farming materials)

- Bad: Evoker have only some limited ability to fly on their own, they are also depend on dragon riding (seems a bit weired i'am a dragon, but i cant fly ... far). Seems strange for one part dragons are highly evolved beings on the other they are mere "pets" to get you from there to here.

- Nice: Flying is more based on real physics, but one thing that is diffrent from the real thing, if you bash into the ground at max speed, you stop at once ... (Landing/braking flaps woud be nice (this has been added in the meanwhile))

- Bad: If youre dead you return to the former riding which always feels wrong.

As this is the first extension for some time (since legion) i play while it's relativly new, there still some things that need adjusting. One thing that took me some time until found is the Race "Emberflow Flight" won't work (the rings at the end just disappear) while the quest to take the obsidian citidal (wrahtion/sabelian) is open! Looking for wowhead comments saves you some time if anything feel strange while youre in the matrix.

One suggestion is to use one of the extra action bars to put the riding actions on (with your own key-bindings), what for me felt difficult to relearn  was the changing of the action bar while mounted (the quickest way to dismount was previously just attack). If the primary actions are now assigned like unmounted, this will make just attack to work like before.

If you reach renown six you may complete dragon races and get some reward (mainly gold), so this is the only place where you get payed for speeding (if youre not "The stick").

What have they done, if you get back to "normal" flying it feels like creeping.

## Comment

The overall atmosphere of this expansion is much more consistent than the Shadowlands. Shadowlands presented you four different main areas for questing with not much in common (2 nice, 2 bad). Here you meet some races from former expansions (ahrg, the gnolls are back) and it feels allright, and the major opponents are all new (The twist that Arenweald had to fight against rebel Faes, the Drust (including witches, hey what is the sense of a expansion if you fight the same foes again), the jailer, the anima draught and the Gorm? (worms) feelt as too many dangers). (This is biased as i played the shadowlands near its end, so here these diffrent features are added one by one...)

And a other major difference is that this expansion tries to be funny, and for some part this works, like the thuskar cooking event its fun all the way. Who did say that too many cooks will spoil the food? You need them all to make it epic!

The world-quests have some nice tasks, not just kill X of Y, one is sucking up slime like Ghostbusters  (but they are fewer)...

The concept of the collaborative events, happens now in many places, which by itself is nice, but sometimes you find yourself in an event, that can hardly done alone, and than it will be just about farming rep costs (the sharding of servers seems not always predictable, and the adaption to how many players are online may only be true for the end of timerifts (isolated area), new events e.g. the fractures in time get the highest attention so its hard to hit a mob, as there are hundreds of players doing the same thing. For others e.g. reserachers under fire you find yourself with just one other player, and this is just very hard to progress e.g. expensive).

## Renown

You can turn in the following items:

Dragon Isles artifact: Draconscale basecamp /way The Waking Shores 47.6 82.7 

Centaur hunting trophy: Maruukai /way Ohn'ahran Plains 64.01 41.04 

Titan Relic: Valdrakken Accord /way Thaldraszus 25.8 40.0 

Sacred Tuskarr Totem: Iskaara Tuskarr /way The Azure Span 12.4 49.2 

Sabelian/Wrathion /way The Waking Shores 25 56.4

Keep questing as these will reward up to 1.5k rep.

## Rewards

Just some overview how to improve your gear at lvl 70.

### Crafting

* with the spark of shadowflame (build from 2*splinters?) you can craft items upto ~420 (you can choose the best stats for your class other items are changeable into some fixed presets)
* weapons require two spark's

### General options

* World quests (at least if you just got 70)
* Fyrakk forces event (place is changing)

### Valdrakken

* offers a five heroic dungeons weekly, that will give you renown and some items.
* the weekly in the central place offers renown for all factions

### Waking shores

* the dragonbane keep siege event offers items up to explorer/adventurer lvl?

### Forbidden reach

* There are diffrent "vaults" with lots of elites that drop random account bound class tokens
* these items are onyl upgradeable by two steps with "frobidden knowledge" items?

### Zaraklek cavern

* The weekly quest offers some explorer (max 398), adventurer (max 411), veteran (max ~420) or champion item (somewhat random, depending on your actual iLvl?)
* these items are upgradeable with flighstones and dragon crests build from fragments (rewarded in different places)
* the event is "researches under fire" only feasible with some people around (if you can solo this you don't need the reward ...)

### Thaldrasuz

* The time rifts event offers some items (random, same as above?) 
* and time capsule reward (check buff or item  to get buff) that can be exchanged for the items you want 
* also paracausal flakes that are exchangable for equip (useful to get trinket upgrades easy) or cosmetic items

### Ohnaren pains

* the dream surges event offer tokens exchangeable for equip
* also dreamsurges coalesces can be exchanged for equip or cosmetic items

## Professions

Are a whole game on their own (previously i wrote mini-game but that is wrong), and collecting stuff is harder as before, as dragon riding doesnt allow to stop to check the area or climb to any level. You might even level by doing just your profession.

Here are the now used "currencies":

- as before skill points, granted by crafting items

- knowledge granted at various occations (e.g.  while crafting, from random world drops), usable to enhance your profession mastery or specialisation

- mettle (e.g.  while crafting) at 100 (or more) pieces, it is useful to enhance skill for specific crafts by "illustrious insight"

Thats new: Unlike previous expansions, here your job might kill you. e.g. when collecting herbs the windswept variant will kick you around. As some are found high up, you will fall a great distance (if think your are clever and mount a dragon, that will slow your fall, you are wrong, you will crash straight into the ground ..., the only thing that helps is the levitate cast usable by priests).

While leveling is a piece of cake, and there were many complaints with a "endless" process for getting to fly in Bfa, here is the new keep playing ...

But i dont want to bash it, most professions offer after half the skill points worthwhile rewards, that are equal to those you get at lvl 70 from dungeons, and later equals those you may get by raiding (but the requirements are not easy to fulfill e.g. khaz'gorite ore and elements (some more than others, and mining will offer no decay,air and frost, herbalism no fire or earth) are hard to farm (it might get better with all the sub specs filled with knowledge points (and some points are carefully hidden by different categories with names/descriptions that might not reveal the complete purpose on first sight). But after 4month playing DF it is still for me a long way to go to fill all useful categories)).

The system of crafting orders allows you to get equip, for a resonable price (not like thousends of G like auctions). At first i was a bit hesitant, as for myself i didn't see any orders ). But at last i tried it, and got my stuff crafted for 40-50g, the mats i had the most around, others i had to buy but that was no real issue (a real alternative to instances). The orders i put in, where crafted in some hours by the same char, so not too many seem to be looking for these orders (that might be due to some people found a way to do this in a addon/macro/script as the crafting in my case was done real quick) ...

After all i think the system is somehow broken, i got only very few orders, so one seems to place crafting orders for simple stuff (e.g. profession equip), instead the items are sold for some unreasonable prices in the auction house (something that takes some dozend pieces of metal for +2k gold)...

Nice it is possible to craft raid ready equip with the stats that are right for your class, with specific traits e.g. armor spikes that enhance your gear event further than just enchanting. But some of the material requirements e.g. for leatherworking are obscene like 150 pieces of leather (and i'am not Marvin form the hitchhikers guide, but just a pandaren, for gnomes it must be hard to stay upright with such a headpiece).
### Nice finds

Profession masters: [Profession masters](https://www.wowhead.com/guide/professions/knowledge-points#hidden-profession-masters)

Silken Surprise /way Ohn'ahran Plains 65.5 53.5 Reward 3 tailoring knowledge (use the katnip to distract prowler)

Decaying brackenhide blanket /way The Azure Span 16.1 39.2 Reward 3 tailoring knowledge (need to get marqee e.g. land on top to get it)

Itinerant Singned Fabric /way Dragonbane Keep 25.1 69.8 Reward 3 tailoring knowledge (up in the tree)

/way Ohn'ahran Plains 82.4 50.7 5 letherworking knowledge

Farm Rousing Air \way The Azure Span 46.6 59.4 

Drangon shard of knowledge, Khadin /way Ohn'ahran Plains 51.8 33.1

### Cooking and roleplay

They tell you, we got back to the basic of RPG, but it shows up in unexpected places:

I was a little bit shocked by the fact the my gooey chocolate i stored, in my bank vault, detoriorated to little morsel of chocolate and some vanished completly (surely it tells you "chocolate will melt", but there are many such explanations that never come true) (on the other hand, i was not sure that the cheese&rat quest i did before might have caused this (can anyone without the rat confirm this? or without pets/toy, please?, or was it my neighbors pet?))...

The fishing daily, with the lost net (need f key to interact), looks like a bug, but it resembles thar fact that lost fishing nets cause real damage in nature, i think.

I'am a bit concerned what the next, expansion will bring, here some suggestions/nightmares:

- logging in after some offtime, you had food from three or more expansions in your inventory, do want to migrate to a new server (paid service) and leave your belongings behind?

- you sold food from your inventory that has been around for three or more years, your server will be down for some days so the population can recover

- a refrigerator beside the bank vault that keeps your food fresh (paid service)

- a shopping channel with cooking knives that are more steadfast than the offered sharp cutting knive (i'am bit concerned it gets used up when the food is prepared where did the serevite go?)

- five additional cooking accessory knives, apron, pots, gloves, mask....

- cooking specialization like sushi, patisserie, ramen, barbecue

- auction house categories: tuna normal, tuna MSC 

- why there is no vegetarian cooking?

## The waking shores

The storyline is is centered around "eggs".
And the most irritating question "Is this an egg?" (if you can't tell as a dragon, what should i say as a human or...).

## Ohn'ahran Plains

Inhabited by centauren. As often they had a first appearence in Thousend needles and Desolace, now they look more convincing.

## Azur span

Resembles Nordend and the Nexus

Reputation quests:

```
/way #2024 51.7 62
```

## Thaldrazus

There your find the dragon capital

## Forbidden reach

The new "Thorgast tower"? 
The keys can easly be purchased (no need to kill elites anymore...).

## Zaralek cavern

Inhabited by moles from down under ... (will be the real aussies be happy with this characterisation?)

"Buddy system, don't wander off alone..."

Do the wrathion/ebyssian/sabelian (2500 rep.) and e.g. swallow quests  (1000 rep.) these are otherwise not easy to get.

# Genshin impact

For a change try this. Compared to Wow it is heavy on the role play side (if you don't like lengthy conversations or spend hours for a quest, this might not be for you...), and Multiplayer is an option.

The options for movement are by far more versatile like, running, climbing, gliding right from the start, and that is for free (if you are willing to spend much time to level up)!!!

There are many function to bring some insight into the various tasks, and currencies, which is quiet useful (otherwise new players would be completely lost).

Most challenges feel harder (again compared to wow) as they come with e.g. a time limit, and boss level mobs may use more attacks. The world level increases for every 5 adventurer ranks and that keeps thinks difficult.

And don't by fooled with the character tryout, you get one at a high level, with their talents leveled up, a formidable weapon..., once you get this char expect much work to get this far...

## Endgame

You can play the main quest-line (archon quests), to  clear out each misconception about the archons.

The areas offer multiple options to advance reputation like values.

You have the option to explore many characters, with individual skills, and even more options to build the optimal team with these skills.

You can wish for a five star characters, that mostly offer major improvement beyond the normal.

You can even hope to get some archon that exceeds the expectations, and their number is growing over time.

But at the moment there is no answer to the initially raised questions:
* will the siblings reunite?
* will the abyss be defeated?
* why was the previous world destroyed?
* and many more... 

To advance, you depend on wishing, which is more or less a gamble (especially the standard banner gives you no hint what you can expect). For me Wow feels more predictable (even if it requires some subscriptions to keep going...).

And the two relate for me in the following way, Genshin does very well to offer a alternative to the leveling part of Wow, but they offer also no definitive finale, e.g. at the end of Fontaine Archecon quest, you will be told go to Natlan, which yet does not exist, just as rumors on some channels on youtube ...

## Teambuilding

* at first i was thinking, use the standard chars (e.g. rosa,bennet,lisa,noel) to level 90, at 80 i realized this was not feasible each has it strength points, but need a lot of micro management e.g. element swapping to get going
* the next thought was, get some 5* chars and level 90, soon i realized that there is always some fineprint that make one char work, if in the right team, or not, e.g. charlot will heal 15% with 3 fontainians in her team...
* the next thought was, get the 5* chars, and equip what weapon is available, but then i realized they need also a matching weapon to be effective
* and the constellation involved are not just linear, so free to play is possible if you don't care to much about the meta

How naive to think this game will be doable in half a year with somewhat reasonable time&f2p.

If you want to stick to f2p don't watch videos where others brag about their chars... (they don't tell how much time&money they used)

## World levels

The option to lower the world-level looks nice at first. But be careful, surely the monsters are easier to kill, but the rewards especial the charcter experience, is lowered from highest to next next lower level, and that is not just a bit. So if you find yourself stuck when leveling characters, check the world level.

## Wishes

At first i did not believe, that where you wish, but after spending primogens for not much, i realized that e.g. wishing on the statue in mondstadt ensures you getting better (at least once ???) ... (i assume this is something cultural ...)

## Story

### Mondstadt

The nation of anemo (resambles the german speaking part of europe, with a winery even if in the taverns there are big beer mugs everywhere)

### Liyue

The nation of of geo (resembles china?)

### Inazuma

The nation of electro (which becomes actually clear if you see the winter illumination in big japanese cities)

### Sumeru

The nation of dendro (it's always about dreams, linked to the ancient sumerani, or india, and the desert part egypt?)

There is much fuss about epilepsy at every start of game, but what is about schizophrenia? who did say that? was it me? or him? or her? (if you don't get this wait til the sumeru archon epilog)

This seems a bit off: plants and humans fight over oxygen (Nahida character quest part II)? After all Teyvat is very different from this world...

### Fontaine

The nation of hydro (some steampunk version of france)

### Natlan

The nation of pyro (with dragons)
