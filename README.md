# STAR WARS RACER AUTOSPLITTER (for LibreSplit)
**A script that automates LibreSplit's timer, for Star Wars Episode I Racer speedruns.**  

### FEATURES
* Auto Start when file is opened, or (experimentally) when selecting "Start Race" for New Game + catagoy
* Split at race finish
  * option to require 1st place to split, for 100% catagory
* Reset on return to file selection
  * can toggle on / off
* Run catagory presets (optional)
* Choose between IGT, LRT and RTA timing methods 
* Ability to remove unfocused / tabbed out time when using LRT
* Option to view extra information in terminal

### REQUIRES
* [LibreSplit](https://github.com/LibreSplit/LibreSplit/tree/main)
* Installation of the re-released PC version of Star Wars Episode I Racer (Steam, GOG, etc.)  
    - does not work with the original CD version

## QUICK SETUP (TLDR)
* Download and open the script in a text editor
* Edit the settings, under "AUTOSPLITTER SETTINGS"
* Load and Enable the script in LibreSplit

___
## SETUP
After downloading the script, open it with a text editor.  
At the top there are script notes, followed by a small settings guide, and under that will be the  
"AUTOSPLITTER SETTING". 
### AUTOSPLITTER SETTINGS
```lua
local sets = {
--____________________________________________________________________________
--------------------------- AUTOSPLITTER SETTINGS ----------------------------
--____________________________________________________________________________
-- CHOOSE RUN CATAGORY --> None | Any%-Am/Semi Circuit | 100% |  NewGame+ |
   preset = 1,         --> [0]  |         [1]          | [2]  |    [3]    |
--______________________________|______________________|______|___________|___
----------------------------------------------------------|  PRESET = SETS
-- TIMING METHOD  -->In race GT | RT No Loads | Real Time | [1,2,3] = [1](LRT)
   timeMethod = 1,--> [0](IGT)  |   [1](LRT)  | [2+](RTA) |      
----------------------------------------------------------|-------------------
-- REQUIRES 1ST PLACE, or if [false] requires 4th place,  |     [2] = [true]
   req1st = false, --  and 3rd on SMRSMR/BB/BEC           |   [1,3] = [false] 
----------------------------------------------------------|-------------------
-- "START RACE" TIMER TRIGGER (SEMIFUNCTIONAL) - Move     |
   trigSR = false, -- from "Track Select" > "START RACE"  |   [1,2] = [false] 
----------------------------------------------------------|-------------------
-- ENABLE RESET TRIGGER - triggers at file selection.     |      
   reset = true, -- [false] for mid-run file switch.      |     [3] = [false]
----------------------------------------------------------\___________________
-- REMOVE UNFOCUSED TIME (TABBED OUT) requires RT No Loads 
   noTab = false, -- Affects all presets [timeMethod = 1]
------------------------------------------------------------------------------
-- VIEW EXTRA INFO IN TERMINAL (when LibreSplit is run through the terminal).
   viewTermStats = false, -- Toggles the view of the following extra info.
--  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --
                viewIGT = true, -- Total race IGT
         viewCurRaceIGT = false, -- Current race IGT
          viewOverheats = true, -- Counts overheats over whole run
             viewDeaths = true, -- Counts viewDeaths over whole run
--____________________________________________________________________________
------------------------------------------------------------------------------
}
```
Here each setting is detailed to a minimal degree. This should be enough to work with, however each setting is described in more detail below.
> [!caution]
> ```lua
> setting = "value",
> ```
> **When adjusting a **```setting```** it is important that you:**
> * Only edit the **```value```**.
> * Only replace a **```value```** with another of its type. 
> * Make sure that any **```value```** is ended with a comma **```,```**.
>   
> Changing or removing any other syntax (like **```setting```** names, missing any commas **```,```** etc.) will break the script.
___
### CHOOSE RUN CATAGORY 
```lua
preset = 1,
```
**```preset```** is used as simple one setting adjustment for switching run catagories. It functions as a override for a number of other settings. For this reason **```preset``` ```0```** exists, allowing full settings control for special use cases.
|  | None | Any% - Amateur / Semi-Pro Circuit | 100% | New Game + |
|:---:|:---:|:---:|:---:|:---:|
| **preset =** | 0 | 1 | 2 | 3 |  
 > [!note]
> The **```preset```** variable is the only setting that requires adjustment, in order to properly run all catagories.

___
**TIMING METHOD**
```lua
timeMethod = 1,
```
Use **```timeMethod```** to choose either IGT, LRT or RTA. As shown below all 3 catagory presets, set LRT (**```timeMethod = 1,```**). The other 2 timing methods are not useful for recording official runs, but are there if you find a use for them.
|  | In race Game Time (IGT) | Real Time No loads (LRT) | Real Time (RTA) |
|:---:|:---:|:---:|:---:|
|**timeMethod =**| 0 | 1 | 2+ |
| **```preset```** |  | **```1``` ```2``` ```3```** |  |
 
___
**REQUIRE 1ST PLACE**
```lua
req1st = false,
```
**```req1st```** toggles the requirement of finishing in 1st place to trigger a split. If **```false```**, the normal win conditions of 4th, or 3rd on the last track of a circuit ( SMR / BB / BEC ) will be used. **```req1st```** is usually set **```true```** for 100% runs, not of much use otherwise.
|  | Require 1st | Require 4th, 3rd (on SMR/BB/BEC) |
|:---:|:---:|:---:|
|**req1st =**| true | false |
| **```preset```** | **```2```** | **```1``` ```3```** |

___
**"START RACE" TIMER TRIGGER (SEMI-FUNCTIONAL)**
```lua
trigSR = false,
```
**```trigSR```** if **```true```** will trigger the timer to start when "Start Race" is selected. When **```false```** the timer will return to it default function of starting at file open. **```trigSR```** exists for the New Game + requirement of; Timing starts when selecting "Start Race" for the first track. No use otherwise.
|  | "Start Race" trigger | File open trigger |
|:---:|:---:|:---:|
|**trigSR =**| true | false |
| **```preset```** | **```3```** | **```1``` ```2```** |
 > [!important]
> Currently the "Start Race" trigger is incomplete. In order for the timer to trigger you must enter the track selection scene and move directly to select "Start Race". If you deviate, just return to the track selection scene, before heading to "Start Race". 

___
**ENABLE RESET TRIGGER**
```lua
reset = true,
```
**```reset```** is a simple toggle for turning the reset trigger on/off. It is disabled under the New Game + **```preset```**. This allows changing modes or files mid run, without triggering a reset. Aside from that, this setting is really just personal preference.
|  | Enabled | Disabled |
|:---:|:---:|:---:|
|**reset =**| true | false |
| **```preset```** |  | **```3```** |
 
___
**REMOVE UNFOCUSED TIME (TABBED OUT)**
```lua
noTab = false,
```
When **```true```**, **```noTab```** will count unfocused / tabbed out time as loading time. This will only take effect if **```timeMethod = 1,``` (LRT)** is set (most cases). If **```false```**, unfocused / tabbed out time will be counted on the timer. **```noTab```** is purely personal choice.
|  | Removed | Counted |
|:---:|:---:|:---:|
|**noTab =**| true | false |

___
**VIEW EXTRA STATS IN TERMINAL**
```lua
viewTermStats = false,
```
**```viewTermStats```** toggles your enabeled stats to be viewable in the terminal. This will have no effect unless you are running LibreSplit through the terminal, so keep **```false```** if not. This is unideal, but is currently the best option. "LiveSplit's variable viewer" allows these stats to be viewed in LiveSplit (there is no LibreSplit alternative).   
  
Each stat gets set just like **```viewTermStats```**.  
|  | Enabled | Disabled |
|:---:|:---:|:---:|
|**viewTermStats =**| true | false |
|  |  |  |
|**viewIGT =**| true | false |
|**viewCurRaceIGT =**| true | etc... |
___
```lua
viewIGT = false,
```
**```viewIGT```** displays you true IGT (totaled in game race times). This is very usefull if you are using LRT or RTA as your timing method and also want to track IGT. It is identical to the IGT timing method.
___
```lua
viewCurRaceIGT = false,
```

___
```lua
viewOverheats = true,
```

___
```lua
viewDeaths = true,
```

___
