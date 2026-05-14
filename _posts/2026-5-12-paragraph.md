---
toc: false 
layout: post
title: Final Details
description: the proof I deserve college credit and also to pass this class
permalink: /finale/
---

### <font color="violet"> Row of contents </font>

Fun thing to note most of the rubric rules require you too review the code of the game, I believe most if not all code requirements have been met so to make it easier for you to read them and for me to double check I'm gonna list up each code requirement and connect it to a section of my game's code with a neat little description as well, so here's a table of links across this page that make it easy to go to any wanted rubric requirement instantaneously:

Object-Oriented Programming	

* [Writing Classes](#writer),	Create minimum 2 custom character classes extending base classes	Code review: Player.js, NPC.js, Enemy.js, 
* [Methods & Parameters](#meth),	Implement methods with parameters (e.g., collisionHandler(other, direction))	Code review: Method signatures with 2+ parameters
Instantiation & Objects,	Instantiate game objects in GameLevel configuration	Code review: GameLevel setup objects
Inheritance (Basic),	Create class hierarchy with 2+ levels (e.g., GameObject → Character → Player)	Code review: extends keyword, inheritance chain
Method Overriding,	Override parent methods (update(), draw(), handleCollision())	Code review: Polymorphic implementations
Constructor Chaining,	Use super() to chain constructors	Code review: super(data, gameEnv) calls

Control Structures

Iteration,	Use loops for game object arrays, animation frames	Code review: for, forEach, while loops
Conditionals,	Implement collision detection, state transitions	Code review: if/else, nested conditions
Nested Conditions,	Complex game logic (e.g., power-up + collision + direction)	Code review: Multi-level conditionals

Data Types

Numbers,	Position, velocity, score tracking	Code review: Numeric properties
Strings,	Character names, sprite paths, game states	Code review: String manipulation
Booleans,	Flags (isJumping, isPaused, isVulnerable)	Code review: Boolean logic
Arrays, 	Game object collections, level data	Code review: Array operations
Objects (JSON), 	Configuration objects, sprite data	Code review: Object literals

Operators

Mathematical,	Physics calculations (gravity, velocity, collision)	Code review: +, -, *, / in physics
String Operations,	Path concatenation, text display	Code review: Template literals, concatenation
Boolean Expressions,	Compound conditions in game logic	Code review: &&, ||, !

Input/Output

Keyboard Input	Arrow keys, space, WASD controls using event listeners	Testing: Key event handlers respond correctly
Canvas Rendering	Draw sprites, backgrounds, platforms using Canvas API	Code review: draw() method implementations
GameEnv Configuration	Set canvas size, difficulty levels, game settings	Code review: GameEnv.create() and GameSetup.js
API Integration	Implement Leaderboard API (POST/GET scores)	Code review: Fetch calls with error handling
Asynchronous I/O	Use async/await or promises for API calls	Code review: async/await or .then() chains
JSON Parsing	Parse API responses (leaderboard data, AI responses)	Code review: JSON.parse(), object destructuring

Documentation	 	 

Code Comments,	JSDoc comments for classes and methods	Code review: Comment density >10%
Mini-Lesson Documentation,	Create comic/visual post with embedded runtime game demo	Portfolio review: Mini-lesson in personal portfolio
Code Highlights,	Annotate key code snippets in documentation (OOP, APIs, collision)	Portfolio review: Highlighted code examples with explanations

Debugging

Console Debugging,	Use console.log to track game state, variables, method calls	Code review: Strategic logging in update/collision methods
Hit Box Visualization,	Draw/visualize collision boundaries to refine detection	Demo: Toggle hit box display, adjust collision rectangles
Source-Level Debugging,	Set breakpoints in DevTools, step through code execution	Demo: Use Sources tab to pause and inspect code flow
Network Debugging,	Examine Network tab for API calls, CORS errors, response status	Demo: Inspect fetch requests, response data, error messages
Application Debugging,	Examine cookies, localStorage, session data for login/state	Demo: Application tab inspection of stored data
Element Inspection,	Use Element Viewer to inspect canvas, DOM elements, styles	Demo: Inspect element properties and game object state

Testing & Verification

Gameplay Testing	Test level completion, character interactions, collision detection	Live demo: Play through level without critical bugs
Integration Testing	Test API integration (Leaderboard, NPC AI) with live backend	Demo: Successful score saving and AI responses
API Error Handling	Try/catch blocks for API calls, network error handling	Code review: Error handling for fetch failures


<a id="writer"></a>

## Writing Classes

Writing classes means litterally just making a class, the was litterally seen here, *bullet.js* and here **, and in many more places, I'd be suprised if you made a game with 1 or no classes at all failing this objective spectacularly! 


<a id="meth"></a>

## Methods & Parameters