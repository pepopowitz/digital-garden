---
{"dg-publish":true,"permalink":"/sprouts/dev/aero-space/","tags":["macos","spaces","tools"],"created":"2025-01-16T07:46:19.255-06:00","updated":"2025-01-16T09:27:04.752-06:00"}
---

## Why am I looking at AeroSpace?

I've actually lost track of this along the way, so I'm capturing my reasons here:

1. I want to be able to name my spaces.
2. I want to be able to associate specific windows (important: not specific _apps_) to specific spaces, or be able to spin up a set of windows in a space easily.
3. I want more, better, and configurable shortcut keys for working with spaces.

## vibe check

I like it...I am one or two scriptable problems (I think) from loving it, and being able to use it as my daily driver everywhere. It checks all of my goals above—with varying levels of effort, but it checks them nonetheless.

**However**: I think it's unusable for me, because any time I go on a video call my computer basically freezes. Video call lag isn't an unusual experience for me even without aerospace running, but when it _is_ running, it's really really bad. Like unusable. This is even after a reboot.

#### conclusion
So, I'm moving on. I've looked at Amethyst and yabai as alternatives, and I think I'm going to give yabai a spin. Amethyst doesn't have enough programmability, and I don't think I can name my spaces. I don't think I can name my spaces with yabai either, but it has a ton of programmability, and I'm curious if that might be enough for me. I am concerned about having to turn off SIP, though that is all for a new sprout on yabai 😄.

## Initial intrigue
- this video made me HYPED to try it out: https://www.youtube.com/watch?v=xdHPmChLVl0&t=2m50s
		- Other neat tools included: 
			- https://github.com/FelixKratz/SketchyBar
				- nice organization of top bar, including organized display of available spaces
			- https://github.com/FelixKratz/JankyBorders
				- aside from coloring the active border, not as useful/necessary as sketchybar I don't think
				- [examples](https://github.com/FelixKratz/SketchyBar/discussions/47)
- I was concerned that I'd have to re-learn tiling. I am using Divvy for tiIing and I like it, I only really want the space management features of AeroSpace.
	- Though with auto-tiling and accordion features, I think I could adapt to this.
	- Future me: it was pretty tough to adapt. I started moving toward hotkeys to mark all windows in a space as floating, instead. 
- another video: https://www.youtube.com/watch?v=-FoWClVHG5g
	- shows a lot more detail about commands & events
	- also shows default aerospace functionality, without sketchybar/jankyborders on top of it.
	- it seems like aerospace assumes windows should be grouped by application, or at least this video demoes ways to connect them that way. I tend to group windows by theme, and they cross application, and I have multiple edge windows for example in many different groups. There's nothing that prevents me from doing this, but I am interested in automating the creation of workspaces, and can I do that with a different code/edge window in each automated workspace?
		- I don't think this exists yet: https://github.com/nikitabobko/AeroSpace/issues/609
## Questions & Findings
### ❓can I name workspaces something more semantic? 
- 💡 You can name workspaces whatever you want! Kind of.
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
	- I also can't assign anything except `1`:`1` to space `1`. 
	- But I could make `2:2(Comms)`, and I guess that's the standard I'm rolling with.
	- Parentheses in the name seem to be okay. Spaces are not.

### ❌👎 do workspaces retain across computer restarts? 
- **NO!!!** This is seriously horrible. With native macos spaces, it's not perfect, but many windows _are_ retained across spaces -- browsers are the notable offender, and I have to move them all after a restart. But having to re-associate _every single window_??? That sounds horrible.
	- Maybe I could script this? While browsers tend to be mis–associated when a restart occurs and I'm using native macos spaces, most other windows _are_ retained across restarts
- Associations aren't even retained across restarts of AeroSpace itself.
### ❓can I assign a specific space to be always floating?
- I don't think there's a way to do this from the creation of the space, but with a command you can easily toggle a window between tiling and floating modes, and you can also emit all windows in the current space. I have added a command under servicemode/alt-r that does the following:
	- `aerospace list-windows --workspace focused | awk -F '|' '{print $1}' | xargs -I {} aerospace layout --window-id {} tiling floating`
	- This lists all windows for the current workspace, and toggles them all between tiling and floating modes. 
### 😅 is it really that bad to use more than one native space?
- yes, weird things happen.
- I'm at a stage right now where I need to commit. The tool benefits from using only one native Macos Space, but I am afraid to close all my spaces because it takes so long to sort them 😭.
	- Update: I did it, I closed all native spaces, and restarted. It did not work out as well as I'd hoped, see notes above re: retaining workspaces across reboots. 

### 🤔 Re: restarts & recovering workspace association via scripting
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
### ❓ how can I view all apps running on the current space? Like mission control.
- Mission control doesn't work because it shows everything in the _native_ space, which is _everything_.
- I don't see a built-in command (which I'm not even sure where that would present me all the windows anyway). I started thinking about how I could transfer the results of `aerospace list-windows` into a UI, and realized raycast would be a good way to do that.....then realized [the aerospace raycast extension](https://www.raycast.com/limonkufu/aerospace) exists, and does exactly this!!! Looking forward to trying it out.
	- OH MY GOD I LOVE THIS. I linked the "windows in workspace" command to alt-tab, so now switching apps in either a tiled or floating layout is easy peasy. Plus, search is built into raycast, so I can filter down to the app I really want, quickly.
### misc thoughts
- 👎 I miss the trackpad swiping to a different space.
	- I could learn to be okay with this. alt-p and alt-n (for previous and next) are just as easy.
- 👎 is there a way to make it tile things in a more useful way? With a balanced tree that shows both horizontal and vertical sets, like splitting the screen into quadrants. I hate having to manually create this.
- 👍 Duh, I replaced the shortcuts for next/prev workspace with the ones I use for native spaces. This freed up the N & P workspaces.
- 🤷 I did it. I switched my pattern from alt-{workspace} and alt-shift-{workspace} to ctrl-alt-{workspace} and ctrl-alt-shift-{workspace}. I did this because I want to work toward using divvy for tiling but aerospace for workspaces, and I got confused and thought my divvy shortcuts all used alt. They don't -- they all use cmd -- but I didn't realize that until after I'd already switched. Oh well, let's see how this goes.
	- Future me, it went well! So well that I also switched a bunch of other commands, to make sure things like "moving a window" had the same modifiers, which they didn't out of the box.
	- I also moved the "join with direction" commands out of service mode, because I want them to be easier to get to.
- 👎 Innnnteresting. The cmd-backtick shortcut, which I use to switch browser windows in native spaces, crosses aerospace workspaces. In native spaces, it only visits browser windows on the current space. I don't think I like this. (Same thing with any other app, like VS Code.)
- 💪 I wrote this script to toggle all windows on the current space between tiling and floating. I will link this to a command in service mode, so that I can completely "reset" a space on demand.
	- `aerospace list-windows --workspace focused | awk -F '|' '{print $1}' | xargs -I {} aerospace layout --window-id {} tiling floating`
- ❗One thing that's been bothering me is that I can't view windows in the current space. Mission control doesn't work well with it, and I don't see a built-in command (which I'm not even sure where that would present me all the windows anyway). I started thinking about how I could transfer the results of `aerospace list-windows` into a UI, and realized raycast would be a good way to do that.....then realized [the aerospace raycast extension](https://www.raycast.com/limonkufu/aerospace) exists, and does exactly this!!! Looking forward to trying it out.
	- OH MY GOD I LOVE THIS. I linked the "windows in workspace" command to alt-tab, so now switching apps in either a tiled or floating layout is easy peasy. Plus, search is built into raycast, so I can filter down to the app I really want, quickly.
- 🤔 My biggest remaining concern is, I want to share my aerospace config in my dotfiles....but I want the space names to differ across machines. 
	- Can I have it import a subset of the config from a separate file, which isn't included in dotfiles?
	- I also have the concern from above re: restoring workspaces after reboot, but that's more future looking and less urgent.
		- Actually this proved to be a major pain in the ass, as I had to restart aerospace occasionally due to cpu slamming that may or may not have been caused by AeroSpace. Sorting windows into spaces sucks. 

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
	- and actually, this is probably simpler for my uses by using the `focused` workspace, instead of a specific name: 
		- `aerospace list-windows --workspace focused`
- join a specific window with something near it
	- `aerospace join-with --window-id 7319 right`
	- can act on a specific window by id...
	- but you can't tell it a specific window to join _to_, other than by direction.
	- this would make it hard to combine two specific windows (e.g. I want to join both browser windows in a subtree), because I don't know how to tell where a window is relative to another in direction.
- toggle all windows on the current space between floating & tiling!!
	- `aerospace list-windows --workspace focused | awk -F '|' '{print $1}' | xargs -I {} aerospace layout --window-id {} tiling floating`
- get computer name (as defined in settings)
	```
	❯ scutil --get ComputerName
	artsylappy
	```
	- this could help me toggle my aerospace config based on device. 