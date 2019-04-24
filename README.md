# Project Overview

In this project we are given a web-based application that reads RSS feeds. The inital files already include [Jasmine](http://jasmine.github.io/). Our objective is to complete the test suite.

### How to successfully run the application
To run this aplication:
* Download or clone the repo
* Open index.html in your default browser
    * `Jasmine` will already be active in there. You can find it at the bottom of the page.
* There are a total of 4 suites, 7 specs and 0 failures:
    * New Feed Selection (suite)
        * content changes (green)
    * Initial Entries (suite)
        * completed work (green)
    * The menu (suite)
        * toggles on and off (green)
        * is hidden (green)
    * RSS Feeds (suite)
        * url defined (green)
        * name defined (green)
        * are defines (green)
* All the activity has taken place in the `feedreader.js` file. You can access it in `/jasmine/spec/feedreader.js`.


## Writing the First Two Tests

The first `TO-DO` we encounter in `/jasmine/spec/feedreader.js` contains two tasks to complete:
* TODO: Write a test that loops through each feed.
* TODO: Write a test that loops through each feed.

Both of them need to be enclosed inside the same `suite`
```
describe('RSS Feeds', function() {});
```

We can use the provided working test as an example of what we have to do. It reveals we are probably going to use [Jasmine](https://devhints.io/jasmine) terms such as `it`, `expect`, `toBeDefined` and `.not.toBe`.

Given the example working test 
```
it('are defined', function() {
    expect(allFeeds).toBeDefined();
    expect(allFeeds.length).not.toBe(0);
});
```

We can recreate the missing ones like so:

### TODO: Write a test that loops through each feed.
```
it('url defined', function() {
    for(let feed of allFeeds) {
        expect(feed.url).toBeDefined();
        expect(feed.url.length).not.toBe(0);
    }
});
```

### TODO: Write a test that loops through each feed
```
it('name defined', function() {
    for(let feed of allFeeds) {
        expect(feed.name).toBeDefined();
        expect(feed.name.length).not.toBe(0);
    }
});
```

If we load/refresh our `index.html ` we can see we get 0 failures (a visual representation of it can be seen by the 3 green dots right below the `Jasmine` logo).

We can modify `/js/app.js` to force the tests to fail, and if we then refresh `index.html` it should signal the failures in red.

## Writing the Test Suite for the Menu Functionality

We need to create a set of tests for the menu functionality.

To do so we first need to `describe` the suite. We can copy the structure we saw for the `RSS Feeds` suite. Note that this new suite belongs to the same `$() function(){};` function.

We `describe` it like so:
```
describe('The menu', function() {});
```

We have two TO-DOs to create for this suite:
* TODO: Write a test that ensures the menu element is hidden by default.
* TODO: Write a test that ensures the menu changes visibility when the menu icon is clicked.

Both can be solved if we check `/js/app.js` where we find:
```
menuIcon.on('click', function() {
    $('body').toggleClass('menu-hidden');
});
```
We'll also need a way to call the menu icon element. If we inspect it on `index.html` we can see that has a class of
```
menu-icon-element
```

### TODO: Write a test that ensures the menu element is hidden by default.
```
it('is hidden', function() {
    const body = document.querySelector('body');
    expect(body.classList.contains('menu-hidden')).toBe(true);
});
```

### TODO: Write a test that ensures the menu changes visibility when the menu icon is clicked.
```
it('toggles on and off', function() {
const body = document.querySelector('body');
const menu = document.querySelector('.menu-icon-link');

menu.click();
expect(body.classList.contains('menu-hidden')).toBe(false);

menu.click();
expect(body.classList.contains('menu-hidden')).toBe(true);
});
```

Note that to 'force' a click we had to use `menu.click()` after defining menu
```
const menu = document.querySelector('.menu-icon-link');
```

## Writing the Test Suite for Initial Entries

First we set up the `Initial Entries` suite as we've been doing:
```
describe('Initial Entries', function() {});
```

We just have one TO-DO to create for this suite:
* TODO: Write a test that ensures when the loadFeed function is called and completes its work, there is at least a single .entry element within the .feed container.

In order to asynchronously call the `loadFeed()` function we need to use `done` to handle the async within a `beforeEach`:
```
beforeEach(function(done) {
    loadFeed(0, done);
});
```

We then just need to declare an `expect` that checks for the `.feed` length to be bigger than 0. That will mean that there's at least a single `.entry` element:
```
it('completes work', function() {
    const feed = document.querySelector('.feed');
    expect(feed.children.length > 0).toBe(true);
});
```

## Writing the Test Suite for New Feed Selection

This suite also uses the the `loadFeed` function so we also need to make it work asynchronously. Again, we only have one TO-DO to create for this suite:
* TODO: Write a test that ensures when a new feed is loaded by the loadFeed function that the content actually changes.

We first write the suite:
```
describe('New Feed Selection', function() {});
```

To create two different feeds we just need to call the pre-made `loadFeed` function two times:
```
beforeEach(function(done) {
    loadFeed(0);
    /* Move content to firstFeed array to store */
    Array.from(feed.children).forEach(function(entry) {
        firstFeed.push(entry.innerText);
    });
    loadFeed(1, done);
});
```

We need to previously declare two variables in the suite scope. One to select the `feed` class: `const feed = document.querySelector('.feed');`. And one to store the feed and compare it with the feed after we run `loadfeed` a second time: `const firstFeed = [];`.

Last, we need to test if it's working. We write an `expect` inside the suite that will tell us if the first and second feeds are different:
```
/* Set test to green-check that the feed has changed */
it('content changes', function() {
    /* Create array to compre index values of new array against firstFeed's array */
    Array.from(feed.children).forEach(function(entry,index) {
        console.log(entry.innerText, firstFeed[index], entry.innerText === firstFeed[index]);
        /* Expect that when a new feed is loaded by the loadFeed function that the content actually changes (i.e, it's not the same) */
        expect(entry.innerText === firstFeed[index]).toBe(false);
    });
});
```
