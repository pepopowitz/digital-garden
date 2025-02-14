---
{"dg-publish":true,"permalink":"/sprouts/dev/getting-mise-to-work-with-vs-code-s-git-interface/","created":"2025-02-05T16:08:37.298-06:00","updated":"2025-02-05T16:08:37.298-06:00"}
---

When setting up my new work laptop, I decided to give `mise` a spin as a version manager. I've used `asdf` in the past and found it very difficult to remember commands. `mise` sounded like a more ergonomically friendly tool, and a new laptop is the ideal time to try a new tool.

Most things so far have been very smooth. One was more of a struggle: getting a `husky` `pre-commit` hook working when committing through VS Code's Git interface. 

### Solution 

Note that this is what worked for my project, and it might not for yours. 

1. Add a `~/.huskyrc` file. At least in my project, which was configured with `husky` v8, this is the global configuration file loaded by `husky` before every hook. 
	- [`husky`'s docs](https://typicode.github.io/husky/how-to.html#startup-files) say it should be a `~/.config/husky/init.sh` file, and that `~/.huskyrc` is deprecated. But I found that the former never actually executed. When I browsed the project's `.husky` folder, the script that runs at the beginning of my hook, which is not committed to git, was looking for `~/.huskyrc`.
	- I'm assuming this means that the `~.huskyrc` option was deprecated in v9, but I didn't take the time to confirm it.
	- I'm going to make the same change in both places, in case I run into a project using `husky` v9 that sets it up according to the v9 docs.
2. Activate `mise` in that file. This was not as straightforward as I expected it to be.
	- [The `mise` docs]() do have a very early message telling you that you're going to have to deal with this problem, and that you should prefer to deal with it using shims: 
		"For interactive shells, `mise activate` is recommended. In non-interactive sessions, like CI/CD, IDEs, and scripts, using `shims` might work best."
	* But the shims docs did not make it very clear how I could activate them in my script. Everything referenced calling `mise` directly, but in a non-interactive shell, `mise` is not activated.
	* You can find where `mise` is running from, using `where mise`. I got: `/opt/homebrew/bin/mise` (and an actual function definition).
	* Reference that directly to activate, like this:
		```
		eval "$(/opt/homebrew/bin/mise activate --shims)"
		```
 	
### Why this was a problem

VS Code's git window is executing in a shell session that doesn't load your shell profile, which is where you activate `mise`. In my case, the hook was calling `npx lint-staged`; `npx` was not a recognized command, because the shell had not run `mise` to hook up Node. 

I'm not sure how I was getting around this with `asdf` previously, but I was. And I know that some users running `nvm` have this problem.

### How **not** to fix this

In the past, I've merged PRs that included an `nvm` activation script in the `pre-commit` hook. This is great for `nvm` users, but I don't think the hook should be polluted for all users based on the preferences of some.

Now that I know the husky profile script exists, I think that's a better solution. I may update our docs to state this. 