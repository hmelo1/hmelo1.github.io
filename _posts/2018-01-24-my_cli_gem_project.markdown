---
layout: post
title:      "My CLI Gem Project"
date:       2018-01-24 23:30:36 +0000
permalink:  my_cli_gem_project
---


Started off my CLI Gem project pretty strong. I knew exactly what I wanted and (mostly) how to do it. But then came the hard part, actually doing it. 

Originally, the plan was to just going to pull prices straight off the Steam webpage(http://store.steampowered.com). I wanted the new price, the old price, the % off, and the reviews. However, I soon the site heavily relies on Javascript. So I scratched that idea and switched over to using SteamDB(https://steamdb.info). This made my life alot easier. I managed to pull most of the information I wanted directly from their sales page.

Then came another hiccup. SteamDB doesn't host the individual pricing and uses a Steam Widget. This time I got a bit crafty, I pulled the pricing straight from the Steam webpage. Now I've got everything I want. The game's name, original Price, the discounted price, the % off, the publisher and the devleoper.

I may eventually add some colorize into the mix to make it a bit more readable. But for now I'm satisfied.
