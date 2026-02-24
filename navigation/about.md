---
layout: post
title: About
permalink: /about/
comments: true
---

## As a conversation Starter

Here are some important places to me.

<comment>
Flags are made using Wikipedia images
</comment>

<style>
    /* Style looks pretty compact, 
       - grid-container and grid-item are referenced the code 
    */
    .grid-container {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); /* Dynamic columns */
        gap: 10px;
    }
    .grid-item {
        text-align: center;
    }
    .grid-item img {
        width: 100%;
        height: 100px; /* Fixed height for uniformity */
        object-fit: contain; /* Ensure the image fits within the fixed height */
    }
    .grid-item p {
        margin: 5px 0; /* Add some margin for spacing */
    }

    .image-gallery {
        display: flex;
        flex-wrap: nowrap;
        overflow-x: auto;
        gap: 10px;
        }

    .image-gallery img {
        max-height: 150px;
        object-fit: cover;
        border-radius: 5px;
    }
</style>

<!-- This grid_container class is used by CSS styling and the id is used by JavaScript connection -->
<div class="grid-container" id="grid_container">
    <!-- content will be added here by JavaScript -->
</div>

<script>
    // 1. Make a connection to the HTML container defined in the HTML div
    var container = document.getElementById("grid_container"); // This container connects to the HTML div

    // 2. Define a JavaScript object for our http source and our data rows for the Living in the World grid
    var http_source = "https://upload.wikimedia.org/wikipedia/commons/";
    var living_in_the_world = [
        {"flag": "f/fc/Flag_of_Mexico.svg", "greeting": "Hallo", "description": "My Birthplace"},
        {"flag": "d/df/Flag_of_Peru_%28state%29.svg", "greeting": "Over here!", "description": "Father's home country"},
        {"flag": "9/9e/Flag_of_Japan.svg", "greeting": "YO!", "description": "Favorite Vacation"},
        {"flag": "c/c3/Flag_of_France.svg", "greeting": "Bonjour", "description": "an extra bit of genetics"},
    ];

    // 3a. Consider how to update style count for size of container
    // The grid-template-columns has been defined as dynamic with auto-fill and minmax

    // 3b. Build grid items inside of our container for each row of data
    for (const location of living_in_the_world) {
        // Create a "div" with "class grid-item" for each row
        var gridItem = document.createElement("div");
        gridItem.className = "grid-item";  // This class name connects the gridItem to the CSS style elements
        // Add "img" HTML tag for the flag
        var img = document.createElement("img");
        img.src = http_source + location.flag; // concatenate the source and flag
        img.alt = location.flag + " Flag"; // add alt text for accessibility

        // Add "p" HTML tag for the description
        var description = document.createElement("p");
        description.textContent = location.description; // extract the description

        // Add "p" HTML tag for the greeting
        var greeting = document.createElement("p");
        greeting.textContent = location.greeting;  // extract the greeting

        // Append img and p HTML tags to the grid item DIV
        gridItem.appendChild(img);
        gridItem.appendChild(description);
        gridItem.appendChild(greeting);

        // Append the grid item DIV to the container DIV
        container.appendChild(gridItem);
    }
</script>

### MIAE LIEF('s journey)

Here's what I've done

- Become born
- Graduated Oak Valley
- Made a number of now vanishified friends (where'd they go?!)
- Grown up and now into HS
- have cried like 6 to 8 times in the last 3 years
- Learned many talents
- Produced much content and more to come
- I can ride a bike 

### Yo, What's up, and my Homies?!

I have had a neat life, doing many things like yo- jk.

- I've grown to the point of 15 year old, and had like 15 haircuts or something
- My fam, hmm, well my fam's pretty lit. ok not really, I have a simple family of 4 as the youngest, and many cousins that on 50% of holidays, My family goes to my favorite cousins place in utah, the Wilsons.
- Up next we have many family ipad pics which held some retro memories that I'm not showing all of them.

### HAPPY NEW YEAR!
<div class="grid-container" id="newyears_container">
    <!-- content will be added here by JavaScript -->
</div>

<script>
   var container = document.getElementById("newyears_container"); // This container connects to the HTML div

    // 2. Define a JavaScript object for our http source and our data rows for the Living in the World grid
    var http_source = "https://upload.wikimedia.org/wikipedia/commons/";
    var living_in_the_world = [
        {"flag": "thumb/d/d0/Happy_new_year.jpg/640px-Happy_new_year.jpg", "greeting": "Feliz, estamos Feliz", "description": "Por un ano nuevo"},
        {"flag": "thumb/7/73/Season%27s_Greetings_A_Happy_New_Year%21_%28eso0653c%29.tiff/lossy-page1-640px-Season%27s_Greetings_A_Happy_New_Year%21_%28eso0653c%29.tiff.jpg", "greeting": "Galaxial omg", "description": "A new year happenned in here, it seemed kinda small though"},
        {"flag": "thumb/4/4a/Happy_New_Year_2022%21.gif/640px-Happy_New_Year_2022%21.gif", "greeting": "Christmas is done", "description": "It was great like our NEW YEAR!!!!!"},
        {"flag": "thumb/9/97/Happy_new_year_rnd.svg/640px-Happy_new_year_rnd.svg.png", "greeting": "Ah yes the color of 2026", "description": "HAVE THE HAPPIEST NEW YEAR OF ALL THYME CHAT"},
    ];

    // 3a. Consider how to update style count for size of container
    // The grid-template-columns has been defined as dynamic with auto-fill and minmax

    // 3b. Build grid items inside of our container for each row of data
    for (const location of living_in_the_world) {
        // Create a "div" with "class grid-item" for each row
        var gridItem = document.createElement("div");
        gridItem.className = "grid-item";  // This class name connects the gridItem to the CSS style elements
        // Add "img" HTML tag for the flag
        var img = document.createElement("img");
        img.src = http_source + location.flag; // concatenate the source and flag
        img.alt = location.flag + " Flag"; // add alt text for accessibility

        // Add "p" HTML tag for the description
        var description = document.createElement("p");
        description.textContent = location.description; // extract the description

        // Add "p" HTML tag for the greeting
        var greeting = document.createElement("p");
        greeting.textContent = location.greeting;  // extract the greeting

        // Append img and p HTML tags to the grid item DIV
        gridItem.appendChild(img);
        gridItem.appendChild(description);
        gridItem.appendChild(greeting);

        // Append the grid item DIV to the container DIV
        container.appendChild(gridItem);
    }
</script>



<comment>
Gallery of my Pad Pics (be sure to scroll to the right for more ...)
</comment>
<div class="image-gallery">
  <img src="{{site.baseurl}}/images/about/pics/IMG_0064.jpg" alt="Image 1">
  <img src="{{site.baseurl}}/images/about/pics/IMG_1770.jpg" alt="Image 2">
  <img src="{{site.baseurl}}/images/about/pics/IMG_2052.png" alt="Image 3">
  <img src="{{site.baseurl}}/images/about/pics/IMG_2060.png" alt="Image 4">
  <img src="{{site.baseurl}}/images/about/pics/IMG_2246.png" alt="Image 5">
  <img src="{{site.baseurl}}/images/about/pics/IMG_2497.png" alt="Image 6">
  <img src="{{site.baseurl}}/images/about/pics/IMG_2839.png" alt="Image 7">
  <img src="{{site.baseurl}}/images/about/pics/IMG_3054.png" alt="Image 8">
  <img src="{{site.baseurl}}/images/about/pics/IMG_4871.png" alt="Image 9">
  <img src="{{site.baseurl}}/images/about/pics/IMG_4907.png" alt="Image 10">
  <img src="{{site.baseurl}}/images/about/pics/IMG_8586.png" alt="Image 11">
</div>
