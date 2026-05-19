---
toc: false 
layout: post
title: Input/Output
description: 3 left after this 
permalink: /finale/inputoutput/
---

### Input/Output

* [Keyboard Input](#key)	Arrow keys, space, WASD controls using event listeners	Testing: Key event handlers respond correctly
* [Canvas Rendering](#can)	Draw sprites, backgrounds, platforms using Canvas API	Code review: draw() method implementations
* [GameEnv Configuration](#gam)	Set canvas size, difficulty levels, game settings	Code review: GameEnv.create() and GameSetup.js
* [API Integration](#api)	Implement Leaderboard API (POST/GET scores)	Code review: Fetch calls with error handling
* [Asynchronous I/O](#asio)	Use async/await or promises for API calls	Code review: async/await or .then() chains
* [JSON Parsing](#jsonp)	Parse API responses (leaderboard data, AI responses)	Code review: JSON.parse(), object destructuring

<a id="key"> </a>

## Keyboard Input

Something simple the coverage of key inputs in code connecting mechanics in game like moving with WASD, and using event listeners to defines keys like-
> Removes ability to click on grandma after level is beaten by destroying the event listener
```
    destroy() {
        // GameLevel system handles destroying background, player, and enemy
        if (this.handleGrandmaClickBound) {
            window.removeEventListener('mousedown', this.handleGrandmaClickBound);
        }
    }
```
> Adding event listener
```
        this.grandma = new Npc(grandmaData, gameEnv);
        const grandmaRef = this.grandma;

        this.grandmaClickCount = 0;
        this.handleGrandmaClickBound = this.handleGrandmaClick.bind(this);
        window.addEventListener('mousedown', this.handleGrandmaClickBound);
```
> WASD usage appearence
```
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
```
> PART 2
```
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
```

<a id="can"> </a>

## Canvas Rendering

Canvas Rendering is about the update cycle updating animation frames and cycles and what happens in code to change what happens on screen, more specifically,