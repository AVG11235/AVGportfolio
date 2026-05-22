---
toc: false 
layout: post
title: Control Structures
description: the second section of rubric objectives 
permalink: /finale/datatypes/
---

### <font color="purple"> Listings </font>

the objectives we will cover in this category are:

 <div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/datatypes-section.png" alt="Image 1">
</div> 

### <font color="red"> Data Types </font>

* [Numbers](#no),	Position, velocity, score tracking	Code review: Numeric properties
* [Strings](#rope),	Character names, sprite paths, game states	Code review: String manipulation
* [Booleans](#boo),	Flags (isJumping, isPaused, isVulnerable)	Code review: Boolean logic
* [Arrays](#arr), 	Game object collections, level data	Code review: Array operations
* [Objects (JSON)](#obj), 	Configuration objects, sprite data	Code review: Object literals

<a id="no"></a>

## <font color="pink"> Numbers </font>

This objective just asks for a simple showcase of numeric data in the code, which I do have in the form of score tracking, (and a timer) in level 4, and velocity which is seen in my bullet and player movement so here's that code:

level4.js (constructor) 57-116:, 

``` js 
        // Minigame state
        this.score = 0;
        this.timeLeft = 60; // 60 seconds
        this.gameOver = false;
        this.enemies = [];
        this.enemySpawnRate = 2000; // Spawn every 2 seconds
        this.enemyLifetime = 5000; // Stay for 5 seconds
        this.lastSpawnTime = Date.now();

        // Score display
        this.scoreDisplay = document.createElement('div');
        this.scoreDisplay.style.position = 'absolute';
        this.scoreDisplay.style.bottom = '20px';
        this.scoreDisplay.style.right = '20px';
        this.scoreDisplay.style.background = 'rgba(0, 0, 0, 0.7)';
        this.scoreDisplay.style.color = 'beige';
        this.scoreDisplay.style.padding = '10px';
        this.scoreDisplay.style.borderRadius = '5px';
        this.scoreDisplay.style.fontSize = '36px';
        this.scoreDisplay.style.fontWeight = 'bold';
        this.scoreDisplay.style.zIndex = '1000';
        this.scoreDisplay.textContent = 'Wolves Eliminated: 0';
        document.body.appendChild(this.scoreDisplay);

        // Secret level banner
        this.secretBanner = document.createElement('div');
        this.secretBanner.id = 'secret-level-banner';
        this.secretBanner.style.position = 'absolute';
        this.secretBanner.style.top = '100px';
        this.secretBanner.style.left = '50%';
        this.secretBanner.style.transform = 'translateX(-50%)';
        this.secretBanner.style.background = 'rgba(0, 0, 0, 0.7)';
        this.secretBanner.style.color = 'red';
        this.secretBanner.style.padding = '10px 18px';
        this.secretBanner.style.borderRadius = '6px';
        this.secretBanner.style.fontSize = '32px';
        this.secretBanner.style.fontWeight = 'bold';
        this.secretBanner.style.zIndex = '1000';
        this.secretBanner.textContent = 'SECRET LEVEL 4: WOLF SHOOTING MINIGAME PRESS Q TO SHOOT';
        document.body.appendChild(this.secretBanner);
        setTimeout(() => {
            if (this.secretBanner && this.secretBanner.parentNode) {
                this.secretBanner.remove();
            }
        }, 10000);

        // Timer display
        this.timerDisplay = document.createElement('div');
        this.timerDisplay.style.position = 'absolute';
        this.timerDisplay.style.bottom = '20px';
        this.timerDisplay.style.left = '20px';
        this.timerDisplay.style.background = 'rgba(0, 0, 0, 0.7)';
        this.timerDisplay.style.color = 'beige';
        this.timerDisplay.style.padding = '10px';
        this.timerDisplay.style.borderRadius = '5px';
        this.timerDisplay.style.fontSize = '36px';
        this.timerDisplay.style.fontWeight = 'bold';
        this.timerDisplay.style.zIndex = '1000';
        this.timerDisplay.textContent = 'Time: 60';
        document.body.appendChild(this.timerDisplay);
```

shooterplayer.js shoot function:,

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

and player.js movement code (4-144):

``` js
// Define non-mutable constants as defaults
const SCALE_FACTOR = 25; // 1/nth of the height of the canvas
const STEP_FACTOR = 100; // 1/nth, or N steps up and across the canvas
const ANIMATION_RATE = 1; // 1/nth of the frame rate
const INIT_POSITION = { x: 0, y: 0 };


class Player extends Character {
    // Static counter for unique player IDs (uninitialized)
    static playerCount;
    /**
     * The constructor method is called when a new Player object is created.
     * 
     * @param {Object|null} data - The sprite data for the object. If null, a default red square is used.
     */
    constructor(data = null, gameEnv = null) {
        super(data, gameEnv);
        // Increment static player counter and assign unique id
        Player.playerCount = (Player.playerCount || 0) + 1;
        this.id = data?.id ? data.id.toLowerCase() : `player${Player.playerCount}`;
        this.keypress = data?.keypress || {up: 87, left: 65, down: 83, right: 68};
        this.touchOptions = data?.touchOptions || {interactLabel: "e", position: "left"};
        this.touchOptions.id = `touch-controls-${this.id}`;
        this.touchOptions.mapping = this.keypress;
        this.pressedKeys = {}; // active keys array
        this.bindMovementKeyListners();
        this.gravity = data.GRAVITY || false;
        this.acceleration = 0.001;
        this.time = 0;
        this.moved = false;
        // Initialize touch controls for mobile devices
        this.touchControls = new TouchControls(gameEnv, this.touchOptions);
    }

    /**
     * Binds key event listeners to handle object movement.
     * 
     * This method binds keydown and keyup event listeners to handle object movement.
     * The .bind(this) method ensures that 'this' refers to the object object.
     */
    bindMovementKeyListners() {
        addEventListener('keydown', this.handleKeyDown.bind(this));
        addEventListener('keyup', this.handleKeyUp.bind(this));
    }

    handleKeyDown({ keyCode }) {
        // capture the pressed key in the active keys array
        this.pressedKeys[keyCode] = true;
        // set the velocity and direction based on the newly pressed key
        this.updateVelocity();
        this.updateDirection();
    }

    /**
     * Handles key up events to stop the player's velocity.
     * 
     * This method stops the player's velocity based on the key released.
     * 
     * @param {Object} event - The keyup event object.
     */
    handleKeyUp({ keyCode }) {
        // remove the lifted key from the active keys array
        if (keyCode in this.pressedKeys) {
            delete this.pressedKeys[keyCode];
        }
        // adjust the velocity and direction based on the remaining keys
        this.updateVelocity();
        this.updateDirection();
    }

    /**
     * Update the player's velocity and direction based on the pressed keys.
     */

    updateVelocity() {
        this.velocity.x = 0;
        this.velocity.y = 0;

        this.moved = false;

        if (this.pressedKeys[this.keypress.right] || this.pressedKeys[this.keypress.left]) {
            this.moved = true;

            if (this.pressedKeys[this.keypress.right]) {
                this.velocity.x += this.xVelocity;
            }

            else if (this.pressedKeys[this.keypress.left]) {
                this.velocity.x -= this.xVelocity;
            }
        }

        if (this.pressedKeys[this.keypress.up] || this.pressedKeys[this.keypress.down]) {
            this.moved = true;

            if (this.pressedKeys[this.keypress.up]) {
                this.velocity.y -= this.yVelocity;
            }

            else if (this.pressedKeys[this.keypress.down]) {
                this.velocity.y += this.yVelocity;
            }
        }
    }

    updateDirection() {       
        // Single-key movement
        if (this.pressedKeys[this.keypress.up]) {
            this.direction = "up";
        } else if (this.pressedKeys[this.keypress.down]) {
            this.direction = "down";
        } else if (this.pressedKeys[this.keypress.right]) {
            this.direction = "right";
        } else if (this.pressedKeys[this.keypress.left]) {
            this.direction = "left";
        }

        // Multi-key movement
        if (this.pressedKeys[this.keypress.left] && this.pressedKeys[this.keypress.up]) {
            this.direction = "upLeft";
        } else if (this.pressedKeys[this.keypress.left] && this.pressedKeys[this.keypress.down]) {
            this.direction = "downLeft";
        } else if (this.pressedKeys[this.keypress.right] && this.pressedKeys[this.keypress.up]) {
            this.direction = "upRight";
        } else if (this.pressedKeys[this.keypress.right] && this.pressedKeys[this.keypress.down]) {
            this.direction = "downRight";
        }
    }

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
<a id="rope"> </a>

## <font color="orange"> Strings </font>

examples of strings are Character names, sprite paths, game states as seen in:

grandma enpeecee code in level3.js: 
``` js
  const grandmaData = {
            id: 'Grandma',
            src: path + "/images/gamify/lrrh-lvl3-grandma.png",
            SCALE_FACTOR: 6, // Shrink canvas (ie grandma and interaction physical box)
            STEP_FACTOR: 1000,
            ANIMATION_RATE: 50,
            INIT_POSITION: { x: 50, y: 150 },
            pixels: { height: 480, width: 480 },
            orientation: { rows: 1, columns: 1 },
            down: { row: 0, start: 0, columns: 1 },
            interactionRadius: 400, // Interaction area (now half the previous size)
            dialogues: ["WHAT ARE YOU STANDING AROUND FOR? GO KILL that WOLF that barged into MY HOUSE! WITH THE RIFLE I so courageusly gave to you dear-y <3"],
            // Use reaction property to trigger dialogue on collision
            reaction: function() {
                if (!this.grandma) this.grandma = grandmaRef;
                if (this.grandma && this.grandma.showReactionDialogue) {
                    // Update dialogue based on wolf defeat status
                    if (this.enemyDefeated) {
                        this.grandma.dialogueSystem.dialogues = ["Now, that's the girl I partially raised!!!"];
                    } else {
                        this.grandma.dialogueSystem.dialogues = ["WHAT ARE YOU STANDING AROUND FOR? GO KILL that WOLF that barged into MY HOUSE! WITH THE RIFLE I so courageusly gave to you dear-y <3"];
                    }
                    this.grandma.showReactionDialogue();
                }
            }.bind(this)
        };
```

(grandma represents id, sprite, and state change between an elimated enemy wolf and non eliminated enemy wolf also known as winning)

<a id="boo"> </a>

## <font color="violet"> Booleans </font>

Booleans covers boolean logic like flags for stuff like player states (like maybe a cooldown in my case), which for me can be seen in , as seen below:

bullet.js constructor code:
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
```
level3.js const code:
``` js
        const sprite_src_red = path + "/images/gamify/Finalred.png";
        const sprite_data_red = {
            id: 'RedRidingHood',
            greeting: "Red Riding Hood - Press Q to shoot!",
            src: path + "/images/gamify/Finalred.png",
            SCALE_FACTOR: 6,
            STEP_FACTOR: 800,
            ANIMATION_RATE: 50,
            INIT_POSITION: { x: width / 2 - 50, y: height - 100 },
            pixels: { height: 144, width: 192 },
            orientation: { rows: 3, columns: 4 },
            down: { row: 0, start: 0, columns: 3 },
            left: { row: 1, start: 0, columns: 3 },
            right: { row: 2, start: 0, columns: 3 },
            up: { row: 3, start: 0, columns: 3 },
            hitbox: { widthPercentage: 0.45, heightPercentage: 0.2 },
            keypress: { up: 87, left: 65, down: 83, right: 68 },
            shootCooldown: 500
        };
```
shooterplayer.js cooldown code:
``` js
class ShooterPlayer extends Player {
    constructor(data, gameEnv) {
        super(data, gameEnv); // Calls the original Player constructor
        this.bullets = [];
        this.shootCooldown = data.shootCooldown || 500; // milliseconds between shots
        this.lastShotTime = 0;
        this.facing = 'up'; // Default facing direction
    }

    update() {
        super.update(); // Keep the top-down movement logic

        // Update facing direction based on movement
        if (this.velocity.x > 0) this.facing = 'right';
        else if (this.velocity.x < 0) this.facing = 'left';
        else if (this.velocity.y > 0) this.facing = 'down';
        else if (this.velocity.y < 0) this.facing = 'up';

        // Fix diagonal movement animation issue by ensuring direction matches sprite data
        // Map diagonal directions to their base directions for proper animation
        if (this.direction === 'upLeft') this.direction = 'left';
        else if (this.direction === 'upRight') this.direction = 'right';
        else if (this.direction === 'downLeft') this.direction = 'left';
        else if (this.direction === 'downRight') this.direction = 'right';

        // Check for Q key press to shoot
        if (this.pressedKeys[81]) { // Q key
            this.shoot();
        }

        // Update bullets
        this.updateBullets();
    }

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

```

<a id="arr"> </a>

### <font color="red"> Arrays </font>

arrays represent collections of "memory" usong brackets [], which I see here

level3.js grandma dialogue code:
``` js
            interactionRadius: 400, // Interaction area (now half the previous size)
            dialogues: ["WHAT ARE YOU STANDING AROUND FOR? GO KILL that WOLF that barged into MY HOUSE! WITH THE RIFLE I so courageusly gave to you dear-y <3"],
            // Use reaction property to trigger dialogue on collision
            reaction: function() {
                if (!this.grandma) this.grandma = grandmaRef;
                if (this.grandma && this.grandma.showReactionDialogue) {
                    // Update dialogue based on wolf defeat status
                    if (this.enemyDefeated) {
                        this.grandma.dialogueSystem.dialogues = ["Now, that's the girl I partially raised!!!"];
                    } else {
                        this.grandma.dialogueSystem.dialogues = ["WHAT ARE YOU STANDING AROUND FOR? GO KILL that WOLF that barged into MY HOUSE! WITH THE RIFLE I so courageusly gave to you dear-y <3"];
                    }
                    this.grandma.showReactionDialogue();
                }
            }.bind(this)
        };
``` 
shooterplayer.js constructor code:
``` js
class ShooterPlayer extends Player {
    constructor(data, gameEnv) {
        super(data, gameEnv); // Calls the original Player constructor
        this.bullets = [];
        this.shootCooldown = data.shootCooldown || 500; // milliseconds between shots
        this.lastShotTime = 0;
        this.facing = 'up'; // Default facing direction
    }
```
shooterplayer.js update code (this covers something very array esq dictation of the q button):

``` js
   // Check for Q key press to shoot
        if (this.pressedKeys[81]) { // Q key
            this.shoot();
        }
``` 
enpeecee.js showreactiondialogue function code (const):
``` js
 showReactionDialogue() {
        if (!this.dialogueSystem) return;

        const npcName = this.spriteData?.id || "";
        const npcAvatar = this.spriteData?.src || null;

        const dialogue = this.dialogueSystem?.dialogues?.[0] || this.spriteData?.dialogues?.[0] || this.spriteData?.greeting || "Hello!";
        if (this.spriteData?.greeting === false && !this.spriteData?.dialogues?.length) {
            console.log("Greeting set to false and no dialogue entries provided!")
            return;
        }
        this.dialogueSystem.showDialogue(dialogue, npcName, npcAvatar, this.spriteData);
    }    
```

<a id="obj"> </a>

### <font color="Blue"> Object JSON </font>

Object Literals are JavaScript code. Keys do not require quotes (unless they contain special characters or spaces), they can hold functions and methods, and trailing commas are allowed.JSON is a strict data-interchange format (string format). Keys must be wrapped in double quotes, functions are prohibited, and trailing commas are forbidden. (used to organize sprite data, character settings, and level configuration information.)

Classic using {} and making a list of categories and values which can be seen below:

(used first half of level3.js const grandma code)

``` js
const grandmaData = {
            id: 'Grandma',
            src: path + "/images/gamify/lrrh-lvl3-grandma.png",
            SCALE_FACTOR: 6, // Shrink canvas (ie grandma and interaction physical box)
            STEP_FACTOR: 1000,
            ANIMATION_RATE: 50,
            INIT_POSITION: { x: 50, y: 150 },
            pixels: { height: 480, width: 480 },
            orientation: { rows: 1, columns: 1 },
            down: { row: 0, start: 0, columns: 1 },
            interactionRadius: 400, // Interaction area (now half the previous size)
            dialogues: ["WHAT ARE YOU STANDING AROUND FOR? GO KILL that WOLF that barged into MY HOUSE! WITH THE RIFLE I so courageusly gave to you dear-y <3"],
```

<style>
.btn-operators { background-color: rgb(102, 16, 136) !important; color: white !important; }
.btn-operators:hover { background-color: rgb(187, 46, 145) !important; }
</style>

<div class="btn-group">
    <a href="{{site.baseurl}}/finale/operators" class="btn btn-operators">Operators</a>
</div>

<br>