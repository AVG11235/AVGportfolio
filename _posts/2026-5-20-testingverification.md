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

<div class="image-gallery">
  <img src="{{site.baseurl}}/images/final-images/lead.png" alt="Image 37">
  <img src="{{site.baseurl}}/images/final-images/AI.png" alt="Image 38">

</div>


<a id="APIEH"> </a>

## <font color="gold"> API Error Handling </font>

this covers, the systematic process of identifying, intercepting, and resolving failures that occur when making API requests. It ensures system reliability and provides users with actionable, clear feedback when things go wrong. So if the link connected to the doesn't exist you get a 404 error like if you mistyped it, well actully no it focuses in things the game asks for, not just the link working like score

so the code gives a result, a failure result to tell you it's not working so you can better know what to solve


this appears in the file(s): leaderboard.js

```
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

>it works by Effective API error handling relies on returning standard HTTP codes paired with machine-readable, actionable data. 
1. HTTP Status CodesThe first layer of error identification uses standardized numerical codes to categorize the issue:4xx (Client Errors): The request was malformed or unauthorized (e.g., 400 Bad Request, 401 Unauthorized, 404 Not Found).5xx (Server Errors): The server failed to fulfill a valid request due to a bug or downtime (e.g., 500 Internal Server Error, 502 Bad Gateway).2. Standardized Problem DetailsTo avoid ambiguous error messages, best practices (such as the RFC 7807 Standard for Problem Details) dictate returning a uniform JSON schema that informs developers exactly what to fix.json{
  "type": "https://example.com",
  "title": "You do not have enough credit",
  "status": 403,
  "detail": "Your current balance is 30, but the requested action costs 50.",
  "instance": "/account/12345/msgs/abc",
  "code": "INSUFFICIENT_FUNDS"
}
Use code with caution.Best Practices for ImplementationTo build resilient and debuggable systems, ensure your API error handling covers the following:Be Specific and Actionable: Avoid vague errors like "Something went wrong." Provide exact reasons and guidance (e.g., "Password must be at least 8 characters").Hide Sensitive Info: Prevent 500 errors from leaking stack traces, database credentials, or internal server mechanics to end-users.Implement Logging & Monitoring: Automatically log all 4xx and 5xx errors internally so your engineering team can trace the sequence of API calls that led to the failure.Handle Retries Gracefully: For transient errors (e.g., 429 Too Many Requests or 503 Service Unavailable), implement an exponential backoff retry strategy rather than overloading the API server

>Standard Implementation PatternTo handle both network failures and server errors, follow this structure:Wrap in Try/Catch: Use a try...catch block with async/await to capture network errors.Check response.ok: Immediately after fetching, check if response.ok is true. If not, manually throw an error to trigger the catch block.Use finally: Use a finally block for cleanup tasks, such as hiding loading spinners.javascriptasync function fetchData(url) {
  try {
    const response = await fetch(url);
    
    // Check for HTTP errors (4xx, 5xx)
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
    return data;
  } catch (error) {
    // This catches network failures AND errors thrown above
    console.error("Fetch failed:", error.message);
  } finally {
    // Code here runs regardless of success or failure
    setLoading(false);
  }
}
Use code with caution.Code Review Checklist for Error HandlingWhen reviewing code for Fetch API failures, check for these critical items:Is response.ok checked? If the code only uses .then().catch(), it is likely ignoring 404/500 errors.Are errors swallowed? Ensure the catch block doesn't just log the error; it should ideally update the UI to inform the user or report it to a service like Sentry.Is the message user-friendly? Technical details like stack traces should be logged for developers but hidden from users.Are specific errors handled? Check if different actions are taken for specific codes (e.g., redirecting on a 401 Unauthorized).Is a timeout implemented? Standard fetch doesn't have a default timeout; verify if an AbortController is used for long-running requests.


That's all!!!!