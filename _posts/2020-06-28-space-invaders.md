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

![Gameplay](/assets/img/gameplay.png)

In our interpretation of the game we focus to recreate the basic concepts of the game as precisely as possible, while adding new content to the game. In a table below we summarize the differences between original and our version: 

| Version | Player control |  Player bullet | Alien count | Alien shooting | Ufo | Audio | Walls | Powerups | Calamities |  
| :------ |:--- | :--- |  :--- |  :--- |  :--- |  :--- |  :--- |  :--- |  :--- |
| Original | L,R,Shoot | One at time | 55 | All in last row | Points | Original | Multiple | None | None |
| Pygame | L,R,Shoot | One at time | 55 | All in last row | Reworked | Imitated | None | TODO | TODO |

Two new terms emerged in this table: **Powerups** and **Calamities**. Both share the same idea, to break the linear gameplay. **Calamity** is a negative event in which aliens gain some sort of special power. For example some of the aliens change their color and instead of shooting they gain the ability to reproduce themselves. Another example that comes in my mind is that 4 of the same type of alien merge into one big alien with bonus HP and new shooting patterns. On the other hand **Powerups** are to benefit the player, allowing him to break the limits like having multiple bullets at once, bullets destroying multiple aliens or even making a clone of the player ship.

In the older version the Ufo alien that passes the screen once in a while was all about bonus points. We feel it could be doing more than that. If a player snipes the ufo he gets the new powerup, if he fails the calamity falls upon him. To make this design sound less like a win/lose situation for player, calamities would allow him to gain more points(reproduced or merged aliens are giving you extra points for killing them). 

**This is still a work in progress and is not implemented in the latest version on github**.


## Code behind the game
[Pygame](https://www.pygame.org) is set of Python modules running on top of [SDL](http://www.libsdl.org) library designed to create video games. The main reason to choose Pygame high portability and ability to run on nearly every platform and operating system. You don’t need expensive pc to run Unity engine, you can create a game like this one on your Raspberry Pi. All you need is to learn some Python and follow Pygame documentation or tutorials to get you going. You can check the code or try the game by cloning this [github repo](https://github.com/Zuvix/Python-pygame-experiment) and running the firstGame.py script.

First thing we do in Pygame is to initialize a window. To make it more simple we used a static screen size and docked the window in the middle of the screen. 

{% highlight python linenos %}
os.environ['SDL_VIDEO_CENTERED'] = '1'
pygame.init()
win_height = 600
win_width = 800
win = pygame.display.set_mode((win_width, win_height))
{% endhighlight %}

Next thing was to get the player behaviour working. For this we made a class called Player with variables describing position, size, speed and shooting ability. We assigned a loaded image for this class and created a function to draw the player sprite. 

{% highlight python linenos %}
class Player:
    player_image = img_p
    player_image = pygame.transform.scale(player_image, (42, 21))

    def __init__(self, x, y, width, height, vel):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.vel = vel
        self.can_fire = True

    def draw(self, window):
        window.blit(self.player_image, (self.x, self.y))
  {% endhighlight %}

We want our player to move left/right by holding the corresponding key on keyboard. We simply change the player X position when we capture the event from our keyboard. When the scene is updated by redrawing all of it’s objects so is the position of player.

{% highlight python linenos %}
def handle_player_input():
    global player_bullet
    global player_shot_count
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player.x > 0:
        player.x -= player.vel
    if keys[pygame.K_RIGHT] and player.x < win_width - player.width:
        player.x += player.vel
    if keys[pygame.K_SPACE] and player.can_fire:
        player.can_fire = False
        player_shot_count += 1
        player_bullet = RectProjectile(4, 16,
                                       round(player.x + player.width // 2 - 1),
                                       round(player.y - 1), (0, 200, 0), 9,
                                       True)
{% endhighlight %}

In a similar way we also handled the Enemy class and it’s behaviour. For each type of game object we save it in a list of objects with similar type. The mentioned redrawing of all game objects is done within dedicated function. 

{% highlight python linenos %}
def redraw_game_window():
    win.fill((0, 0, 0))
    player.draw(win)
    for bullet in bullets:
        bullet.draw(win)
    if player_bullet != None:
        player_bullet.draw(win)
    for enemy in enemies:
        enemy.draw(win)
    if ufo:
        ufo.draw(win)
    score_text.draw(win, "Score: " + str(score))
    level_text.draw(win, "Wave: " + str(iteration))
    pygame.display.update()
{% endhighlight %}

These code snippets should help you get the basic idea of how i manage the code in this pygame project. The complexity of whole project however is a lot higher because of the many interactions and events that can occur within the game. Also a negative i’m aware of is the length of the code. The game was supposed to be a short 200-400 line script, but in the end i couldn’t resist adding new features resulting in a huge lengthy code separated only by comment sections. Maybe one day i’ll get to refactor it to make it more readable and understandable.

## Conclusion

This project was a fun way for me to get more into Python and gave me a better understanding of how mechanics in retro games worked. Hope you liked the idea and someday i’ll finish the project and share it with Pygame community.

![Gameplay](/assets/img/start.png)

See you in the race to conquer the Space!
