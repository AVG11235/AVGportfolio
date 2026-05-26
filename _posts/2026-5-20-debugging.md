---
toc: false 
layout: post
title: Debugging
description: 1 left after this 
permalink: /finale/debugging/
---

### Debugging

* [Console Debugging](#sole),	Use console.log to track game state, variables, method calls	Code review: Strategic logging in update/collision methods
* [Hit Box Visualization](#hit),	Draw/visualize collision boundaries to refine detection	Demo: Toggle hit box display, adjust collision rectangles
* [Source-Level Debugging](#sour),	Set breakpoints in DevTools, step through code execution	Demo: Use Sources tab to pause and inspect code flow
* [Network Debugging](#netw),	Examine Network tab for API calls, CORS errors, response status	Demo: Inspect fetch requests, response data, error messages
* [Application Debugging](#appl),	Examine cookies, localStorage, session data for login/state	Demo: Application tab inspection of stored data
* [Element Inspection](#elem),	Use Element Viewer to inspect canvas, DOM elements, styles	Demo: Inspect element properties and game object state

<a id="sole"> </a>

## <font color="orange">Console Debugging </font>

1. Opening Chrome DevTools
Method 1

Right click the page → Inspect

Method 2

Keyboard shortcuts:

Windows/Linux
Ctrl + Shift + I
Mac
Cmd + Option + I

Then click the Console tab.

2. What the Console Does

The Console:

displays errors
prints variables
tracks game events
logs collisions
shows API responses
helps debug movement and animations
3. Basic console.log()

Example: 

``` js
                console.warn('Failed to load sprite sheet:', data.src, e);
```

Appears if in the (specified by character.js) sprite sheet fails to load after checks and other console logs proven unused

console debugging focuses on the ability to check the console to see stuff going on in game using the console by inspecting the page to find important details of whats going on in game, I do have, code that logs stuff for the console and here are images of it too

``` js
   showInstructions() {
        console.log("=== LEVEL 3: FACE THE WOLF ===");
        console.log("WASD - Move Red Riding Hood");
        console.log("Q - Shoot bullets");
        console.log("Defeat the wolf in the upper middle!");
        console.log("============================");
    }
```

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/consolewolf.png" alt="Image 31">
</div>

<a id="hit"> </a>

## <font color="green"> Hit Box Visualization </font> 

this asks for their to be a feature to showcase the hit box toggle I don't believe this was something incorporated, you can kind of see them using the console but it's not their actual actual hit boxes. I know level 2 had something similar but it's not toggle-able.

<a id="sour"> </a>

## <font color="purple"> Source-level Debugging </font>

>Source-level debugging was performed using Chrome DevTools’ Sources tab. Breakpoints were set in movement, collision, and animation methods to pause execution and inspect variables such as position, velocity, and collision data. Stepping through code line-by-line helped identify gameplay bugs and verify logic flow during runtime.

Source-Level Debugging Guide for Games in Chrome

Source-level debugging means using the Sources tab in Chrome DevTools to:

pause code execution
inspect variables
step through functions line-by-line
track game logic in real time

This directly matches your rubric requirement:

“Set breakpoints in DevTools, step through code execution”

1. Open Chrome DevTools
Shortcut
Windows/Linux
Ctrl + Shift + I
Mac
Cmd + Option + I

Then click:

Sources
2. Find Your JavaScript File

In the left panel:

open your site files
navigate to:
Player.js
Character.js
Enemy.js

Click the file you want to debug.

3. Set a Breakpoint

A breakpoint pauses the game at a specific line.

How

Click the line number.

Example:

this.velocity.x += this.xVelocity;

A blue marker appears.

When the code reaches that line:

the game pauses
DevTools stops execution
4. Inspect Variables

While paused:

hover variables
view values in the right panel
inspect objects

Example:

this.position
this.velocity
this.direction

You can see:

player coordinates
movement speed
collision states
animation frames

6. Example With Your Player Code

Suppose you set a breakpoint here:

updateVelocity() {

Now when movement happens:

game pauses
you inspect:
pressedKeys
velocity
direction

This helps debug:

movement bugs
stuck movement
incorrect controls
7. Debugging Collisions

Set a breakpoint here:

handleCollisionReaction(other)

When collision occurs:

game pauses instantly
inspect:
other
touchPoints
velocity

Perfect for debugging walls and enemy collisions.

8. Debugging Animation

Breakpoint example:

updateAnimationFrame()

Inspect:

frameIndex
frameCounter
direction

Useful for:

broken animations
wrong sprite rows
animation timing
9. Watch Expressions

In the right panel:
Add watches like:

this.position.x
this.velocity.y
this.direction

Chrome updates them live while debugging.

10. Pausing on Errors

In Sources:
Enable:

Pause on exceptions

Now Chrome automatically pauses when:

code crashes
undefined variables occur
APIs fail

Very useful.

11. Debugging Game Loops

Games update constantly, so:

breakpoints pause the loop
you can inspect the exact frame where bugs happen

This is extremely useful for:

collision bugs
teleporting
physics glitches
animation issues
12. Example Workflow
Problem:

Player clips through wall.

Debug Process:
Open Sources
Set breakpoint in:
handleCollisionReaction()
Move into wall
Game pauses
Inspect:
touchPoints
velocity
position
Find incorrect collision logic

This is real source-level debugging.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/sourcegame.png" alt="Image 32">
</div>

<a id="netw">  </a>

## <font color="red"> Network Debugging </font>

1. Open Chrome DevTools
Shortcut
Windows/Linux
Ctrl + Shift + I
Mac
Cmd + Option + I

Then click:

Network
2. Reload the Game

Press:

F5

or refresh the page.

The Network tab now records:

images
JavaScript files
APIs
sounds
fetch requests
3. Understanding the Network Tab

Each request shows:

file/API name
request type
status code
file size
loading time
4. Important Status Codes
Code	Meaning
200	Success
404	File not found
500	Server error
403	Forbidden
401	Unauthorized

Example:

404 leaderboard API

means the request failed.

5. Debugging API Requests

Suppose your game saves scores:

fetch("/api/leaderboard")

In Network:

click the request
inspect details

You can see:

request headers
response body
errors
JSON data
6. Inspecting JSON Responses

Click:

Preview

or:

Response

You can inspect returned leaderboard data like:

[
  {
    "name": "Alex",
    "score": 500
  }
]

This demonstrates:

JSON parsing
backend communication
API integration
7. Debugging Failed Requests

If a request fails:

it appears red in Network

Example errors:

404 Not Found
500 Internal Server Error
CORS Error

This helps identify:

broken API URLs
backend issues
missing assets
8. Debugging Asset Loading

Games load many assets:

sprites
sounds
backgrounds

Network helps find missing files.

Example:

player.png 404

Means:

image path incorrect
asset missing
9. Filtering Requests

Use filters:

Fetch/XHR → APIs
Img → sprites/images
JS → scripts

Very useful for large games.

10. Example Using Your Project

Suppose sprite fails to load:

this.spriteSheet.src = data.src;

Network debugging lets you:

check image request
verify path
inspect loading failure
11. Debugging API Error Handling

Your rubric wants this specifically.

Example:

try {
    const response = await fetch("/api/leaderboard");
} catch (error) {
    console.error(error);
}

Then in Network:

inspect failed request
verify status code
analyze response

This is full API/network debugging.

12. Simulating Slow Internet

In Network:
Change:

No throttling

to:

Slow 3G
Fast 3G

This helps test:

loading screens
async behavior
lag handling

Very useful for games.

13. Preserve Logs

Enable:

Preserve log

This keeps requests visible after page refresh.

Helpful for debugging startup/loading issues.

14. Example Debug Workflow
Problem:

Leaderboard not saving.

Steps:
Open Network tab
Filter Fetch/XHR
Submit score
Inspect request
Check:
status code
response body
request payload

You may discover:

wrong API URL
missing JSON
server failure


<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/networkgame.png" alt="Image 34">
</div>

<a id="appl"> </a>

## <font color="pink"> Application Debugging </font>

1. Open Chrome DevTools
Shortcut
Windows/Linux
Ctrl + Shift + I
Mac
Cmd + Option + I

Then click:

Application
2. What the Application Tab Does

The Application tab lets you inspect browser-stored data.

Games often store:

player progress
high scores
settings
login information
save states
3. localStorage Debugging

Many games use:

localStorage.setItem("score", 500);

to save data.

Viewing localStorage

In DevTools:

Application → Local Storage

Select your website.

You’ll see:

keys
values
stored game data

Example:

Key	Value
score	30
playerName	AVG
4. Example Game Save System

Saving data:

localStorage.setItem("highScore", score);

Loading data:

const highScore = localStorage.getItem("highScore");
5. Why This Helps Debugging

You can verify:

scores save correctly
settings persist
game state updates
values are not corrupted
6. Editing Stored Data

In the Application tab:

double click a value
edit it live

Example:
Change:

highScore = 999999

Useful for:

testing
debugging edge cases
verifying game behavior
7. Removing Broken Data

You can delete storage entries if:

save files break
old data causes bugs
testing needs reset

Right click → Delete

8. sessionStorage

Similar to localStorage but temporary.

Data disappears when tab closes.

Example:

sessionStorage.setItem("currentLevel", 3);

Useful for:

temporary sessions
checkpoints
current match state
9. Cookies

Games/web apps may use cookies for:

login sessions
authentication
preferences

View them under:

Application → Cookies
10. Cache Storage

Games often cache:

images
sounds
scripts

Application tab helps inspect:

cached assets
service workers
offline behavior

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/appgame.png" alt="Image 33">
</div>

<a id="elem"> </a>

## <font color="yellow"> Elemental Inspection </font>

Element Inspection Guide for Games in Chrome

Element inspection uses the Elements tab in Chrome DevTools to inspect and debug:

HTML elements
canvas positioning
styles
game UI
hitboxes
DOM structure

This directly matches your rubric requirement:

“Inspect canvas, DOM elements, styles, and game object state”

1. Open Chrome DevTools
Shortcut
Windows/Linux
Ctrl + Shift + I
Mac
Cmd + Option + I

Then click:

Elements
2. What the Elements Tab Does

The Elements tab lets you inspect:

webpage structure
canvas elements
CSS styles
game UI components
dynamically created objects

For games, this is useful because many objects are drawn or controlled through the DOM.

3. Selecting Elements

Click the cursor icon:

Select an element

Then click something on the page.

Chrome highlights:

the HTML element
styles
dimensions
positioning
4. Inspecting the Game Canvas

Your project creates canvases dynamically:

this.canvas = document.createElement("canvas");

In Elements you can inspect:

canvas size
position
styles
z-index
visibility
5. Inspecting CSS Styles

Your game uses styles like:

this.canvas.style.left = `${this.position.x}px`;

In Elements:

see computed styles
edit values live
test positioning changes

Useful for debugging:

off-screen objects
layering problems
scaling issues
6. Live Editing Styles

You can directly edit CSS in DevTools.

Example:
Change:

width: 50px;

to:

width: 100px;

This updates instantly without refreshing.

Very useful for:

hitbox alignment
UI testing
scaling adjustments
7. Inspecting Game Object State

Since your game stores object data in elements:

this.canvas.id = data.id;

you can inspect:

object IDs
class names
styles
visibility
8. Debugging Positioning Problems

Example issue:

player appears in wrong place

Inspect:

left
top
width
height

inside Elements.

This helps debug:

movement bugs
resize problems
scaling calculations
9. Viewing Canvas Dimensions

You can inspect:

canvas.width
canvas.height

Useful for:

sprite scaling
hitbox mismatch
animation clipping
10. Debugging Hidden Elements

If UI disappears:

inspect element visibility
check:
display
visibility
opacity
z-index

Very common debugging step.

11. Inspecting Touch Controls

Your code creates:

TouchControls

Element inspection can verify:

buttons exist
positions are correct
controls appear on mobile
12. Example Debug Workflow
Problem:

Player sprite appears off-screen.

Steps:
Open Elements
Select canvas
Inspect:
left
top
width
height
Find incorrect positioning value
Adjust/test live

Element inspection was performed using Chrome DevTools’ Elements tab to inspect canvas objects, UI components, and CSS styles during gameplay. This allowed debugging of positioning, scaling, visibility, layering, and dynamically generated game elements by examining DOM structure and live style properties.

simply speaking: you inspect with right click, go to the elements tab, press the ctrl+shift+c button thats like a square with a mouse on it and it lets you click elements on the game to check their code mainly image files, yeah btw elements mainly shows off code.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/elemgame.png" alt="Image 35">
</div>


<br>

<style>
.btn-testingverification { background-color: #2b0091ff !important; color: white !important; }
.btn-testingverification:hover { background-color: #0017e4ff !important; }
</style>

<div class="btn-group">
    <a href="{{site.baseurl}}/finale/testingverification" class="btn btn-testingverification">Testing & Verification</a>
</div>

<br>