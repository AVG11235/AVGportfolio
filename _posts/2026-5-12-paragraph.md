---
toc: false 
layout: post
title: Final Details
description: the proof I deserve college credit and also to pass this class
permalink: /finale/
---

### <font color="violet"> Row of contents </font>

Fun thing to note most of the rubric rules require you too review the code of the game, I believe most if not all code requirements have been met so to make it easier for you to read them and for me to double check I'm gonna list up each code requirement and connect it to a section of my game's code with a neat little description as well, so here's a table of links across this page that make it easy to go to any wanted rubric requirement instantaneously:

### Object-Oriented Programming	

* [Writing Classes](#writer),	Create minimum 2 custom character classes extending base classes	Code review: Player.js, NPC.js, Enemy.js, 
* [Methods & Parameters](#meth),	Implement methods with parameters (e.g., collisionHandler(other, direction))	Code review: Method signatures with 2+ parameters
* [Instantiation & Objects](#instant),	Instantiate game objects in GameLevel configuration	Code review: GameLevel setup objects
* [Inheritance (Basic)](#inherit),	Create class hierarchy with 2+ levels (e.g., GameObject → Character → Player)	Code review: extends keyword, inheritance chain
* [Method Overriding](#method),	Override parent methods (update(), draw(), handleCollision())	Code review: Polymorphic implementations
* [Constructor Chaining](#construct),	Use super() to chain constructors	Code review: super(data, gameEnv) calls

>Lastly, the rubric objectives will be separated across pages depending on their category so here on the first of these pages that link to each other, the first category we will cover is ... OBJECT-ORIENTED PROGRAMMING!!!


<a id="writer"></a>

## <font color="yellow"> Writing Classes </font>

Writing classes means litterally just making a class, the was litterally seen in, *bullet.js*  and here **,
 <div class="image-gallery">
  <img src="{{site.bas constructor(data = null, gameEnv = null) {
        super(data, gameEnv);
        // Increment static player counter and assign unique id
        Player.playerCount = (Player.playerCount || 0) + 1;eurl}}/images/final-images/bullet-class.png" alt="Image 1">
  <img src="{{site.baseurl}}/images/final-images/shooterplayer-class.png" alt="Image 2">
</div> 
and in many more places, I'd be suprised if you made a game with 1 or no classes at all failing this objective spectacularly! Overall classes don't just cover the constructor, they cover 90% of these .js files look how little there is after the end of the bullet.js class that isn't class code (it's what's there after the yellow })
<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/bullet-class-end.png" alt="Image 3">
</div>

<a id="meth"></a>

## <font color="blue"> Methods & Parameters </font>

Methods and Parameters covers stuff like handlers and although there are uses of "handle"'s in level3.js, the only 2 times "handler" appears in my custom code (it may also be in player.js) is in the variant version I made of Npc.js called enpeecee.js which has slightly different styles of functions while keeping the original Npc.js available for other use, here they are, one for he destroy function, and the other for clean up work!

 <div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/enpeecee-handler.png" alt="Image 4">
  <img src="{{site.baseurl}}/images/final-images/enpeecee-handler-clean.png" alt="Image 5">
</div> 

>more examples of methods and parameters

```
   update() {
        if (this.gameOver) return;

        // Get player from game objects (created by GameLevel system)
        const player = this.gameEnv.gameObjects.find(obj => obj instanceof ShooterPlayer);
        if (!player) return;

        // Spawn enemies every 3 seconds
        const currentTime = Date.now();
        if (currentTime - this.lastSpawnTime > this.enemySpawnRate) {
            this.spawnEnemy();
            this.lastSpawnTime = currentTime;
        }

        // Update enemies and remove expired ones
        this.enemies.forEach((enemy, index) => {
            enemy.update();
            // Remove enemy after 5 seconds if not shot
            if (currentTime - enemy.spawnTime > this.enemyLifetime) {
                enemy.destroy();
                this.enemies.splice(index, 1);
            }
        });

        // Check bullet collisions with enemies
        player.bullets.forEach(bullet => {
            this.enemies.forEach((enemy, enemyIndex) => {
                if (bullet.checkCollision(enemy)) {
                    // Create hit marker at enemy position
                    const hitMarker = new HitMarker(
                        enemy.x + enemy.width / 2, // Center of enemy
                        enemy.y, // Top of enemy
                        this.gameEnv
                    );
                    this.gameEnv.gameObjects.push(hitMarker);

                    // Create explosion at enemy position
                    const explosion = new Explosion(
                        enemy.x + enemy.width / 2,
                        enemy.y + enemy.height / 2,
                        this.gameEnv
                    );
                    this.gameEnv.gameObjects.push(explosion);

                    // Enemy defeated!
                    bullet.destroy();
                    enemy.destroy();
                    this.enemies.splice(enemyIndex, 1);
                    this.score++;
                    this.scoreDisplay.textContent = `Wolves Eliminated: ${this.score}`;
                }
            });
        });
```
```
    update() {
        this.draw();
        this.collisionChecks();
        this.move();
    }
```
```
    draw() {
        // Clear the canvas before drawing
        this.clearCanvas();

        if (this.spriteSheet) {
            // Draw the sprite sheet frame
            this.drawSprite();
            // Update the frame index for animation
            this.updateAnimationFrame();
        } else {
            // Draw default red square
            this.drawDefaultSquare();
        }

        // Set up the canvas dimensions and styles
        this.setupCanvas();
    }
```
<a id="instant"></a>

## <font color="red"> Instantiation & Objects </font> 

For this segment Instantiation and objects covers how gamelevel's in the game engine set up objects in the game like characters, background areas, collisions, coins, bullets, etc. which for example can be seen here: in the class of Game Object constructor it sets up creates and instances which are used and applied in other parts of game object throughout all "Game Object"'s.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/game-object-create.png" alt="Image 6">
</div> 

<a id="inherit"></a>

## <font color="green"> Inheritance (basic) </font>

This one's a classic, the heirarchy system of classes using phrases like "extends" it's when a file of code is reused specified, and improved across multiple new creations of code like character.js being extend to make player.js or enemy.js, and those files extend to become shooterplayer.js and wolf.js, making grand new creations while baseing code off previous more general work, even character.js is an expansion of GameObject.js, increaseing efficiency and customization, without starting fresh every time and to produce checkpoints or landmarks of to take notes of or as an acheivement. this can be seen below: 

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/characterextend.png" alt="Image 7">
  <img src="{{site.baseurl}}/images/final-images/characterimport.png" alt="Image 8">
  <img src="{{site.baseurl}}/images/final-images/playerimex.png" alt="Image 9">
  <img src="{{site.baseurl}}/images/final-images/shooterimex.png" alt="Image 10">
</div> 

<a id="method"></a>

## <font color="gray"> Method Overriding </font>

Method Overriding includes "update", "draw", and "handlecollision" as they're parent methods in its work.

 What Method Overriding it does is A subclass defining its own version of a method that already exists in the parent class.

The method name and parameter list stay the same. The subclass implementation replaces the parent behavior for that object type. Which can be seen below: like If GameObject or Entity has: update(), draw(), handleCollision(). Then a child class like Player, Enemy, or NPC can override them: as seen using Player.update() controls player movement and input, Enemy.update() controls enemy AI or patrol logic, Player.draw() renders the player sprite, Enemy.draw() renders enemy sprites, Player.handleCollision() reacts to walls, items, bullets, Enemy.handleCollision() reacts to the player, obstacles, spells. 

This proves polymprphic implementation, meaning Code outside doesn’t need to know the exact subclass
It can call object.update() or object.handleCollision(...)
The correct subclass version runs at runtime.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/game-object-update.png" alt="Image 11">
  <img src="{{site.baseurl}}/images/final-images/game-object-draw.png" alt="Image 12">
  <img src="{{site.baseurl}}/images/final-images/enemy-update.png" alt="Image 14">
  <img src="{{site.baseurl}}/images/final-images/enpeecee-update.png" alt="Image 15">
  <img src="{{site.baseurl}}/images/final-images/playerupdatehandlecoll.png" alt="Image 16">
  <img src="{{site.baseurl}}/images/final-images/game-object-handle-1.png" alt="Image 17">
  <img src="{{site.baseurl}}/images/final-images/game-object-handle-2.png" alt="Image 18">
  <img src="{{site.baseurl}}/images/final-images/game-object-handle-3.png" alt="Image 19">
</div> 

(ooh look "override" in player.js!, 146 to 175!)

```
   /**
     * Overrides the reaction to the collision to handle
     *  - clearing the pressed keys array
     *  - stopping the player's velocity
     *  - updating the player's direction   
     * @param {*} other - The object that the player is colliding with
     */
    handleCollisionReaction(other) {    
        // Do NOT clear pressed keys; keep walking animation active
        // Halt movement by zeroing velocity along collision axis

        // Avoid DOM-based push-out; rely on velocity zeroing only
            // Do NOT clear pressed keys; keep walking animation active
            // Halt movement by zeroing velocity along the touched axes; avoid DOM-based push-out
            try {
                const touchPoints = this.collisionData?.touchPoints?.this;
                if (touchPoints) {
                    // Horizontal block
                    if (touchPoints.left || touchPoints.right) {
                        this.velocity.x = 0;
                    }
                    // Vertical block
                    if (touchPoints.top || touchPoints.bottom) {
                        this.velocity.y = 0;
                    }
                }
            } catch (_) {}

        super.handleCollisionReaction(other);
    }
```

<a id="construct"></a>

## <font color="aquamarine"> Constructor Chaining </font>

basically using super() to chain constructors like: 

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/super-player.png" alt="Image 20">
</div>

<style>
.btn-controlstructures { background-color: #100977ff !important; color: white !important; }
.btn-controlstructures:hover { background-color: #461ff3ff !important; }
</style>

<div class="btn-group">
    <a href="{{site.baseurl}}/finale/controlstructures" class="btn btn-controlstructures">Control Structures</a>
</div>

<br>