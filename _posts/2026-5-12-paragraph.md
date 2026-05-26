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

``` js

class Bullet {
    constructor(data) {
        this.x = data.x;
        this.y = data.y;
        this.velocity = data.velocity || { x: 0, y: 0 };
        this.gameEnv = data.gameEnv;
        this.shooter = data.shooter;
        this.direction = data.direction || 'down'; // down, left, right, up
        this.width = 40;
        this.height = 40;
        this.lifetime = 10000; // 10 seconds
        this.creationTime = Date.now();
        this.destroyed = false;
        this.isVisible = true;
        this.frameIndex = 0;
        this.frameCounter = 0;
        this.animationRate = 3;
    }

    update() {
        // Move the bullet continuously in its direction
        this.x += this.velocity.x;
        this.y += this.velocity.y;

        // Check lifetime (10 seconds)
        if (Date.now() - this.creationTime > this.lifetime) {
            this.destroy();
            return;
        }
    }

    draw() {
        if (this.destroyed) return;
        
        // Always show yellow cube
        this.gameEnv.ctx.fillStyle = 'yellow';
        this.gameEnv.ctx.fillRect(this.x, this.y, this.width, this.height);
    }

    checkCollision(target) {
        if (this.destroyed || !target) return false;

        return this.x < target.x + target.width &&
               this.x + this.width > target.x &&
               this.y < target.y + target.height &&
               this.y + this.height > target.y;
    }

    destroy() {
        if (!this.destroyed) {
            this.destroyed = true;
            // Don't remove from gameObjects - just mark as destroyed
            // This allows bullets to be cleaned up naturally by the game
        }
    }
}

export default Bullet;
```

what this code has 

> Class Declaration

``` js
class Bullet
```
means this creates a blueprint for "bullet". If this were a player class, it could be used to make player objects

> Constructor

``` js
    constructor(data) {
```

The constructor runs automatically when a new object is created.

It sets up the bullet's starting data.

> "this"

``` js
        this.x = data.x;
        this.y = data.y;
        this.velocity = data.velocity || { x: 0, y: 0 };
        this.gameEnv = data.gameEnv;
        this.shooter = data.shooter;
        this.direction = data.direction || 'down'; // down, left, right, up
        this.width = 40;
        this.height = 40;
        this.lifetime = 10000; // 10 seconds
        this.creationTime = Date.now();
        this.destroyed = false;
        this.isVisible = true;
        this.frameIndex = 0;
        this.frameCounter = 0;
        this.animationRate = 3;
    }
```

this stores values inside the current object.

Each bullet object gets its own: x (data), y (data), velocity (x,y axis), gameEnv, shooter, direction, width, height, lifetime, creationTime, destroyed (t/f statement), isVisible (t/f statement), frameIndex, frameCounter, animation Rate.


> shoot function in shooterplayer.js

``` js
    shoot() {
        const currentTime = Date.now();
        if (currentTime - this.lastShotTime < this.shootCooldown) return;

        this.lastShotTime = currentTime;

        // Create bullet data based on facing direction
        let velocity = { x: 0, y: 0 };
        switch (this.facing) {
            case 'up': velocity.y = -6; break;
            case 'down': velocity.y = 6; break;
            case 'left': velocity.x = -6; break;
            case 'right': velocity.x = 6; break;
        }

        const bulletData = {
            x: this.position.x + this.width / 2 - 20,
            y: this.position.y + this.height / 2 - 20,
            velocity: velocity,
            gameEnv: this.gameEnv,
            shooter: this,
            direction: this.facing
        };

        const bullet = new Bullet(bulletData);
        this.bullets.push(bullet);
        this.gameEnv.gameObjects.push(bullet);
        console.log('Bullet spawned at', this.position.x, this.position.y, 'facing', this.facing);
    }
```

> Creating an Object From the Class

``` js 
        const bullet = new Bullet(bulletData);
```

this creates a new bullet object

> Using the Object

Input:

``` js
        this.bullets.forEach(bullet => bullet.destroy());

```

Output:

``` js
        // Clean up bullets when player is destroyed

```

Input:

``` js
        this.bullets = this.bullets.filter(bullet => {

```

Output:

``` js
        // Remove destroyed bullets

```

Input:

``` js
        const bullet = new Bullet(bulletData);
        this.bullets.push(bullet);
        this.gameEnv.gameObjects.push(bullet);
```

Output:

``` js
        console.log('Bullet spawned at', this.position.x, this.position.y, 'facing', this.facing);

```

> Explanation all together:

A class is a blueprint used to create objects. In this example, the Player class stores player data like name and health and includes methods that allow the player to move and take damage. The constructor initializes the object, while this stores values that belong to each specific player object.


<a id="meth"></a>

## <font color="blue"> Methods & Parameters </font>

> Methods

Methods are actions the object can perform.

This class has: update(), draw(), destroy(), checkCollision(target).

> Parameters

``` js
    checkCollision(target) {
```

"target" is a parameter passed into the method.

Methods and Parameters covers stuff like handlers too, and although there are uses of "handle"'s in level3.js, the only 2 times "handler" appears in my custom code (it may also be in player.js) is in the variant version I made of Npc.js called enpeecee.js which has slightly different styles of functions while keeping the original Npc.js available for other use, here they are, one for he destroy function, and the other for clean up work!

 <div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/enpeecee-handler.png" alt="Image 4">
  <img src="{{site.baseurl}}/images/final-images/enpeecee-handler-clean.png" alt="Image 5">
</div> 

>more examples of methods and parameters

``` js
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
``` js
    update() {
        this.draw();
        this.collisionChecks();
        this.move();
    }
```
``` js
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

Difference Between create() and Instantiation
Concept	Meaning
Instantiation	Creating individual objects
create()	Setting up larger systems or environments
Example
const player = new Player();

Instantiation:
creates ONE object.

GameEnv.create();

Setup method:
initializes the entire environment.

``` js
            // Load the sprite sheet
            this.spriteSheet = new Image();
```

For this segment Instantiation and objects covers how gamelevel's in the game engine set up objects in the game like characters, background areas, collisions, coins, bullets, etc. which for example can be seen here: in the class of Game Object constructor it sets up creates and instances which are used and applied in other parts of game object throughout all "Game Object"'s.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/game-object-create.png" alt="Image 6">
</div> 

<a id="inherit"></a>

## <font color="green"> Inheritance (basic) </font>

Files in the same folder can inherit things from one another using objects (key words) and IMPORTING data from other files to use in something new like EXTENDING from an object to a character to a player or an enemy.

Player class:
``` js
class Player extends Character
```
inherits from Character.

This one's a classic, the heirarchy system of classes using phrases like "extends" it's when a file of code is reused specified, and improved across multiple new creations of code like character.js being extend to make player.js or enemy.js, and those files extend to become shooterplayer.js and wolf.js, making grand new creations while baseing code off previous more general work, even character.js is an expansion of GameObject.js, increaseing efficiency and customization, without starting fresh every time and to produce checkpoints or landmarks of to take notes of or as an acheivement. this can be seen below: 

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/characterextend.png" alt="Image 7">
  <img src="{{site.baseurl}}/images/final-images/characterimport.png" alt="Image 8">
  <img src="{{site.baseurl}}/images/final-images/playerimex.png" alt="Image 9">
  <img src="{{site.baseurl}}/images/final-images/shooterimex.png" alt="Image 10">
</div> 

<a id="method"></a>

## <font color="gray"> Method Overriding </font>

Method overriding happens when a child class replaces a method from a parent class with its own custom version.

>This is important in games because different objects may need different behaviors even if they share the same base structure.

Example: Character.js: 
``` js
update() {
    super.update();

    if(!this.moved){
        if (this.gravity) {
            this.time += 1;
            this.velocity.y += 0.5 + this.acceleration * this.time;
        }
    }
    else{
        this.time = 0;
    }
}
```
Player.js
``` js
update() {
    this.draw();
    this.collisionChecks();
    this.move();
}
```

>Not completely replacing the parent behavior.

I also use:
``` js
super.update();
```
This means:

“Run the original Character update first, THEN add custom Player behavior.”

That is advanced OOP usage and very good rubric evidence.


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