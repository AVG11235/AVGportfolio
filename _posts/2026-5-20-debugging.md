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

To perform source-level debugging in Chrome DevTools, you use the Sources panel to set breakpoints and control code execution.
1. Open the Sources PanelAccess Chrome DevTools by pressing F12 or Ctrl+Shift+I (Cmd+Option+I on Mac) and click the Sources tab. Use the file navigator on the left to select the JavaScript file you want to inspect.
2. Set BreakpointsBreakpoints tell the browser to pause execution so you can inspect the application's state.Manual Breakpoint: Click the line number in the gutter where you want to pause. A blue icon indicates it is set.Conditional Breakpoint: Right-click a line number, select Add conditional breakpoint, and enter a JavaScript expression. The code only pauses if the expression is true.debugger Statement: Add debugger; directly into your source code. If DevTools is open, the browser will automatically pause at that line.
3. Step Through ExecutionOnce paused, use the debugging toolbar (usually in the top right of the Sources panel) to navigate the code:Resume (F8): Continues execution until the next breakpoint is hit.Step over (F10): Executes the next line of code but does not enter into functions called on that line.Step into (F11): Enters the first line of the function being called on the current line.Step out (Shift+F11): Finishes the current function and pauses at the line following the function call.4. Inspect StateWhile paused, you can examine values in real-time:Scope Pane: View all local and global variables currently in memory.Watch Expressions: Add specific variables or expressions to monitor their values as you step through code.Call Stack: See the history of function calls that led to the current execution point.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/sourcegame.png" alt="Image 32">
</div>

<a id="netw">  </a>

## <font color="red"> Network Debugging </font>

To perform network debugging using browser developer tools, follow these steps to examine API calls, identify CORS errors, and inspect response data:
1. Open the Network TabOpen the Developer Tools by right-clicking anywhere on the page and selecting Inspect, or by using keyboard shortcuts:Windows/Linux: Ctrl + Shift + I or F12macOS: Cmd + Opt + INavigate to the Network tab at the top of the DevTools panel. If it is empty, reload the page (press F5 or Cmd + R) to begin logging active requests.
2. Filter for API CallsTo focus on fetch requests and suppress noise like images or CSS, click the Fetch/XHR filter button in the Network tab toolbar. Each row now represents a specific data request made by the application.
3. Examine Response Status and HeadersLocate your request in the Name column and check the Status column:200 OK: Success.4xx/5xx: Client or server errors (often highlighted in red).Click a request to open its details panel and select the Headers tab to view the full Request URL, Method (GET, POST), and both request and response headers.
4. Identify CORS ErrorsCORS errors occur when a browser blocks a cross-origin request due to missing or incorrect security headers.Network Tab: A CORS failure often shows as a failed request with no status code or "CORS error" in the status column.Headers Tab: Look for Access-Control-Allow-Origin. If it is missing or doesn't match your domain, the browser will block the response.Console Tab: Check the Console for specific red error messages detailing which CORS policy was violated.
5. Inspect Fetch Data (Demo)To verify the actual data exchanged, use these tabs within the request details panel:Payload: View the data sent to the server (e.g., JSON in a POST request).Preview: Shows a formatted view of the returned data, making JSON objects easy to read.Response: Shows the raw content returned by the server.Pro Tip: If you need to test a fix, right-click a request and select "Copy as fetch". You can then paste it into the Console tab, modify parameters, and re-run the request without reloading the whole page.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/networkgame.png" alt="Image 34">
</div>

<a id="appl"> </a>

## <font color="pink"> Application Debugging </font>

To debug application storage like cookies, localStorage, and session data, use the Application tab in Chrome DevTools. This panel provides a central view for inspecting and managing all the data your web app stores in the browser.Steps to Inspect Stored DataOpen DevTools: Right-click anywhere on your page and select Inspect, or use the keyboard shortcut F12 (or Ctrl+Shift+I on Windows/Linux; Cmd+Option+I on Mac).Navigate to the Application Tab: Click the Application tab at the top of the DevTools window. If it's hidden, click the ">>" more tabs icon.Locate Storage in the Sidebar: On the left-hand menu, you will see a Storage section.Demo: Inspecting Specific Data TypesExamine Cookies: Expand the Cookies dropdown and select the site's URL. This table shows current cookies, their values, and expiration dates—crucial for verifying if a session token is set correctly.Inspect localStorage: Expand the Local Storage menu and click the domain to see persistent key-value pairs. You can double-click any value to edit it and test how your app reacts to data changes.View sessionData: Expand Session Storage to see temporary data that lasts only until the tab is closed.Debugging Login/StateFor login issues, check the Cookies or Local Storage to see if an auth_token or session_id exists. If you are logged in but the storage is empty, the token might be stored in a way that is hidden or expired. You can right-click any item to Delete it and refresh the page to see if it forces a logout.

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/appgame.png" alt="Image 33">
</div>

<a id="elem"> </a>

## <font color="yellow"> Elemental Inspection </font>

To perform Element Inspection and use the Element Viewer for inspecting canvas, DOM elements, and game object states, follow these steps based on your development environment:1. General Web DOM & Style InspectionFor standard web elements and CSS styles, use your browser's built-in developer tools:Open the Inspector: Right-click any element on a webpage and select Inspect or Inspect Element.Keyboard Shortcuts: Use Ctrl + Shift + C (Windows/Linux) or Cmd + Option + C (macOS) to enter Inspect Mode directly.Analyze Styles: Once an element is selected, the Styles pane displays all applied CSS rules, including those that are inherited or overridden.2. Canvas & Game Object InspectionStandard DOM inspectors cannot "see" inside a <canvas> element because it is a single pixel-based surface. To inspect game objects or canvas states:Chrome Canvas Inspection: You can enable experimental canvas debugging by navigating to chrome://flags and enabling Developer Tools experiments.Specialized Extensions: For WebGL-based games, tools like the WebGL Inspector (or similar browser extensions) allow you to capture frames and view the state of game objects and textures.Unity Element Viewer: If you are working in Unity, the Element Viewer is used within the editor to modify properties of UI elements or game objects in a tree-like structure, similar to the browser's DOM tree.3. Debugging Game State via ConsoleIf specialized visual tools are unavailable, you can often inspect game object states through the Console:Get a reference to the canvas or game object in the code.Use yourCanvas.toDataURL() to grab a snapshot of the current frame.Log game objects to the console (console.log(gameObject)) to inspect their properties, variables, and current state in real-time.

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