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

``` js
       const wolfBox = {
            x: enemy.x + enemy.width * 0.15,  // Offset by 15% on each side (shrink by 30% total)
            y: enemy.y + enemy.height * 0.15, // Offset by 15% on top and bottom (shrink by 30% total)
            width: enemy.width * 0.7,          // Use only 70% of width
            height: enemy.height * 0.7         // Use only 70% of height
        };
```

+ is addition, * is multiplication, combines object properties and math.

<a id="stop"> </a>

## <font color="indigo"> String Operations </font>

main representations of string operations are path concatenation which is connecting files with links, like a certain file in certain folders leading to addresses and stuff, or more accurately the process of combining directory names and a file name to create a complete file system path, and texts displays which I do have in the grandma npc, and the level victory popup. String Operations are written like so (${)

const path
``` js
            src: path + "/images/gamify/ridinghood/wolfff.png",
```

>well because we use + to combine strings together which is string concatenation, doing:

If:
``` js
path = "/game"
```
Then:
``` js
path + "/images/gamify/ridinghood/wolfff.png"
```
becomes:
``` js
/game/images/gamify/ridinghood/wolfff.png
```
This dynamically builds the image file path.

Template Literal Example

>This is ALSO excellent:

``` js
message.innerHTML = `
```
This uses:

template literals

with:
``` js
`
```
(backticks)

Why Template Literals Matter

They allow:

multi-line strings
embedded variables
cleaner HTML generation
Your UI Message Example
``` js
<h2 style="color:white; margin-top:0;">Victory!</h2>
```
This dynamically creates:

UI text
HTML content
gameplay messages

Very strong real-world string usage.

<a id="booex"> </a>

## <font color="red"> Boolean expressions </font>

Boolean expressions are compounded expressions of game logic using && || ! 
examples of them in my game look like 

handleGrandmaClick funtion:
``` js
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

Boolean expressions are:

conditions that evaluate to either true or false

<style>
.btn-inputoutput { background-color: rgba(122, 221, 191, 1) !important; color: white !important; }
.btn-inputoutput:hover { background-color: rgba(0, 255, 179, 1) !important; }
</style>

<div class="btn-group">
    <a href="{{site.baseurl}}/finale/inputoutput" class="btn btn-inputoutput">Input/Output</a>
</div>

<br>