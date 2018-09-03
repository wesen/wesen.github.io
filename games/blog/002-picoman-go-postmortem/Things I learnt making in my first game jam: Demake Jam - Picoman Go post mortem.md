# Things I learnt making in my first game jam: Demake Jam - Picoman Go post mortem
#pico8 #patreon #gamedev

This article is about the development of my [Demake Jam](https://itch.io/jam/demake-jam) entry [Picoman Go](https://wesen3000.itch.io/picoman-go), a demake of the Square Enix title [Hitman Go](https://hitmango.square-enix-games.com/us/) using the [Pico-8](https://www.lexaloffle.com/pico-8.php) fantasy console. In Hitman Go, you move your character, the hitman, around a board made of nodes connected by links, and try to reach the exit. You can only move along links. Unfortunately, numerous enemies block the way. As soon as they see you, and you are one link away, you die and have to start the level from the beginning. Behind these very simple rules hides an ingenuous puzzle game. The graphics are gorgeous, using a minimalistic 3D style and board game pieces for the characters. The theme of the demake jam is to take an existing game, and transform it into a "smaller" version. For example, turning a 3D game into a 2D game, or making all the graphics be in 16 colors. 

This blog post is about the development of my demake, and what I learnt doing it. For the impatient crowd out there, here's a TLDR:

* every player interaction needs to have visual and auditory feedback
* spend a lot of time on finish and presentation. This is what makes or breaks a good game idea
* polish and gameplay refinement takes a lot more time than implementing game mechanics
* having people play the game immediately uncovers a lot of issues that need addressing. I spent the whole week alone, but should have shared by game much sooner.
* having an integrated (yet limiting) environment is fantastic for creativity
* making even a simple game takes a lot of work and time
* game development is fun!

## Taking a week off work to do game development
After having participated in [LD42](https://ldjam.com/events/ludum-dare/42/theme), which was my first competition, and using [pico-8](https://www.lexaloffle.com/pico-8.php) as an engine, I was hooked. I am new to game development, but am a seasoned systems programmer (embedded and linux). I dabbled in unity and C#, but a full-featured engine is intimidating, and I found it stifling my creativity. Should I go 3D or 2D? Should I use anima 2D? Should I use sprites? Which sprite editor do I want? Do I use tilemaker? Pico-8 however restricts you in fairly significant ways, while freeing you up in others. It comes with a code editor, a sprite editor, a map editor, a sound editor, and a music editor. In about 10 minutes, you can go from a blank screen to a moving player sprite with collision detection and sound effects. On the other hand, you can only have up to 8192 tokens, 16 colors, 128x128 pixels on screen, and 4 channels of limited synthesizer sounds.

After LD42, I took a week off from work to focus on game development, and spent the first day going through [Wilmer Lin's Udemy course](https://www.udemy.com/make-an-assassins-go-board-game-in-unity/) about making a [Hitman Go](https://hitmango.square-enix-games.com/us/) clone in unity. If you don't know hitman go, do yourself a favor and buy it. There also is a [wonderful postmortem talk](https://www.youtube.com/watch?v=0a9Rh997RPo) Daniel Lutz held at GDC. It is a fantastic mobile puzzle game, with a beautiful and minimalistic interface, and great levels. 

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/BF6727FD-082E-4034-BAA8-145703773D0D.png)

The udemy course taught me a great amount of things. I learnt to polish menu and player interactions, I constructed a whole level out of boxes, I implemented the graph construction using the weird idea (to me) of using collision boxes to encode graph edges. I learnt about  game feel elements making the game's minimalistic controls more interactive. The result of the tutorial was a fairly polished, if simplistic, level of Hitman Go. I was hungry for more.

## Joining the Demake Jam
Having had so much fun on the previous weekend's Ludum Dare, I looked for more game jams. The [Demake Jam](https://itch.io/jam/demake-jam) had just started, and it seemed obvious to build a demake of hitman go! I decided to focus on the game polish elements that felt daunting to me until the unity course:
* Level polish
* Animations and tweening
* Level selection
* Intro screen and music
* Incorporate nice graphics into the level layouts

Furthermore, I wanted to play with some more advanced features of pico8:
* [https://www.lexaloffle.com/bbs/?tid=3458](https://www.lexaloffle.com/bbs/?tid=3458)
* Object oriented lua with [https://www.lexaloffle.com/bbs/?tid=3342](https://www.lexaloffle.com/bbs/?tid=3342)
* Tweening using the [PICO-Tween library](https://github.com/JoebRogers/PICO-Tween)

## Writing a tweening coroutine
[Source code](https://github.com/wesen/carts/commit/070ede1bd05e3c1456a20623c87e39ea92c7ae43)

Implementing the game mechanics feels like the easiest part to me because of my programming background. I reused some class construction code from the BBS, added my hoarders code to manage actors and tick coroutines, and implemented a tweened move_to method to move a circle. Off to a great start!

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_4.gif)

## Level animation and the OO approach
[Source code](https://github.com/wesen/carts/commit/a52238fb494fb451e4d22f8937eee483090e935c)

The next big step was to construct a level and animate the display on screen. The code immediately started getting messy because I was trying to reuse the approach taken in the unity tutorial, where graph building is intricately linked to its display animation. I threw in some coroutines into the mix, some easing, and after stirring a little bit, I had my first working graph display. 

I kept a standard OO layout, with a single god class that represents the actual board. That kept my actual main routines (_draw, _update and _init) very small, and would allow me to create multiple levels more easily in the future. I used the level editor to design my initial animation, because getting the timing right with easing (for 4 frames!) was too fiddly. I also started using the level editor as a way to encode my graph, by flagging sprites as nodes and links.

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_6.gif)

## Player control and movement, first mistakes
[Source code](https://github.com/wesen/carts/commit/b5733f536a7c8a977cc6bedd78d33dac43014593)

Things were now starting to get serious, and I implemented the player character. My favourite part of pico-8 is the sprite editor and its 16 color palette. While it is frustrating to have very limited options for shading, the result is that all my pixel-art efforts ended up looking decent and stylistically consistent. 

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_000.png)

I implemented player movement using a mix of coroutines. Being new to the coroutines used in lua, I actually didn't think my approach through, and ended up spawning them in a weird way. Instead of calling functions that were also using yield(), I ended up spawning "sub-coroutines", and ticking them inside coroutines. Not that big of an issue, but something I should have thought through better. 

```

function add_cr(f)
 local cr=cocreate(f)
 add(crs,cr)
 return cr
end

function wait_for_cr(cr)
 if (cr==nil) return
 while costatus(cr)!='dead' do
  yield()
 end
end

function wait_for_crs(crs)
 local all_done=false
 while not all_done do
  all_done=true
  for cr in all(crs) do
   if costatus(cr)!='dead' then
    all_done=false
    break
   end
  end
 end
end

function run_sub_cr(f)
 wait_for_cr(add_cr(f))
end
```

I also made my biggest mistake at this point. The animation for moving the player takes slightly too long, and because I block input for the whole animation, moving the player doesn't feel right. I decided it wasn't a big deal, and left it as is. I realized later on (after posting it to the jam website) that it was a big deal, and it was immediately pointed out by everybody playing the game. Lesson learnt, and game feel was something I focused on in my follow-up game. 

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_7.gif)

This is also the point where the dynamic typing of lua started to show its uglier head. Not having to specify class structure along with types, I started pushing things indiscriminately into the player table (similar to javascript, everything in lua is a table). I started using node and link tables to indicate position, and had different ways of representing directions: either as an integer, or as a vector. While not terrible, this ended up costing me some time. Writing everything in the pico-8 editor didn't give me enough peripheral context to realize I was starting to tangle up my code.

```
function class_mover:move(i)
 local direction=directions[i]
 local node=board:get_node_in_direction(self.node,direction)
 if node!=nil then
  local cr=add_cr(function()
   self.is_moving=true
   self.direction=i
   wait_for_cr(move_to(self,direction[1]*16,direction[2]*16,1,outexpo))
   self.x=0
   self.y=0   
   self.node=node
   self.is_moving=false
  end) 
  return cr
 end
end
```

## Adding enemies and a game loop
[Source code](https://github.com/wesen/carts/commit/49c21c4924d5c4f7e167e08eb85f4ec50e82b4e7)

The next step was adding enemies and using a proper gameloop to handle turns and game ending conditions. Coroutines really shine here, as they make the implementation of the gameflow coherent and readable. 
```
function class_game:play_game_loop()
 
 return add_cr(function()
  while true do
   self:play_start_screen()
 
   while true do
    if (self:play_metalevel()) break
    self:play_normal_level() 
   end
   
   self:play_finish_game()
  end
 end)
end

function class_game:play_start_screen()
 self.current_level=dbg_start_level

 if not dbg_skip_start then
  self.state=state_start_screen
  wait_for_cr(fade(true))  
  music(start_screen_music,music_fade_duration)
  pal()
  while not (btnp(4) or btnp(5)) do
 	 yield()
 	end
  music(-1,music_fade_duration)
 	sfx(start_screen_sfx)
 	printh("start game")
 	blink()
 	wait_for_cr(fade())
 end 	
end
```
Also note the addition of little UI elements like the arrows indicating when the player can move. The demake aspect of the jam was perfect, because I could reuse all the carefully thought out UI and gameplay features of the original. I learnt a tremendous amount doing that.

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_8.gif)

## Implementing base mechanics
[Source code](https://github.com/wesen/carts/commit/f61b9ede10fc8b2568bb3fc613098fbcad35819b)

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_9.gif)

At this point, I had a solid basis to move forward, and started implementing more general game features:
* Level ending cards, with animation and graphics
* Particles, smoke effects
* Player death
* Level selection, encoding multiple levels on the map,
* Game progress

This taught me that actual mechanics is maybe the part of the game that requires the least work. Implementing the graph animation of the level ending card was three times the work required to implement enemy graph search and the rock throwing logic. 

## Feeling the urge to stop, and moving forward
[Source code](https://github.com/wesen/carts/commit/9c3861428edae9be5fd581e6b1e5085606e38e05)

At this point, I felt the urge to call it a day, simple game mechanics were completed, what did I want more. However, my goal for this week off work was to challenge myself, so I decided to implement as much polish as I could before the deadline. This is where things really came together. I added:
* camera shake,
* fades between level screens, 
* variables to skip certain aspects of the game to make development easier
* blinking text and enemies
* an end game screen
* sound effects
* the mechanic of hiding in plants

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_14.gif)

Once I added more enemies, I stumbled upon a big problem. I could have multiple enemies on the same node, and while that is fairly easy to display on a high resolution screen, I couldn't just overlay multiple sprites in the same 8x8 area. Pico-8's limitation forced me to design a proper solution to the problem. I tried multiple things, count indicates above the enemies, but that would lose the information about their orientation. I finally figured out that adding a border and staggering the sprites allowed me to display up to 5 enemies, which was the maximum enemies getting stacked anyway. Without pico-8's limitations, I would not have scouted the design space as deeply as I did. In the same vein, one mechanic allows the player to hide in a plant. Again, I had to find a clever solution by designing a special sprite that would be swapped in for the plant sprite, and hide the player sprite.

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_13.gif)

## Adding the rock mechanic
[Source code](https://github.com/wesen/carts/commit/4364efb7cdd997111bd28d9be423a4c44b01321e)

The most complicated mechanic in the game is throwing rocks to distract enemies. Not only does it require some special UI elements (I chose yellow arrows which are slightly confusing. Some players figured it out right away, some didn't have a clue what they meant), but it also requires to run some pathfinding for enemies, and keep track of consumed objects. I also had to add a feedback element showing which enemies heard the sound and which didn't. Again, the original game was a good source of inspiration, and helped me implement something that wasn't entirely bad. As you can see, I also added the briefcase mechanic, with a very bad sprite that is incomprehensible at the bottom left. I later on colored it brown and orange, which made it much more legible.
 
![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_15.gif)

You can see my beautiful implementation of breadth-first search in pico-8 lua here.

```
function class_board:get_path(from,to)
 local next={from}
 local crumbs={}
 crumbs[v_idx(from.x,from.y)]="start"
 while #next>0 do  
  local cur=popend(next)
  for n in all(self:get_neighbors(cur.x,cur.y)) do
   local v=v_idx(n.x,n.y)
   if crumbs[v]==nil then
    insert(next,n)
    crumbs[v]=cur
   end
  end
  if (cur==to) break  
 end

 local path={}
 local cur=to
 while cur!=from and cur!=nil do
  add(path,cur)
  cur=crumbs[v_idx(cur.x,cur.y)]
 end
 printh("finished path")
 
 return path
end

function class_board:get_neighbors(x,y)
 local res={}
 if (self.links[v_idx(x-1,y)]!=nil) add(res,self.nodes[v_idx(x-2,y)])
 if (self.links[v_idx(x+1,y)]!=nil) add(res,self.nodes[v_idx(x+2,y)])
 if (self.links[v_idx(x,y-1)]!=nil) add(res,self.nodes[v_idx(x,y-2)])
 if (self.links[v_idx(x,y+1)]!=nil) add(res,self.nodes[v_idx(x,y+2)])
 return res
end
```

## Beautification and music
[Source code](https://github.com/wesen/carts/commit/b671d9b58e7471471c2bdbbccb6a5eda7e354f17)

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_002.png)

I made a "pretty" intro screen on thursday afternoon, and overcame my fear of making pixel fonts. I have an irrational fear of trying out artistic ideas, and go through tutorial after tutorial before making something on my own. This is a fear I am trying to consciously overcome, and in this case, I just dove straight in. You can see some of the title screen sketches in the following pictures.

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/pico-go_16.gif)

Friday afternoon was spent on beautification, making better sprites and dressing up the levels. We are now nearing the end. I was very wary of this stage, because I am not great at pixel art. It took me quite a while to make everything look better, and I sketched out a lot of ideas on paper. This helped a lot, and made me realize how much these big sheets of newsprint helped (sadly, they got roughed up quite a bit in my backpack). 

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/IMG_0299.jpg)


![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/IMG_0301.jpg)

I also scrapped my ridiculous attempts at an atmospheric score, and I spent saturday afternoon scoring [Schubert's Ellen's song](https://www.youtube.com/watch?v=mVMmIJiqSJc) in the tracker, attempting to make the lead synth sound like a human voice. I can't hear this song ever again.

![](Things%20I%20learnt%20making%20in%20my%20first%20game%20jam:%20Demake%20Jam%20-%20Picoman%20Go%20post%20mortem/IMG_0300.jpg)

## Final touches
[Source code](https://github.com/wesen/carts/commit/f5fd190459c2516694d7d7797e19c40cea7ccec6)

By sunday, I was quite ready to submit the game to the jam. The only things I touched up were some particles in the explosions to make them less obnoxious.

## Lessons learnt
Analyzing this brilliant game taught me a great amount. Demaking a game is probably the best way of analyzing it, because you encounter the same issues that the original designers had. The limitations enforced by pico-8 actually fostered my creativity and design skills, and never really got in the way. To recap, a few of the lessons I learnt:
* smooth player interaction is crucial (I messed that up)
* every player interaction needs to have visual and auditory feedback
* spend a lot of time on finish and presentation. This is what makes or breaks a good game idea
* polish and gameplay refinement takes a lot more time than implementing game mechanics
* having people play the game immediately uncovers a lot of issues that need addressing. I spent the whole week alone, but should have shared by game much sooner.
* it is very easy to get messy in game code, and OO doesn't have a very good "impedance match". I will have to look into ECS and other patterns in the future.
* while I am not in love with lua, it doesn't stand in the way, and gives enough flexibility to implement everything needed
* having an integrated (yet limiting) environment is fantastic for creativity
* making even a simple game takes a lot of work and time
* game development is fun!

You can play the final game over at [Picoman Go by wesen3000](https://wesen3000.itch.io/picoman-go).