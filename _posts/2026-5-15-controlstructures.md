---
toc: false 
layout: post
title: Control Structures
description: the second section of rubric objectives 
permalink: /finale/controlstructures/
---

### <font color="purple">   Listings </font>

the objectives we will cover in this category are

 <div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/controlstructures-section.png" alt="Image 1">
</div> 

### <font color="orange">   Control Structures </font>

* [Iteration](#it),	Use loops for game object arrays, animation frames	Code review: for, forEach, while loops
* [Conditionals](#con),	Implement collision detection, state transitions	Code review: if/else, nested conditions
* [Nested Conditions](#nest),	Complex game logic (e.g., power-up + collision + direction)	Code review: Multi-level conditionals

<a id="it"> </a>

### <font color="green">  Iteration </font>

Iteration covers loops in the code like animation which can be seen in this character code: 

```    /**
     * Updates the frame index for animation at a slower rate.
     */
    updateAnimationFrame() {
        this.frameCounter++;
        if (this.frameCounter % this.animationRate === 0) {
            const directionData = this.spriteData[this.direction] || {};
            const frames = directionData.columns || this.spriteData.orientation.columns || 1;
            this.frameIndex = (this.frameIndex + 1) % frames;
        }
    } 
```

<a id="con"> </a>

### <font color="blue"> Conditionals </font>

this part of code implements collision detection, often using if/else (while Mr. Mortonsen recomends something else!(a different phrase)) seen in this gameobject code (74-91): 

```    
collisionChecks() {
        let collisionDetected = false;

        for (var gameObj of this.gameEnv.gameObjects) {
            if (gameObj.canvas && this != gameObj) {
                this.isCollision(gameObj);
                if (this.collisionData.hit) {
                    collisionDetected = true;
                    this.handleCollisionEvent();
                }
            }
        }

        // Reset collision events if no collisions detected
        if (!collisionDetected) {
            this.state.collisionEvents = [];
        }
    }
```
<a id="nest"> </a>

### <font color="green"> Nested Conditions </font>

nested conditions are multiple levels of conditionals to produce complex game logic which I believe is best represented in the bullets and what they do, they deal damage, dissapear on contact, dissappear off screen, move have collision, spawn from a q key press, and could use a texture. Their code is this: (bullet.js, update and shoot function)

``` 
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
<style>
.btn-datatypes { background-color: #881010ff !important; color: white !important; }
.btn-datatypes:hover { background-color: #bb2e2eff !important; }
</style>

<div class="btn-group">
    <a href="{{site.baseurl}}/finale/datatypes" class="btn btn-datatypes">Data Types</a>
</div>

<br>