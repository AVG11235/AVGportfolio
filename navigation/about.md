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
        {"flag": "5/54/War_flag_of_Peru.svg?utm_source=commons.wikimedia.org&utm_campaign=index&utm_content=original", "greeting": "Over here!", "description": "Father's home country"},
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
- Graduated Oak Valley with a signature by Donald Trump
- Made a number of friends #quanityoverquality (jk btw jeez louis there's so many) 
- Grown up and now into HS
- have cried like 6 to 8 times in the last 4 years
- Learned many talents
- Produced much content and more to come
- I can ride a bike 

### Yo, What's up, and my Homies?!

I have had a neat life, doing many things like yo- jk.

- I've grown to the point of 16 year old, and had like 16 haircuts or something
- My fam, hmm, well my fam's pretty lit. ok not really, I have a simple family of 4 as the youngest, and many cousins that on 50% of holidays, My family goes to my favorite cousins place in utah, the Wilsons, recently I've seen a lot of my cousin Carlota, and she's become pretty alright now, so that's a nice suprise.
- Up next we have many facial pics which are taken on my new and first phone.

### HAPPY HALLOWEEN !!! JESUS LOVES YOU !!!
<div class="grid-container" id="newyears_container">
    <!-- content will be added here by JavaScript -->
</div>

<script>
   var container = document.getElementById("newyears_container"); // This container connects to the HTML div

    // 2. Define a JavaScript object for our http source and our data rows for the Living in the World grid
    var http_source = "https://upload.wikimedia.org/wikipedia/commons/";
    var living_in_the_world = [
        {"flag": "2/28/FileSpooky_reflection_of_the_South_Portico_decorated_for_Halloween%2C_Monday%2C_October_30%2C_2023%2C_at_the_White_House.jpg?utm_source=commons.wikimedia.org&utm_campaign=index&utm_content=original", "greeting": "My Family loves checking out all the houses that the owners put 110% effort into decorating", "description": "One person in a culdesac near us really goes crazy for halloween AND christmas"},
        {"flag": "9/9a/Pyrkon_2022_-_Among_Us_cosplay.jpg?utm_source=commons.wikimedia.org&utm_campaign=index&utm_content=original", "greeting": "I went as this a couple times", "description": "my mother the worst (best) instagram post of all time about it"},
        {"flag": "1/13/Jack-o-lantern_pumpkins.jpg?utm_source=commons.wikimedia.org&utm_campaign=index&utm_content=original", "greeting": "Halloween is soon", "description": "It will be great like our NEW YEAR!!!!!"},
        {"flag": "c/c3/Schriever_celebrates_Halloween_%284861702%29.jpg?utm_source=commons.wikimedia.org&utm_campaign=index&utm_content=original", "greeting": "The Candy collection at the end of all my halloweens were always crazy", "description": "And the preservation saga afterwards... oof"},
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
Gallery of my Facial compilation Pics (scroll right for more)
</comment>
<div class="image-gallery">
  <img src="{{site.baseurl}}/images/about/pics/tomatoy.png" alt="Image 1">
  <img src="{{site.baseurl}}/images/about/pics/aww.jpeg" alt="Image 2">
  <img src="{{site.baseurl}}/images/about/pics/AH.png" alt="Image 3">
  <img src="{{site.baseurl}}/images/about/pics/oop.png" alt="Image 4">
  <img src="{{site.baseurl}}/images/about/pics/speed.png" alt="Image 5">
  <img src="{{site.baseurl}}/images/about/pics/yawn.png" alt="Image 6">
  <img src="{{site.baseurl}}/images/about/pics/SBROK.png" alt="Image 7">
  <img src="{{site.baseurl}}/images/about/pics/smile.png" alt="Image 8">
  <img src="{{site.baseurl}}/images/about/pics/Hat.png" alt="Image 9">
</div>
