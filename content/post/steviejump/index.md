---
title: StevieJump
author: admin
type: page
date: 2020-07-21T06:56:38+00:00
featured_image: "sJ_icon.png"

---
 

StevieJump is a fast paced jump&#8217;n&#8217;run&#8217;n&#8217;gun game. The goal is to climb an endless tower while fighting enemies and collecting upgrades, to get on the top of the scoreboard. 

I programmed Stevie Jump during the summer semester of the year 2020. The idea was to use my programming skills to create an application of my own completly from scratch. I started with an empty visual studio project and worked my way up using c++, openGL and many other libraries. 

<div style="text-align: center">
<a href="/setup.exe">
<button>
Download Game
</button>
</a>

</div>
Dependencies: OpenSSL

**LeaderBoard**
<iframe src="https://steven.schuerstedt.com/steviejump/board.html" title="Leaderboard" width="350" height="210" frameborder="0"></iframe>


{{< figure src="/sJ_ingame.png" title="ingame" width="100%" >}}

### How do you play the game?

For every higher platform you reach, you will get 10 points. Killing enemies will give you more points. The aiming is done with the mouse cursor, your weapon will get stronger if you collect more upgrades. As the game progresses the enemies will move faster and also the level is scrolling downwards faster. Once an enemy touches you, or you fall of a platform, the game is over. The game menu is openend by pressing ESC.



<div class="wp-block-media-text alignwide is-stacked-on-mobile" style="grid-template-columns:15% auto">
  <figure class="wp-block-media-text__media"><img loading="lazy" width="40" height="24" src="https://steven.schuerstedt.com/wp-content/uploads/2020/07/enemy.gif" alt="" class="wp-image-633" /></figure>
  
  <div class="wp-block-media-text__content">
    <p class="has-normal-font-size">
      <strong>Small enemy: </strong>This enemy type moves fast and is killed with one shot. It gives 100 points.
    </p>
  </div>
</div>

<div class="wp-block-media-text alignwide is-stacked-on-mobile" style="grid-template-columns:15% auto">
  <figure class="wp-block-media-text__media"><img loading="lazy" width="71" height="42" src="https://steven.schuerstedt.com/wp-content/uploads/2020/07/wizard_right-1.gif" alt="" class="wp-image-641" /></figure>
  
  <div class="wp-block-media-text__content">
    <p class="has-normal-font-size">
      <strong>Big enemy</strong>: This enemy moves slower and tanks up to 10 shots. It gives 300 points. This enemy only spawns when you progress far enough into the game.
    </p>
  </div>
</div>

<div class="wp-block-media-text alignwide is-stacked-on-mobile" style="grid-template-columns:15% auto">
  <figure class="wp-block-media-text__media"><img loading="lazy" width="119" height="119" src="https://steven.schuerstedt.com/wp-content/uploads/2020/08/ak74_green.gif" alt="" class="wp-image-663" /></figure>
  
  <div class="wp-block-media-text__content">
    <p class="has-normal-font-size">
      <strong>Weapon upgrade</strong>: This upgrade will increase the range and power of your weapon, while also decreasing the cooldown between shots. When you collect four upgrades you get an extra shot.
    </p>
  </div>
</div>





## Stevie Jump &#8211; behind the scenes 

{{< figure src="/sJ_progress.png" title="progress of development, each picture is three weeks apart" width="100%" >}}



Used Resources:

Libraries:

  * openGL, graphics
  * glfw, window handling
  * glm, mathematics
  * box2D, physics
  * imGUI, menues
  * DevIL, textures
  * irrKlang, sound/music
  * libcurl, http connection
  * micropather, A* pathfinding

Programs:

  * GIMP, creating animations and normal maps
  * famiTracker, 8 bit music

all assets are taken from itch.io

### Level Generation

The level you jump through is generated with an one dimensional cellular automata. A cellular automata is a grid of cells with different states. In the case of a elementary cellular automata there are only two states. The state of the neighbour cells influence if the state of a cell is changed within a timestep. <figure class="wp-block-image size-large is-resized">

<img loading="lazy" src="https://steven.schuerstedt.com/wp-content/uploads/2020/08/129_rules.png" alt="" class="wp-image-684" width="323" height="67" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/08/129_rules.png 902w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/129_rules-300x63.png 300w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/129_rules-768x161.png 768w" sizes="(max-width: 323px) 100vw, 323px" /> </figure> 

On this picture you can see a ruleset to describe the change of cells. Every elementary cellular automata is given a unique name, based on the ruleset.

For more information about cellular automata see <https://mathworld.wolfram.com/ElementaryCellularAutomaton.html>, or chapter 3.2 of my bachelor thesis <https://steven.schuerstedt.com/?page_id=17>.<figure class="wp-block-image size-large is-resized">

<img loading="lazy" src="https://steven.schuerstedt.com/wp-content/uploads/2020/08/129-1024x526.png" alt="" class="wp-image-685" width="404" height="207" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/08/129-1024x526.png 1024w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/129-300x154.png 300w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/129-768x395.png 768w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/129.png 1210w" sizes="(max-width: 404px) 100vw, 404px" /> <figcaption>rule 129</figcaption></figure> 

The beaty of cellular automata is, that they form interesting and diversified patterns. They are not completly random, so that no pattern is seen at all, but they also do not reproduce the same pattern again and again. They are complex. That why I decided to use different cellular automata to generate the level for my game. To generate the level the cellular automata is turned upside down, so the level advances at the top. Next, the vertical space between is set to a fixed value, to allow for more room for the player to move. At random the current rule of the cellular automata is changed, so that the generated patterns will change. This change can be seen when the texture of the blocks and also the music is changing. A downside of this approach is, that I cannot guarantee the generated level is actually beatable within the limits of the game physics. There are situations (which are very rare) where the player cannot advance further through the level. 

### Game Physics

For the game physics I used the box2D library. box2D is a very powerful library and I spent most time working on the game physics. The game physics deals with moving the player, the enemies and the bullets and also handles all collisions between those entities and the environment. <figure class="wp-block-image size-large is-resized">

<img loading="lazy" src="https://steven.schuerstedt.com/wp-content/uploads/2020/08/gamephysics.png" alt="" class="wp-image-686" width="474" height="213" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/08/gamephysics.png 887w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/gamephysics-300x135.png 300w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/gamephysics-768x346.png 768w" sizes="(max-width: 474px) 100vw, 474px" /> </figure> 

In order to move the player, an impulse is applied to the player object when a key is pressed. This information is given to the box2D library that calculates how the player and the environment respond to that event. The new position and rotation of the objects is then given to shader to draw the updated position.

### Enemy AI

The enemy ai was one of the hardest parts to program. I used micropather, a A\* path finding library. Path finding is the process of finding the cheapest path in a tree like graph structure. A\* is a very effective algorithm doing so. The hard part is to translate the problem into a graph. In my case I divided the level into different blocks, which are the nodes of the graph. If the block is passable, its cost is the distance from the current block. If it is not passable, the cost is at a max value. This information, together with the position of the enemy and the player in this graph, are given to the micropather algorithm, which then calulates the cheapes (and therefore shortest) path for the enemy to take. Now this path has to be translated back into impulses or forces for box2D, in order to move the enemy into the correct direction.

This process was very error-prone and for an easy fix I disabled the collision detection of the enemy completly. This results in the enemy sometimes walking right throught the corner of a block.<figure class="wp-block-image size-large is-resized">

<img loading="lazy" src="https://steven.schuerstedt.com/wp-content/uploads/2020/08/solvedpath.png" alt="" class="wp-image-687" width="388" height="180" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/08/solvedpath.png 593w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/solvedpath-300x140.png 300w" sizes="(max-width: 388px) 100vw, 388px" /> </figure> 



### Lighting

The lighting is done with normal mapping. The phong lighting model uses the normal of a surface to calculate the lighting value. Because 2D surfaces have no normal, normal-mapping &#8220;fakes&#8221; a normal by encoding it in rgb values, as seen below. This normal is then used to perform the lighting calculations. This way the geometry appears 3D, although it is just a 2D Texture. <figure class="wp-block-image size-large is-resized">

<img loading="lazy" src="https://steven.schuerstedt.com/wp-content/uploads/2020/08/brickwall02_normal.png" alt="" class="wp-image-688" width="306" height="230" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/08/brickwall02_normal.png 1020w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/brickwall02_normal-300x225.png 300w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/brickwall02_normal-768x576.png 768w" sizes="(max-width: 306px) 100vw, 306px" /> <figcaption>normal map of the background</figcaption></figure> 

The phong lighting model is a very broad estimation of light. Its main component is the so called lambertian term, which calculates the orientation of the surface in relation to the light source.

### Lessons learned:

Because I programmed the game in c++ with opengl and some other low level libraries, there was alot of work todo, and alot of things I had to do where not directly related to the game.

Many people might ask (including myself) why I did it that way, and not used a tool like unity, which would be easier to use and more effective to create a game. I think there are several reasons to use these low level tools for creating an application, one of them it being an educational experience for myself. (see <https://steven.schuerstedt.com/?page_id=505> for an indepth, philosopical explanation)

For the next project I would spent a lot of time thinking about what tools I want to use for the problem I want to solve and on what level between hardware and software I need to be in to solve it most efficiently.<figure class="wp-block-image size-large is-resized">

<img loading="lazy" src="https://steven.schuerstedt.com/wp-content/uploads/2020/08/vergleich.png" alt="" class="wp-image-689" width="457" height="375" srcset="https://steven.schuerstedt.com/wp-content/uploads/2020/08/vergleich.png 677w, https://steven.schuerstedt.com/wp-content/uploads/2020/08/vergleich-300x246.png 300w" sizes="(max-width: 457px) 100vw, 457px" /> </figure>