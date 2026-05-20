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

## <font color="red"> Keyboard Input </font>

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

## <font color="blue"> Canvas Rendering </font>

Canvas Rendering is about the update cycle updating animation frames and cycles and what happens in code to change what happens on screen, more specifically, just things involving the draw() function:
(chose to not use a sprite as I prefer the look of the yellow cube for a bullet as it doesn't to be different for each possible direction the bullet could go)
```
    draw() {
        if (this.destroyed) return;
        
        // Always show yellow cube
        this.gameEnv.ctx.fillStyle = 'yellow';
        this.gameEnv.ctx.fillRect(this.x, this.y, this.width, this.height);
    }
```

<a id="gam"> </a>

## <font color="yellow"> GameEnv Configuration </font>

This section represents customization in game like the screen, controls, difficulties etc. which can be seen here as :

```
import GameEnv from "./GameEnv.js"

class GameLevel {

  constructor(gameControl) {
    this.gameEnv = new GameEnv()
    this.gameEnv.game = gameControl.game
    this.gameEnv.path = gameControl.path
    this.gameEnv.gameContainer = gameControl.gameContainer
    this.gameEnv.gameControl = gameControl
  }

  create(GameLevelClass) {
    this.continue = true
    this.gameEnv.create()
    this.gameLevel = new GameLevelClass(this.gameEnv)
    this.gameObjectClasses = this.gameLevel.classes

    // Set current level instance in Game
    if (typeof Game !== 'undefined' && Game.setCurrentLevelInstance) {
        Game.setCurrentLevelInstance(this.gameLevel);
    }

    for (let gameObjectClass of this.gameObjectClasses) {
        if (!gameObjectClass.data) gameObjectClass.data = {}
        let gameObject = new gameObjectClass.class(gameObjectClass.data, this.gameEnv)
        this.gameEnv.gameObjects.push(gameObject)
    }

    if (typeof this.gameLevel.initialize === "function") {
        this.gameLevel.initialize()
    }

    window.addEventListener("resize", this.resize.bind(this))
  }
```

<a id="api"> </a>

## <font color="yellow"> API integration </font>

basically focuses on the leaderboard system and scores seen in level 1 (and 4) using POST/GET:

```
 * BACKEND INTEGRATION:
 * - Endpoint: /api/events/ELEMENTARY_LEADERBOARD (GET/POST/DELETE elementary scores)
 * - Endpoint: /api/events/SCORE_COUNTER (GET/POST dynamic scores)
 * - Uses shared javaURI and fetchOptions from /assets/js/api/config.js
 * - JWT authentication via cookies (handled by fetchOptions)
 * - Graceful fallback to localStorage when backend unavailable
 * 
 * API CHAINING PATTERN:
 * All backend methods use .then()/.catch() chaining for:
 * - Elegant sequential operations (fetch → transform → display)
 * - Centralized error handling with single .catch() block
 * - Clean promise composition without nested try-catch blocks
 * - Authentication error detection (401/403) with user-friendly messages
 * 
 * DATA FLOW (Dynamic Mode):
 * User clicks "Dynamic Leaderboard" → setupDynamicMode()
 *                                   ↓
 * fetchLeaderboard() → fetch(SCORE_COUNTER) → .then(transform) → displayLeaderboard()
 *                   ↓
 * Auto-refresh every 30 seconds
 * 
 * DATA FLOW (Elementary Mode):
 * User clicks "Elementary Leaderboard" → setupElementaryMode()
 *                                      ↓
 * fetchElementaryLeaderboard() → fetch(ELEMENTARY_LEADERBOARD) → .then(transform) → displayElementaryLeaderboard()
 *                              ↓
 * User adds score → addElementaryScore() → fetch(POST) → .then(refresh) → .catch(error)
 *                              ↓
 * User deletes score → deleteElementaryScore(id) → fetch(DELETE) → .then(refresh) → .catch(error)
 * 
```
```
 showElementaryForm() {
        const list = document.getElementById('leaderboard-list');
        if (!list) return;

        // Check if we have existing entries to display
        if (this.elementaryEntries.length > 0) {
            // Show the full leaderboard with "Add Another Score" button
            this.displayElementaryLeaderboard();
        } else {
            // Show the form for first entry
            list.innerHTML = `
                <div class="elementary-form">
                    <div class="form-group">
                        <label for="player-name">Player Name</label>
                        <input type="text" id="player-name" placeholder="Enter name" />
                    </div>
                    <div class="form-group">
                        <label for="player-score">Score</label>
                        <input type="number" id="player-score" placeholder="Enter score" />
                    </div>
                    <button class="submit-btn" id="add-score-btn">Add Score</button>
                </div>
            `;

            // Bind event listeners after innerHTML is set
            const addScoreBtn = document.getElementById('add-score-btn');
            const scoreInput = document.getElementById('player-score');
            
            if (addScoreBtn) {
                addScoreBtn.addEventListener('click', () => {
                    this.addElementaryScore();
                });
            }

            // Allow Enter key to submit
            if (scoreInput) {
                scoreInput.addEventListener('keypress', (e) => {
                    if (e.key === 'Enter') this.addElementaryScore();
                });
            }
        }
    }

    addElementaryScore() {
        console.log('=== ADD ELEMENTARY SCORE ===');
        const nameInput = document.getElementById('player-name');
        const scoreInput = document.getElementById('player-score');

        const name = nameInput.value.trim();
        const score = parseInt(scoreInput.value);

        if (!name || isNaN(score)) {
            alert('Please enter both name and score');
            return;
        }

        const endpoint = '/api/events/ELEMENTARY_LEADERBOARD';
        console.log('POST endpoint:', endpoint);

        // If backend is unavailable, save locally in localStorage and update UI
        if (!this.hasBackend) {
            const storageKey = `elementary_leaderboard_${this.gameName}`;
            const stored = JSON.parse(localStorage.getItem(storageKey) || '[]');
            const entry = {
                id: `local-${Date.now()}`,
                payload: { user: name, score: score, gameName: this.gameName },
                timestamp: new Date().toISOString()
            };
            stored.push(entry);
            localStorage.setItem(storageKey, JSON.stringify(stored));

            // Clear inputs and refresh local display
            nameInput.value = '';
            scoreInput.value = '';
            this.fetchElementaryLeaderboard();
            return;
        }

        const url = `${javaURI}${endpoint}`;
        console.log('Full URL:', url);

        // Create payload matching Java backend AlgorithmicEvent structure
        const requestBody = {
            payload: {
                user: name,
                score: score,
                gameName: this.gameName
            }
        };
        console.log('Payload:', JSON.stringify(requestBody));

        // POST to backend using API chaining pattern
        fetch(
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
                console.log('Score saved successfully:', savedEntry);
                // Clear inputs
                nameInput.value = '';
                scoreInput.value = '';
                // Fetch updated leaderboard from backend
                return this.fetchElementaryLeaderboard();
            })
            .catch(error => {
                console.error('Error adding score:', error);
                // Check for authentication errors (401 or 403 status)
                if (error.message && (error.message.includes('401') || error.message.includes('403'))) {
                    alert('Please login to access this feature.');
                } else if (error.message && error.message.includes('Failed to fetch')) {
                    alert('Network error: Unable to connect to server. Please check if the backend is running.');
                } else {
                    alert(`Failed to save score: ${error.message}`);
                }
            });
    }
```
```
   submitScore(username, score, gameName = null) {
        console.log('=== SUBMIT SCORE TO SCORE_COUNTER ===');
        
        if (!username || isNaN(score)) {
            return Promise.reject(new Error('Invalid username or score'));
        }

        const endpoint = '/api/events/SCORE_COUNTER';
        console.log('POST endpoint:', endpoint);

        // If backend unavailable, store locally and update display
        if (!this.hasBackend) {
            const storageKey = `score_counter_${gameName || this.gameName}`;
            const stored = JSON.parse(localStorage.getItem(storageKey) || '[]');
            const entry = {
                id: `local-${Date.now()}`,
                payload: { user: username, score: score, gameName: gameName || this.gameName },
                timestamp: new Date().toISOString()
            };
            stored.push(entry);
            localStorage.setItem(storageKey, JSON.stringify(stored));

            if (this.mode === 'dynamic') this.fetchLeaderboard();
            return Promise.resolve(entry);
        }

        const url = `${javaURI}${endpoint}`;
        console.log('Full URL:', url);

        // Create payload matching Java backend AlgorithmicEvent structure
        const requestBody = {
            payload: {
                user: username,
                score: score,
                gameName: gameName || this.gameName
            }
        };
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

    displayLeaderboard(data) {
        const list = document.getElementById('leaderboard-list');
        const preview = document.getElementById('leaderboard-preview');

        const filteredByGame = data.filter(event => {
            const name = event.payload?.gameName || event.payload?.game || 'Unknown';
            const user = event.payload?.user || event.payload?.username || 'Anonymous';
            
            // FILTER 1: Must be your game
            const isMyGame = name === this.gameName;
            // FILTER 2: Ignore the old "Red" or "Anonymous" spam entries
            const isNotSpam = user !== "Red" && user !== "Anonymous" && user !== "";
        
            return isMyGame && isNotSpam;

        });
```

<a id="asio"> </a>

## <font color="green"> Asynchronous I/O </font>

this allows for multitasking on API calls which prevents freezes because of the server waits as it all doesn't run on the main thread which runs most of the in action game assets. It looks like this (using .then()): (in level1.js update function)

```
  update() {
    // Gravity and Physics Logic
    const player = this.gameEnv.gameObjects.find(obj => obj.id === 'player');
    if (player) {
        this.vy += this.gravity;
        player.position.y += this.vy;

        const floor = this.gameEnv.innerHeight - player.height;
        if (player.position.y >= floor) {
            player.position.y = floor;
            this.vy = 0;
            this.isGrounded = true;
        } else {
            this.isGrounded = false;
        }

        if (player.pressedKeys?.[87] && this.isGrounded) {
            this.vy = -10;
            this.isGrounded = false;
        }
    }

    const currentScore = this.gameEnv.stats?.coinsCollected || 0;
    if (this.scoreElement) this.scoreElement.innerHTML = "Cookies Collected: " + currentScore;

    if (currentScore >= 5 && !this.scoreSubmitted) {
        this.scoreSubmitted = true; 
        const timeTaken = ((Date.now() - this.startTime) / 1000).toFixed(2);
        this.successElement.innerHTML = `
            <h1 style="color: red; font-size: 40px; margin-bottom: 10px;">VICTORY!</h1>
            <p style="color: white; font-size: 22px; margin-bottom: 20px;">Time: ${timeTaken}s</p>
            <div id="inputArea">
                <input type="text" id="playerName" placeholder="Enter Name" style="padding: 10px; width: 200px; text-align: center; border-radius: 5px;"><br><br>
            </div>
            <button id="finalBtn" style="padding: 15px 30px; font-size: 20px; cursor: pointer; background: red; color: white; border: none; font-weight: bold; border-radius: 8px;">SUBMIT SCORE</button>
        `;
        this.successElement.style.display = 'block';
        const finalBtn = this.successElement.querySelector('#finalBtn');
        finalBtn.addEventListener('click', () => {
            if (this.saveAttempted) {
                const engine = this.gameEnv.gameControl || this.gameEnv.game?.gameControl || this.gameControl;
                engine.currentLevelIndex = 1; engine.transitionToLevel();
                return; 
            }
            const name = document.getElementById('playerName').value.trim() || "Anonymous";
            finalBtn.disabled = true; finalBtn.innerHTML = "SAVING...";
            this.saveAttempted = true; 
            if (this.leaderboard) {
                this.leaderboard.submitScore(name, parseFloat(timeTaken), "RedRidingHood")
                    .then(() => {
                        this.successElement.querySelector('#inputArea').style.display = 'none';
                        finalBtn.disabled = false; finalBtn.style.background = "#28a745"; 
                        finalBtn.innerHTML = "GO TO LEVEL 2 →";
                    })
                    .catch(() => {
                        this.successElement.querySelector('#inputArea').style.display = 'none';
                        finalBtn.disabled = false; finalBtn.style.background = "#ffc107"; 
                        finalBtn.innerHTML = "CONTINUE →";
                    });
            }
        });
    }
  }
```

<a id="jsonp"> </a>

## <font color="orange"> JSON Parsing </font>

Identified with Json.parse(), it parses API responses to make it readable to json as before it's as complex as bytes of code! not even sm64 uses that this is all about AI responses like in an NPC and leaderboards which are both in level1.js seen in leaderboard.js:

```
  addElementaryScore() {
        console.log('=== ADD ELEMENTARY SCORE ===');
        const nameInput = document.getElementById('player-name');
        const scoreInput = document.getElementById('player-score');

        const name = nameInput.value.trim();
        const score = parseInt(scoreInput.value);

        if (!name || isNaN(score)) {
            alert('Please enter both name and score');
            return;
        }

        const endpoint = '/api/events/ELEMENTARY_LEADERBOARD';
        console.log('POST endpoint:', endpoint);

        // If backend is unavailable, save locally in localStorage and update UI
        if (!this.hasBackend) {
            const storageKey = `elementary_leaderboard_${this.gameName}`;
            const stored = JSON.parse(localStorage.getItem(storageKey) || '[]');
            const entry = {
                id: `local-${Date.now()}`,
                payload: { user: name, score: score, gameName: this.gameName },
                timestamp: new Date().toISOString()
            };
            stored.push(entry);
            localStorage.setItem(storageKey, JSON.stringify(stored));

            // Clear inputs and refresh local display
            nameInput.value = '';
            scoreInput.value = '';
            this.fetchElementaryLeaderboard();
            return;
        }
```



<style>
.btn-documentation { background-color: #98ff22ff !important; color: white !important; }
.btn-documentation:hover { background-color: #2db112ff !important; }
</style>

<div class="btn-group">
    <a href="{{site.baseurl}}/finale/documentation" class="btn btn-documentation">Documentation</a>
</div>

<br>