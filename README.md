# Star Wars Racer Auto Splitter (for LibreSplit)
A script that automates LibreSplit's timer, for Star Wars Episode I Racer speedruns.  

### Features
* Auto Start when file is opened, or (experimentally) when selecting "Start Race" for New Game +
* Split at race finsh
  * Option to require 1st place for 100%
* Reset on return to file selection
  * Option to disabled reset
* Run catagory presets (optional)
* Choose between IGT, LRT, RTA for timing methods 
* Ability to remove unfocused / tabbed out time when using LRT
* Option to view extra information in terminal (when LibreSplit is run through terminal)

### Requires
* Installation of the re-released PC version of Star Wars Episode I Racer (Steam, GOG, etc.).  
    - Does not work with the original CD version.
* [LibreSplit](https://github.com/LibreSplit/LibreSplit/tree/main)

## Quick Setup (TLDR)
* Download the script (swe1r-auto_splitter.lua).
* Open the script with a text editor
* under "AUTO SPLITTER SETTINGS" edit settings 
* Load and Enable the script in LibreSplit

## Configuring Settings

### Getting Started
After downloading the auto splitter, open it with a text editor.  
Beneath the script information at the top, there will be some settings notes, and under that will be the  
"AUTO SPLITTER SETTING". 

### Auto Splitter Settings
___
> [!caution]
> ```lua
> setting = "value",
> ```
> **When adjusting settings it is important that you:**
> * Only edit the **```value```**.
> * Only replace a **```value```** with another of its type. 
> * Make sure that any **```value```** is ended with a comma **```,```**.
>   
> Changing or removing any other syntax (like **```setting```** names, missing any commas **```,```** etc.) will break the script.
___
```lua
local sets = {
--____________________________________________________________________________
--------------------------- AUTO SPLITTER SETTINGS ---------------------------
--____________________________________________________________________________
-- Choose Run Catagory-->| None | Any%-Am/Semi Circuit | 100% |  NewGame+ |
   preset = 1, --     -->| [0]  |         [1]          | [2]  |    [3]    |
--_______________________|______|______________________|______|___________|___
----------------------------------------------------------|  FORMAT = SETS
-- Timing Method -   In race GT - RT No Loads - Real Time | [1,2,3] = [1](LRT)
   timeMethod = 1, -- [0](IGT)  -   [1](LRT)  - [2+](RTA) |      
----------------------------------------------------------|-------------------
-- Requires 1st place, or if [false] requires 4th place,  |     [2] = [true]
   req1st = false, --  and 3rd on SMR/BB/BEC.             |   [1,3] = [false] 
----------------------------------------------------------|-------------------
-- (semi-functional) Timer triggers at "Start Race",      |     [3] = [true]
   trigSR = false, -- instead of file select.             |   [1,2] = [false] 
-- Must move directly from "Track Select" > "Start Race"  |
----------------------------------------------------------|-------------------
-- Enable reset trigger - triggers at file selection.     |      
   reset = true, -- [false] for mid-run file switch.      |     [3] = [false]
----------------------------------------------------------\___________________
-- Real-Time No Loads removes unfocused time (tabbed out)
   noTab = false, -- Affects all presets [timeMethod = 1]
------------------------------------------------------------------------------
-- View Extra Info In Terminal (when LibreSplit is run through the terminal).
   termInfo = { view = false, -- Toggles the view of the following extra info.
--  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --  --
                 totalRIGT = true, -- Total race IGT
                   curRIGT = false, -- Current race IGT
                 overHeats = true, -- Counts overheats over whole run
                    deaths = true, -- Counts deaths over whole run
   }--________________________________________________________________________
------------------------------------------------------------------------------
}
```
In the script, each setting is detailed to a minimal degree. This should be enough to work with, but below each setting is described in more detail.
___
### Choose Run Catagory 
```lua
preset = 1,
```
|Catagory| None | Any%-Am/Semi Circuit | 100% | New Game + |
|:---:|:---:|:---:|:---:|:---:|
|**Value**| 0 | 1 | 2 | 3 |  
 
**```preset```** is the most important setting. It functions as a parent setting for of a number of other settings. Setting **```preset```** to **```1```,  ```2```, or ```3```** will override these settings. For this reason **```preset``` ```0```** exists, allowing for full settings control for special use cases (advanced users).

 > [!note]
> The **```preset```** variable is the only setting that requires adjustment, in order to properly run all catagories.

___
**Timing Method**
```lua
timeMethod = 1,
```
|Method| In race Game Time (IGT) | Real Time No loads (LRT) | Real Time (RTA) |
|:---:|:---:|:---:|:---:|
|**Value**| 0 | 1 | 2+ |
| **```preset```** |  | **```1``` ```2``` ```3```** |  |
 
Here you are given the option to choose your prefered Timing Method. As shown all 3 catagory presets override to **```timeMethod = 1,``` (LRT)**. The other 2 aren't useful for recording official runs, but maybe useful for other specific use cases.

___
**Requires 1st Place**
```lua
req1st = false,
```
| Win Condition | Require 1st | Require 4th, 3rd (on SMR/BB/BEC) |
|:---:|:---:|:---:|
|**Value**| true | false |
| **```preset```** | **```2```** | **```1``` ```3```** |

**```req1st```** toggles the requirement of finishing in 1st place to trigger a split. If **```false```**, the normal win conditions of 4th, or 3rd on the last track of a circuit ( SMR / BB / BEC ) will be used. **```req1st```** is usually set **```true```** for 100% runs, not of much use otherwise.
 
___
**Timer Triggers At "Start Race" (semi-functional)**
```lua
trigSR = false,
```
| Trigger on | "Start Race" | File open |
|:---:|:---:|:---:|
|**Value**| true | false |
| **```preset```** | **```3```** | **```1``` ```2```** |

**```trigSR```** if **```true```** will trigger the timer to start when "Start Race" is selected. When **```false```** the timer will return to it default function of starting at file open. **```trigSR```** exists for the New Game + requirement of; Timing starts when selecting "Start Race" for the first track. No use otherwise.
 > [!important]
> Currently the "Start Race" trigger is incomplete. In order for the timer to trigger you must enter the track selection scene and move directly to select "Start Race". If you deviate, just return to the track selection scene, before heading to "Start Race". 

___
**Enable Reset Trigger**
```lua
reset = true,
```
| Reset Trigger | On | Off |
|:---:|:---:|:---:|
|**Value**| true | false |
| **```preset```** |  | **```3```** |
 
**```reset```** is a simple toggle for turning the reset trigger on/off. It is disabled under the New Game + **```preset```**. This allows changing modes or files mid run, without triggering a reset. Aside from that, this setting is really just personal preference.

___
**REMOVE UNFOCUSED TIME (tabbed out)**
```lua
noTab = false,
```
| Unfocused Time | Removed | Counted |
|:---:|:---:|:---:|
|**Value**| true | false |
 
When **```true```**, **```noTab```** will count unfocused / tabbed out time as loading time. This will only take effect if **```timeMethod = 1,``` (LRT)** is set (this is most cases). If **```false```**, unfocused / tabbed out time will be counted on the timer. **```noTab```** is purely personal choice.

___
**View Extra Info In Terminal**
```lua
termInfo = { view = false,
```
| View Terminal Info | Yes | No |
|:---:|:---:|:---:|
|**Value**| true | false |

> [!warning]
> **```termInfo = { }```** is a lua table. It's purpose is to contain the settings that toggle the viewability of each individual piece of extra info. It should not be edited, and any **```{``` ```}```** should not be touched. Only edit the values of **```view```** and values of the settings under **```view```**.

**```view```** simply sets your enabled statistics to be viewed in the terminal. This will have no effect unless you are running LibreSplit through the terminal. This is unideal, but is currently the best option, as a method of viewing the statistics that is supported for viewing through "LiveSplit's variable viewer" (there is no LibreSplit alternative). Keep **```false```** if you're not running LibreSplit through the terminal.

**Individual Statistics for Viewing in Terminal**
| Statistic | Description |
|---|---|
| totalRIGT |  |
| curRIGT |  |
| overHeats |  |
| deaths |  |

Each statistic should be treated like **```view```**. Set **```true```** to enable and **```false```** to hide.

WIP.....
