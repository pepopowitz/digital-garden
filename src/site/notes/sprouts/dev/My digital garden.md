---
{"dg-publish":true,"permalink":"/sprouts/dev/my-digital-garden/","created":"2025-01-06T09:36:31.698-06:00","updated":"2025-01-06T13:03:52.888-06:00"}
---


## What is this digital garden?

- I don't write nearly as much as I used to.
- A significant reason for this is the bar for entry.
	- Writing long-form blog articles takes too dang long, and a lot of effort.
	- But I have lots of thoughts throughout the day that would be fine as short-form posts.
	- And I like the idea of building drafts of longer-form writing in public.
	- Plus, short-form in the form of social media posts feels dead to me at this point. No engagement, and I change main drivers every 6 months.
- Soooooo I've set up this digital garden, using Obsidian, and the [Digital Garden](https://dg-docs.ole.dev/) extension to publish it. 

## Things to do beyond the default setup
- [x] Get timestamps displaying
	- This was a setting that I overlooked.
- [x] Add a display for all content? Most of it is currently orphaned because I don't do a lot of cross-note linking
	- Even though [Anne-Laure says I shouldn't keep orphan notes](https://www.mentalnodes.com/do-not-keep-orphan-notes)
	- This was accomplished by enabling more settings that I'd overlooked.
- [x] Ensure there is a solid sitemap & RSS feed
	- I needed to configure the base URL in the extension settings in order for it to generate these.
		- And to configure the base URL, I needed an actual URL. So I promoted the site to a custom subdomain in netlify.
	- There was a bug with the RSS feed: [the generator was removing the self-close slash from link elements](https://github.com/oleeskild/obsidian-digital-garden/issues/493)
		- As discussed in that thread, [the solution was to use _five_ slashes](https://github.com/oleeskild/obsidian-digital-garden/issues/493#issuecomment-1825034758) to escape one slash. Obligatory [Onion reference](https://theonion.com/fuck-everything-were-doing-five-blades-1819584036/).
		- My commit: https://github.com/pepopowitz/digital-garden/commit/4397077e599e237586dce9d2e2dc08b64d7c7741
- [x] fix horrible contrast of header for the theme I'm using ([RetroNotes](https://github.com/sr-campelo/retronotes))
	- (and other elements that weren't initially enabled) 
	- (and move the header element to the actual site header rather than the page content heading)
	- Commit: https://github.com/pepopowitz/digital-garden/commit/f21fa399335494d505f0f81fc4837bea214f3030
	- I still don't know why but I struggled with specificity on the header h1. I had to give the element a class in order to target it without the base CSS being somehow more specific.
- [x] Prove in Obsidian that I can easily create content here & link to it from my daily notes
- [x] Fix contrast of search results
- [ ] Meta tags & opengraph data
	- see https://dg-docs.ole.dev/advanced/note-specific-settings/#metatags
- [ ] Social sharing image
- [ ] Add a header to link it back to my blog/site
- [ ] Add a link to it in my site's header
- [ ] favicon
- [ ] logo, branding
- [ ] Add a feed somewhere for most recent updates
- [ ] Use tags?
- [ ] Automate posting updates to socials? 
- [ ] Annoying bug: when writing content on one machine, and then pulling it to a second, it updates the created/edited timestamps. 
- [ ] Annoying bug (website): all headings are the same size 😭
