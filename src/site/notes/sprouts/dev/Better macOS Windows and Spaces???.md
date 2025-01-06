---
{"dg-publish":true,"permalink":"/sprouts/dev/better-mac-os-windows-and-spaces/","tags":["ramblings","macos","spaces"],"created":"2025-01-06T11:23:51.041-06:00","updated":"2025-01-06T12:45:38.563-06:00"}
---

# Journey to better macOS windows and spaces.
The macOS Spaces implementation and I go way back. I have had a love/hate relationship with Spaces since owning my first mac. On the one hand, Spaces presents a seductive interface that aligns with my mental model of how I want to organize my desktop. On the other hand, it takes about 3 minutes of usage to discover that it was implemented as an MVP, and the product team was obviously disbanded after the first release and no improvements have been made.

I keep anywhere from 3 to 7 spaces active on my laptop at all times. Each of them holds windows associated with a specific project or task (except right after I restart my computer, because all browsers end up on the first space after a reboot until I manually move them back 😠). I have no way to remember which space contains which project/task except by looking, because I can't name the spaces 😠...but I can usually tell pretty quickly by looking at the open windows in Mission Control.

## Moving a window to another space

Something I do tens of times per day is move a window from one space to another. Sometimes I need a window on a different space, or I shift trains of thought and open a new window for the current app before I move to the corresponding space. There are probably five other reasons I'm not thinking of, it just happens.

For a long time, I was using Raycast for this. According to my notes from 2024-08-19, this broke when I updated to macOS Sonoma (14.6). The issue is [tracked by Raycast](https://github.com/raycast/extensions/issues/12493#issuecomment-2129003282).

This inspired me to look for other ways to move windows to spaces. 
- [Hammerspoon](https://github.com/Hammerspoon/hammerspoon) looked promising....but had [the same issue](https://github.com/Hammerspoon/hammerspoon/issues/3666).
- [Amethyst](https://github.com/ianyh/Amethyst) didn't work for me at all.
- [Yabai](https://github.com/koekeishiya/yabai) also looked promising, but it sounded like I would need to change some configuration (disabling System Integrity Protection (SIP)) and I wasn't comfortable doing that on my work laptop. 
- [BetterTouchTool](https://folivora.ai/downloads) worked! But after a 45 day trial it costs $12 and given all these other things kept breaking I didn't feel confident that it wouldn't eventually also suffer the same problem, so I didn't want to pay for it.

Eventually, I tried Yabai just for funsies...and it worked _without_ disabling SIP! 

So I added a couple scripts to my dotfiles ([move-next](https://github.com/pepopowitz/dotfiles/blob/96ac44eb7718a8d908c49dd050dff9e53d61f33b/move-next.sh) and [move-previous](https://github.com/pepopowitz/dotfiles/blob/96ac44eb7718a8d908c49dd050dff9e53d61f33b/move-previous.sh)), and pointed Raycast at them. Here's `move-next`: `yabai -m window --space next --focus`.

This worked for months!

### MacOS Sequoia (15) broke everything

IT yelled at me for not updating my operating system to Sequoia, so I did that one day.....and Yabai stopped working for moving windows to spaces. It's tracked at https://github.com/koekeishiya/yabai/issues/2500, and multiple other issues.

When it happened I tried a few other approaches again, though I didn't keep notes. I couldn't find a fix, and I moved windows to different spaces **like a cave person** for months. 

### Pavlos always has answers

In a chat with my friend [Pavlos](https://bsky.app/profile/pav.vin), he was demoing his micro-blogging toolset ([including lmno.lol](https://lmno.lol/pvinis)), and he just casually mentioned Yabai. So we ended up talking about that for a bit, and it reminded me of my current cave person window movement conditions and I complained about it to him.

He mentioned [aerospace](https://nikitabobko.github.io/AeroSpace/guide), which I'll come back to.

But he also prompted me to remember what my issue was, and find out if Yabai had fixed it.

- Yabai had not fixed it.
- [But there was an update!](https://github.com/koekeishiya/yabai/issues/2484#issuecomment-2523222213).
- Unfortunately it was not an update I was going to use—"you can't do this without disabling SIP." As previously mentioned, I don't want to disable SIP on my work laptop, if I even can, because I don't like being put on lists.

Then I looked back for my notes—most of which are above—to remind myself what I was using before Yabai. Basically, because Pavlos asked if I had tried Raycast's built in window management for this, and I was _sure_ I had notes about that not working. I found these notes, including the link to [the tracked issue](https://github.com/raycast/extensions/issues/12493#issuecomment-2129003282), and....

## Raycast fixed theirs!!!

> I had this issue after upgrading to MacOS Sequoia 15.1 but it was resolved when I updated to Raycast 1.88.4

[_jaimemarijke_](https://github.com/raycast/extensions/issues/12493#issuecomment-2551992403)

Me too!!!

So now I'm switching back to Raycast for this feature. (Though OMG is the animation slowwwwwww, I'm sure that's the OS not Raycast, but uuuuugggghhhhhh.)

## Is Aerospace the better implementation of spaces that I've been looking for???!!!!

Ok so I can move windows to spaces again....but I got just far enough into [the Aeroplane docs](https://nikitabobko.github.io/AeroSpace/guide#emulation-of-virtual-workspaces) to be very very very intrigued. 

They had me at "Native macOS Spaces have a lot of problems": 

> Native macOS Spaces have a lot of problems
> 
> - The animation for Spaces switching is slow
>     
>     - You can’t disable animation for Spaces switching (you can only make it slightly faster by turning on `Reduce motion` setting, but it’s suboptimal)
>     
> - You have a limit of Spaces (up to 16 Spaces with one monitor)
>     
> - You can’t create/delete/reorder Space and move windows between Spaces with hotkeys (you can only switch between Spaces with hotkeys)
>     
> - Apple doesn’t provide public API to communicate with Spaces (create/delete/reorder/switch Space and move windows between Spaces)
> 
> Since Spaces are so hard to deal with, AeroSpace reimplements Spaces and calls them "Workspaces".

I have a few things I'd add to that list (can't name a space, rebooting resets spaces for some apps but not others, ...) but **omg Aerospace you are speaking my language**. I have a dream of being able to spin up spaces willy nilly as I start tasks, and I have yet to find anything that remotely made this possible.

Is this the thing I need in my life? I'll update with my findings.
