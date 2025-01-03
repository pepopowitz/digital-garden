---
{"dg-publish":true,"permalink":"/sprouts/dev/my-digital-garden/","created":"2025-01-03T11:57:02.204-06:00","updated":"2025-01-03T16:28:08.475-06:00"}
---


## What is this digital garden?

- I don't write nearly as much as I used to.
- A significant reason for this is the bar for entry.
	- Writing long-form blog articles takes too dang long, and a lot of effort.
	- But I have lots of thoughts throughout the day that would be fine as short-form posts.
	- And I like the idea of building drafts of longer-form writing in public.
	- Plus, short-form in the form of social media posts feels dead to me at this point. No engagement, and I change main drivers every 6 months.
- Soooooo I've set up this digital garden, using Obsidian, and the Digital Garden extension to publish it. 

## Things I did after the initial setup
- [x] Get timestamps displaying
	- [ ] This was a setting that I overlooked.
- Ensure there is a solid sitemap & RSS feed
	- I needed to configure the base URL in the extension settings in order for it to generate these.
	- But then there was a bug with the RSS feed: [the generator was removing the self-close slash from link elements](https://github.com/oleeskild/obsidian-digital-garden/issues/493)
		- As discussed in that thread, [the solution was to use _five_ slashes](https://github.com/oleeskild/obsidian-digital-garden/issues/493#issuecomment-1825034758) to escape one slash. Obligatory [Onion reference](https://theonion.com/fuck-everything-were-doing-five-blades-1819584036/).
		- My commit: https://github.com/pepopowitz/digital-garden/commit/4397077e599e237586dce9d2e2dc08b64d7c7741
- fix horrible contrast of header 
	- (and move the header element theres)
- Add a header to link it back to my blog/site
- Add a link to it in my site's header
- Prove in Obsidian that I can easily create content here & link to it from my daily notes
- Add a feed somewhere for most recent updates
- Add a display for all content? Most of it is currently orphaned because I don't do a lot of cross-note linking
	- Even though [Anne-Laure says I shouldn't keep orphan notes](https://www.mentalnodes.com/do-not-keep-orphan-notes)
- Automate posting updates to socials? 
