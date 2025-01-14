---
{"dg-publish":true,"permalink":"/sprouts/dev/aero-space/","tags":["macos","spaces","tools"],"created":"2025-01-10T20:21:38.506-06:00","updated":"2025-01-13T22:34:24.473-06:00"}
---

## Intriguing
- this video made me HYPED to try it out: https://www.youtube.com/watch?v=xdHPmChLVl0&t=2m50s
		- Other neat tools included: 
			- https://github.com/FelixKratz/SketchyBar
				- nice organization of top bar, including organized display of available spaces
			- https://github.com/FelixKratz/JankyBorders
				- aside from coloring the active border, not as useful/necessary as sketchybar I don't think
				- [examples](https://github.com/FelixKratz/SketchyBar/discussions/47)
- concerned that I'll have to re-learn tiling. I am using Divvy for tiIing and I like it, I only really want the space management features of AeroSpace.
	- Though with auto-tiling and accordion features, I think I could adapt to this.
- another video: https://www.youtube.com/watch?v=-FoWClVHG5g
	- shows a lot more detail about commands & events
	- also shows default aerospace functionality, without sketchybar/jankyborders on top of it.
	- it seems like aerospace assumes windows should be grouped by application, or at least this video demoes ways to connect them that way. I tend to group windows by theme, and they cross application, and I have multiple edge windows for example in many different groups. There's nothing that prevents me from doing this, but I am interested in automating the creation of workspaces, and can I do that with a different code/edge window in each automated workspace?
		- I don't think this exists yet: https://github.com/nikitabobko/AeroSpace/issues/609
## Questions & Findings
- ❓can I name workspaces something more semantic? A: Yes, see below!
- ❌👎 do workspaces retain across restarts? 
	- **NO!!!** This is seriously horrible. With native macos spaces, it's not perfect, but many windows _are_ retained across spaces -- browsers are the notable offender, and I have to move them all after a restart. But having to re-associate _every single window_??? That sounds horrible.
	- Maybe I could script this? While browsers tend to be mis–associated when a restart occurs and I'm using native macos spaces, most other windows _are_ retained across restarts
- ❓can I assign a specific space to be always floating?
	- Probably, through scripting.
- 💡 You can name workspaces whatever you want!
	- Out of the box, it names them 1-9 and A-Z.
	- You can *rename* them to something more semantic, if you want. 
	- Example: I could rename workspace C to workspace Camunda. 
		```
		alt-a = 'workspace Alpha'
		alt-shift-a = 'move-node-to-workspace Alpha'
		```
		- Why do this? Because the name Camunda then shows in the top bar, and I don't have to associate letters/numbers to a specific meaning.
		- But I still need to use a single-character shortcut (with modifiers) to reference that workspace in hotkeys. That means I may have to associate a letter/number to each workspace anyways.
	- Ooookay, it's not as flexible as I thought. It looks like the name needs to use the same first letter as the shortcut, surprisingly. `a`:`Alpha` worked fine; `1:Comms` did not -- it assigned the `Comms` space to the position that `c` was in. 
	- I also can't assign anything except `1`/`1` to space `1`. 
	- But I could make `2:2(Comms)`, and I guess that's the standard I'm rolling with.
- 😅 I'm at a stage right now where I need to commit. The tool benefits from using only one native Macos Space, but I am afraid to close all my spaces because it takes so long to sort them 😭.
	- Update: I did it, I closed all native spaces, and restarted. It did not work out as well as I'd hoped, see notes above re: retaining workspaces across reboots. 
- 🤔 Re: restarts & recovering workspace association via scripting
	- I think this might be doable. Listing all windows shows that there are many that I can infer enough information from the description to know what to do with them.
	- run `aerospace list-windows --workspace 1` to see open windows on space 1
	- `Code` includes the name of the project. The space could be deduced from those.
		- `126 | Code           | goto — TODO.md`
	- `Figma`, `Mail`, `Obsidian`, `Slack`, `Spotify`, `Toggl Track` are all one–window apps that could be sorted properly.
	- `Preview`, `Hyper`, `Finder` could be put in a space that needs to be sorted manually.
	- `Microsoft Edge` includes the profile name, and those could be roughly sorted. The page title could also be used.
		- `205 | Microsoft Edge | AeroSpace Commands - Microsoft Edge - Brilliant Supernova`
		- `193 | Microsoft Edge | Aerospace Is The Best... - YouTube - Microsoft Edge - steven.j.hicks`
		- `201 | Microsoft Edge | Form | Raycast API - Microsoft Edge - 2 rad dads`
	- I could also write a script to capture all open windows and the spaces they are associated with, and write it to a file, so that on restart another script can do its best interpretation of where windows belong.
- 👎 I wish there was a way to easily view all apps running on the current space! Like mission control.
	- Mission control doesn't work because it shows everything in the _native_ space, which is _everything_.
- 👎 UUUGGGHHHH I forgot about my divvy shortcuts -- alt-d, alt-f (alt-m, alt-1, alt-l, ....)
	- I think this is the reason I need to switch aerospace to use a different modifier -- ctrl-alt instead of alt, perhaps.
- 👎 I miss the trackpad swiping to a different space.
	- I could learn to be okay with this. alt-p and alt-n (for previous and next) are just as easy.
- 👎 is there a way to make it tile things in a more useful way? With a balanced tree that shows both horizontal and vertical sets, like splitting the screen into quadrants. I hate having to manually create this.
- 👍 Duh, I replaced the shortcuts for next/prev workspace with the ones I use for native spaces. This freed up the N & P workspaces.
- 🤷 I did it. I switched my pattern from alt-{workspace} and alt-shift-{workspace} to ctrl-alt-{workspace} and ctrl-alt-shift-{workspace}. I did this because I want to work toward using divvy for tiling but aerospace for workspaces, and I got confused and thought my divvy shortcuts all used alt. They don't -- they all use cmd -- but I didn't realize that until after I'd already switched. Oh well, let's see how this goes.
- 👎 Innnnteresting. The cmd-backtick shortcut, which I use to switch browser windows in native spaces, crosses aerospace workspaces. In native spaces, it only visits browser windows on the current space. I don't think I like this. (Same thing with any other app, like VS Code.)
- 💪 I wrote this script to toggle all windows on the current space between tiling and floating. I will link this to a command in service mode, so that I can completely "reset" a space on demand.
	- `aerospace list-windows --workspace focused | awk -F '|' '{print $1}' | xargs -I {} aerospace layout --window-id {} tiling floating`

### vibe check

I like it...but I'm starting to feel enough pain points that I'm not sure I _love_ it. 

## helpful commands
- get current workspace name:
	- `aerospace list-workspaces --monitor focused --visible`
- get windows for specific workspace
   ```
   ❯ aerospace list-windows --workspace 1
   7361 | Figma          | Mainnav
   170  | Hyper          | Hyper
   7319 | Microsoft Edge | AeroSpace Commands - Microsoft Edge - Brilliant Supernova
   161  | Obsidian       | AeroSpace - foamington - Obsidian v1.7.7
   ```
- join a specific window with something near it
	- `aerospace join-with --window-id 7319 right`
	- can act on a specific window by id...
	- but you can't tell it a specific window to join _to_, other than by direction.
	- this would make it hard to combine two specific windows (e.g. I want to join both browser windows in a subtree), because I don't know how to tell where a window is relative to another in direction.
- toggle all windows on the current space between floating & tiling!!
	- `aerospace list-windows --workspace focused | awk -F '|' '{print $1}' | xargs -I {} aerospace layout --window-id {} tiling floating`