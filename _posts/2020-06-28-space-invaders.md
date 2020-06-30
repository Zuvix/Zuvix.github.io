---
layout: post
title: Space Invaders Pygame
subtitle: Best way to practice Python
gh-repo: Zuvix/Python-pygame-experiment
gh-badge: [star, fork, follow]
tags: [gamedev, python]
comments: true
---
When schools are closed and going outside is dangerous, how else can we programmers procrastinate and evade working on master thesis, than to work on a game. This time i decided step out of Unity engine and experiment with Python. For today the game we will be working on is [Space Invaders](https://en.wikipedia.org/wiki/Space_Invaders) with a few added features to spice up the old retro gaming experience. 

## About the game
Space Invaders is an arcade game designed by Tomohiro Nishikado in 1978. The plot of the game is simple. Aliens are invading Earth and it is your job to defend your home planet from the 3 types of descending Invaders before they destroy you or touch the ground.

In Space Invaders, you control the Core Cannon, which moves horizontally across the screen firing at aliens. The aim is to shoot all of the aliens in 5 rows of 11. They move back and forth and every time they complete and get back to where they started they move down one row. Their goal is to reach the ground (Earth) and invade, for which then the player loses. Source: [Fandom wiki](https://spaceinvaders.fandom.com/wiki/Space_Invaders).

In our interpretation of the game we focus to recreate the basic concepts of the game as precisely as possible, while adding new content to the game. In a table below we summarize the differences between original and our version: 

| Version | Player control |  Player bullet | Alien count | Alien shooting | Ufo | Audio | Walls | Powerups | Calamities |  
| :------ |:--- | :--- |  :--- |  :--- |  :--- |  :--- |  :--- |  :--- |  :--- |
| Original | L,R,Shoot | One at time | 55 | All in last row | Points | Original | Multiple | None | None |
| Pygame | L,R,Shoot | One at time | 55 | All in last row | Reworked | Imitated | None | TODO | TODO |

Two new terms emerged in this table: Powerups and Calamities. Both share the same idea, to break the linear gameplay. Calamity is a negative event in which aliens gain some sort of special power. For example some of the aliens change their color and instead of shooting they gain the ability to reproduce themselves. Another example that comes in my mind is that 4 of the same type of alien merge into one big alien with bonus HP and new shooting patterns. On the other hand powerups are to benefit the player, allowing him to break the limits like having multiple bullets at once, bullets destroying multiple aliens or even making a clone of the player ship.

In the older version the Ufo alien that passes the screen once in a while was all about bonus points. We feel it could be doing more than that. If a player snipes the ufo he gets the new powerup, if he fails the calamity falls upon him. To make this design sound less like a win/lose situation for player, calamities would allow him to gain more points(reproduced or merged aliens are giving you extra points for killing them). **This is still a work in progress and is not implemented in the latest version on github.


## Code behind the game
[Pygame](https://www.pygame.org) is set of Python modules running on top of [SDL](http://www.libsdl.org) library designed to create video games.