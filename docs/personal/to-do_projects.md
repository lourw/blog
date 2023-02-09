---
title: To-do Projects
---

For the projects that pop into my head, but I never have time to get around to:

- [ ] **webgrep**  
	a CLI that allows you to search for a piece of text within  URL. 
	- if pattern exists within the HTML of a site, return the surrounding HTML element or document object path. 
	- write the tool in rust
	- flags
		- r: recursive search through all a tags that belong to the same host
		- b: return true or false depending if the given pattern is in the webpage
		- t: set the timeout before page scans (to avoid ddossing) 
