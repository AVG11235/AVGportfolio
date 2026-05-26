---
toc: false 
layout: post
title: Testing & Verification
description: the left in question
permalink: /finale/testingverification/
---

### Testing & Verification

* [Gameplay Testing](#gate)	Test level completion, character interactions, collision detection	Live demo: Play through level without critical bugs
* [Integration Testing](#intes)	Test API integration (Leaderboard, NPC AI) with live backend	Demo: Successful score saving and AI responses
* [API Error Handling](#APIEH)	Try/catch blocks for API calls, network error handling	Code review: Error handling for fetch failures

<a id="gate"> </a>

## <font color="green"> Gameplay Testing </font>

Yeah I can show you my 2 levels, they work well, they'll be another link to the game at the end of this last chapter

<a id="intes"> </a>

## <font color="purple"> Integration Testing </font>

an AI npc and leaderboard was done by Rashi but it doesn't work too well on my computer from tests
this just covers how well everything in the works all together

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/lead.png" alt="Image 37">
  <img src="{{site.baseurl}}/images/final-images/AI.png" alt="Image 38">

</div>


<a id="APIEH"> </a>

## <font color="gold"> API Error Handling </font>

this covers, the systematic process of identifying, intercepting, and resolving failures that occur when making API requests. It ensures system reliability and provides users with actionable, clear feedback when things go wrong. So if the link connected to the doesn't exist you get a 404 error like if you mistyped it, well actully no it focuses in things the game asks for, not just the link working like score

so the code gives a result, a failure result to tell you it's not working so you can better know what to solve


this appears in the file(s): leaderboard.js

``` js
        console.log('Payload:', JSON.stringify(requestBody));

        // POST to backend using API chaining pattern
        return fetch(
            url,
            {
                ...fetchOptions,
                method: 'POST',
                body: JSON.stringify(requestBody)
            }
        )
            .then(res => {
                if (!res.ok) {
                    return res.text().then(errorText => {
                        console.error('Server error:', errorText);
                        throw new Error(`Failed to save score: ${res.status} - ${errorText}`);
                    });
                }
                return res.json();
            })
            .then(savedEntry => {
                console.log('Score saved successfully to SCORE_COUNTER:', savedEntry);

                // Refresh leaderboard if we're in dynamic mode
                if (this.mode === 'dynamic') {
                    return this.fetchLeaderboard().then(() => savedEntry);
                }

                return savedEntry;
            });
    }
```

<br>

<div class="btn-group">
    <a href="https://teamram.opencodingsociety.com/gamify/redridinghood" class="btn btn-red">Red Riding Hood</a>
    <a href="https://rashig-1804.opencodingsociety.com/red-riding" class="btn btn-panel">Gamerunner Style Version</a>
</div>

<br>

>If you want to know the way I solved API errors, what happens is I work and update in bits so when I realise the page doesn't work, I go back to a previous bit of work and try to rewrite it to have the game work again, and I repeat this if it still doesn't work or move on if it does work. 

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/typeerror.png" alt="Image 33">
</div>


Ainpc.js code error handling

``` js

    /**
     * Test backend API availability
     * @returns {Promise<boolean>} True if API is available
     */
    static async testAPI() {
        try {
            const response = await fetch(pythonURI + '/api/ainpc/test', {
                ...fetchOptions,
                method: 'GET'
            });
            const data = await response.json();
            return data.status === 'ok';
        } catch (err) {
            console.error('AI NPC API test failed:', err);
            return false;
        }
    }
```

``` js
                    try {
                        const pixels = this.spriteData.pixels;
                        const orientation = this.spriteData.orientation;
                        const frameW = Math.max(1, Math.round(pixels.width / orientation.columns));
                        const frameH = Math.max(1, Math.round(pixels.height / orientation.rows));
                        console.log("[Character] Sprite loaded:", this.spriteSheet.src,
                                    "natural:", this.spriteSheet.naturalWidth + 'x' + this.spriteSheet.naturalHeight,
                                    "pixels:", pixels.width + 'x' + pixels.height,
                                    "orientation:", orientation.columns + 'x' + orientation.rows,
                                    "frame:", frameW + 'x' + frameH);

                        const dirs = ['up','right','down','left','upRight','upLeft','downRight','downLeft'];
                        dirs.forEach(d => {
                            const dd = this.spriteData[d];
                            if (!dd) return;
                            const row = dd.row || 0;
                            const cols = dd.columns || orientation.columns || 1;
                            console.log(`[Character] direction ${d}: row=${row}, columns=${cols}`);
                        });
                    } catch (logErr) {
                        console.warn('Character sprite diagnostics failed', logErr);
                    }
                } catch (err) {
                    console.warn('Error during sprite onload processing', err);
                }
```
try

Runs risky code:

fetch requests
JSON parsing
API communication
catch

Runs if something fails.

Example failures:

no internet
server offline
invalid JSON
CORS errors

Why response.ok Matters

Status codes like:

404
500
403

do NOT automatically trigger catch.

Even if fetch succeeds, the server may still fail.



## Super notes (temp.)

Element inspection was performed using Chrome DevTools’ Elements tab to inspect canvas objects, UI components, and CSS styles during gameplay. This allowed debugging of positioning, scaling, visibility, layering, and dynamically generated game elements by examining DOM structure and live style properties.

> super is used in a child class to access things from the parent class.

``` js
constructor(data = null, gameEnv = null) {
    super(data, gameEnv);
}
```

> ++ means increase the value by 1.

> forEach is a method used to go through every item in an array one at a time.

> catch is used for handling errors in code. It works with try. Basic structure:
``` js
try {
    // code that might fail
} catch (error) {
    // code that runs if there is an error
}
``` 

That's all!!!!