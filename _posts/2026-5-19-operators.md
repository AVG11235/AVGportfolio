---
toc: false 
layout: post
title: Operators
description: 4 left after this 
permalink: /finale/operators/
---

### Operators

* [Mathematical](#mathes),	Physics calculations (gravity, velocity, collision)	Code review: +, -, *, / in physics
* [String Operations](#stop),	Path concatenation, text display	Code review: Template literals, concatenation
* [Boolean Expressions](#booex),	

```
Compound conditions in game logic	Code review: &&, ||, !
```

<a id="mathes"> </a>

## <font color="magenta"> Mathematical </font>

the first section of the Operators subject in code is "Mathematical", it covers velocity, collisions, and gravity we've seen gravity used for a jumping mechanic in level 1 of the little red riding hood game and the other two mechanics are used in my levels, these values use these symbols: + - * / and now of it is this: 

level3.js: update: const wolfbox (there are more examples but I wanted this to be quick and I have already covered velocity data)

```
       const wolfBox = {
            x: enemy.x + enemy.width * 0.15,  // Offset by 15% on each side (shrink by 30% total)
            y: enemy.y + enemy.height * 0.15, // Offset by 15% on top and bottom (shrink by 30% total)
            width: enemy.width * 0.7,          // Use only 70% of width
            height: enemy.height * 0.7         // Use only 70% of height
        };
```

<a id="stop"> </a>

## <font color="indigo"> String Operations </font>

main representations of string operations are path concatenation which is connecting files with links, like a certain file in certain folders leading to addresses and stuff, or more accurately the process of combining directory names and a file name to create a complete file system path, and texts displays which I do have in the grandma npc, and the level victory popup. String Operations are written like so (${)

const path
```
 const enemyData = {
            id: 'Wolf',
            greeting: "The Wolf!",
            src: path + "/images/gamify/ridinghood/wolfff.png",
            SCALE_FACTOR: wolfScale,
            STEP_FACTOR: 1000,
            ANIMATION_RATE: 50,
            // Positioned in the bottom-left corner
            INIT_POSITION: { x: 150, y: 400 }, 
            pixels: wolfPixels,
            orientation: { rows: 1, columns: 1 },
            down: { row: 0, start: 0, columns: 1 },
            // Smaller hitbox than the sprite (85% of actual size)
            collisionWidth: wolfPixels.width * wolfScale * 0.85,
            collisionHeight: wolfPixels.height * wolfScale * 0.85,
            hitbox: { widthPercentage: 0.85, heightPercentage: 0.85 },
            hp: 5 // Give the wolf some health points to make the fight last a bit longer
        };

```
link imports:
```
// level3.js - Red Riding Hood Level 3: The Confrontation
import GameEnvBackground from '../essentials/GameEnvBackground.js';
import ShooterPlayer from './ShooterPlayer.js';
import Enemy from './Enemy.js';
import HitMarker from './HitMarker.js';
import Explosion from './Explosion.js';
import Npc from './enpeecee.js';
```
message:
```
   showGrandmaVictory() {
        const message = document.createElement('div');
        message.id = 'victory-popup';
        message.style.position = 'absolute';
        message.style.top = '40%';
        message.style.left = '50%';
        message.style.transform = 'translate(-50%, -50%)';
        message.style.background = 'rgba(255, 225, 159, 0.95)';
        message.style.border = '4px solid #b00';
        message.style.padding = '32px';
        message.style.borderRadius = '16px';
        message.style.fontSize = '1.5em';
        message.style.textAlign = 'center';
        message.style.zIndex = 1000;
        message.style.boxShadow = '0 10px 25px rgba(0,0,0,0.3)';
        
        message.innerHTML = `
            <h2 style="color:white; margin-top:0;">Victory!</h2>
            <p style="color:#b00 !important;">Good job my girl! These old wolfies have gone rampant this season. Now you said you have some cookies?<br><br></p>
            <button onclick="location.reload()" style="padding:12px 24px; font-size:18px; cursor:pointer; background:#b00; color:white; border:none; border-radius:8px; font-weight:bold;">Play Again</button>
        `;
        document.body.appendChild(message);
    }
```
W template for string operations
```
// 1. Path Concatenation (Old way)
const folder = "users";
const file = "photo.png";
const path = "/" + folder + "/" + file; 

// 2. Template Literals (Modern/Review choice)
// Use backticks (`) and ${} for cleaner pathing and text display
const templatePath = `/${folder}/${file}`;

// 3. Text Display (Multi-line)
const display = `
  <div>
    <h1>File: ${file}</h1>
    <p>Path: ${templatePath}</p>
  </div>
`;

console.log(templatePath); /
```


<a id="booex"> </a>

## <font color="red"> Boolean Expansions </font>

Boolean expansions are compounded expansions of game logic using && || ! 
examples of them in my game look like 

handleGrandmaClick funtion:
```
    handleGrandmaClick(event) {
        if (!this.grandma || !this.gameEnv.canvas) return; // look here

        const rect = this.gameEnv.canvas.getBoundingClientRect();
        const clickX = event.clientX - rect.left;
        const clickY = event.clientY - rect.top;

        // Check if click is directly on grandma sprite
        if (clickX >= this.grandma.position.x && clickX <= this.grandma.position.x + this.grandma.width && // look here
            clickY >= this.grandma.position.y && clickY <= this.grandma.position.y + this.grandma.height) {
            if (this.grandma.dialogueSystem) {
                this.grandma.dialogueSystem.dialogues = ["this level doesn't use a mouse deary, my life is currently in danger due to that cloud with bones"];
                this.grandma.showReactionDialogue();
            }
            return;
        }

        // Check if click is within interaction radius for 1-click counter
        const centerX = this.grandma.position.x + this.grandma.width / 2;
        const centerY = this.grandma.position.y + this.grandma.height / 2;
        const radius = this.grandma.spriteData?.interactionRadius || 100;

        if (Math.hypot(clickX - centerX, clickY - centerY) > radius) {
            return;
        }

        this.grandmaClickCount += 1;
        if (this.grandmaClickCount >= 1) {
            this.grandmaClickCount = 0;
            if (this.grandma.dialogueSystem) {
                this.grandma.dialogueSystem.dialogues = ["Deary. HURRY! Q to shoot WASD to move top down esq, figure out the rest #combos"];
                this.grandma.showReactionDialogue();
            }
        }
    }
```
<style>
.btn-inputoutput { background-color: rgba(122, 221, 191, 1) !important; color: white !important; }
.btn-inputoutput:hover { background-color: rgba(0, 255, 179, 1) !important; }
</style>

<div class="btn-group">
    <a href="{{site.baseurl}}/finale/inputoutput" class="btn btn-inputoutput">Input/Output</a>
</div>

<br>