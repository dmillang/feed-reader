# Project Overview

In this project we are given a web-based application that reads RSS feeds. The inital files already include [Jasmine](http://jasmine.github.io/). Our objective is to complete the test suite.


## Writing the First Two Tests

The first `TO-DO` we encounter in `/jasmine/spec/feedreader.js` contains two tasks to complete:
    * TODO: Write a test that loops through each feed.
    * TODO: Write a test that loops through each feed

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

Both are can be solved if we check `/js/app.js` where we find:
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

In order to asynchronously call `loadFeed()`, we need to write it inside a `beforeEach` and with a `done` value inside the function:
```
beforeEach(function(done) {
    loadFeed(0, done);
});
```

We then just need to declare an `expect` that checks for the `.feed` length to be bigger than 0:
```
it('completes work', function() {
    const feed = document.querySelector('.feed');
    expect(feed.children.length > 0).toBe(true);
});
```


# Development Strategy

For a refresher (or reference) before you begin writing code, we recommend reviewing the content from [JavaScript Testing](https://www.udacity.com/course/javascript-testing--ud549). Your project will be evaluated by a Udacity code reviewer according to the [Feed Reader Testing project rubric](https://review.udacity.com/#!/rubrics/18/view). Please review for detailed project requirements.

1. Familiarize yourself with the starter code
    * Open up `index.html` and review the functionality of the application within your browser
    * What is all the code in `app.js` doing? Be sure to read all code comments
    * Check out `style.css`. How is styling applied to the application?
2. Explore the Jasmine spec file in `feedreader.js`
    * This is the file in which you'll be writing your tests
    * Make sure to read all code comments here as well
    * Review the [Jasmine documentation](http://jasmine.github.io) if needed
3. Edit the `allFeeds` variable in `app.js` to make the provided test fail
    * See how Jasmine visualizes this failure in your application
    * Return the `allFeeds` variable to a passing state after reviewing the failed test
4. Write a test that loops through each feed in the `allFeeds` object and ensures it has a URL defined _and_ that the URL is not empty
    * For example, how would you use a `for...of` loop in this test?
5. Write a test that loops through each feed in the `allFeeds` object and ensures it has a name defined and that the name is not empty
    * Think about how you wrote the previous test. What are you testing for this time?
6. Write a new test suite named `"The menu"`
    * What are you `describe`-ing in this test suite?
7. Write a test that ensures the menu element is hidden by default
    * You'll have to analyze the HTML and the CSS to determine how the hiding/showing of the menu element is implemented
    * What code in `app.js` is directly involved with toggling the menu on and off?
8. Write a test that ensures the menu changes visibility when the menu icon is clicked. This test should have two expectations: does the menu display itself when clicked, and does it hide when clicked again?
    * Think about how you wrote the previous test. What is different this time around?
    * Which clickable element are you checking for?
    * How do you "simulate" a mouse click that element without actually clicking it?
9. Write a test suite named `"Initial Entries"`
    * What are you `describe`-ing in this test suite?
10. Write a test that ensures when the `loadFeed` function is called and completes its work, there is at least a single `.entry` element within the `.feed` container
    * How does Jasmine's `beforeEach()`function work?
    * How does the `loadFeed()` function in `app.js` work? Is it synchronous or asynchronous?
11. Write a test suite named `"New Feed Selection"`
    * What are you `describe`-ing in this test suite?
12. Write a test that ensures when a new feed is loaded by the `loadFeed` function that the content actually changes
    * How is this test different from the previous test?

Additionally, note that:

 * No test should be dependent on the results of another
 * Callbacks should be used to ensure that feeds are loaded before they are tested
 * Error handling should be implemented for undefined variables and out-of-bound array access
 * When complete, all of your tests should pass

When you're all finished, write a `README` file detailing all steps required to successfully run the application. If you have added additional tests, provide documentation for what these future features are and what the tests are checking for.

# Contributing

This repository is the starter code for _all_ Udacity students. Therefore, we most likely will not accept pull requests.
