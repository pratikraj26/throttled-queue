# throttled-queue

This utility function allows throttling of a queue any arbitrary code. For example, making network calls to popular APIs
such as Twitter is subject to rate limits.  By wrapping all of your API calls in a throttle, it will automatically adjust
your requests to be within the acceptable rate limits.

## Installation
Can be used in a Node.js environment, or directly in the browser.
### Node.js
`npm install throttled-queue`
### Browser
`<script src="throttled-queue.min.js"></script>`

## Usage
1) If in node.js, `require` the factory function:
```
var throttledQueue = require('throttled-queue');
```
Else, include it in a script tag in your browser and `throttledQueue` will be globally available.

2) Create an instance of a throttled queue by specifying the maximum number of requests as the first parameter,
and the interval in milliseconds as the second:
```
var throttle = throttledQueue(5, 1000); // at most 5 requests per second.
```
3) Use the `throttle` instance as a function to enqueue actions:
```
throttle(function() {
    // perform some type of activity in here.
});
```

## Quick Examples
### Basic
Rapidly assigning network calls to be run, but they will be limited to 1 request per second.
```
var throttledQueue = require('throttled-queue');
var throttle = throttledQueue(1, 1000); // at most make 1 request every second.

for (var x = 0; x < 100; x++) {

    throttle(function() {
        // make a network request.
    });
}
```
### Reusable
Wherever the `throttle` instance is used, your action will be placed into the same queue, 
and be subject to the same rate limits.
```
var throttledQueue = require('throttled-queue');
var throttle = throttledQueue(1, 60 * 1000); // at most make 1 request every minute.

for (var x = 0; x < 50; x++) {

    throttle(function() {
        // make a network request.
    });
}
for (var y = 0; y < 50; y++) {

    throttle(function() {
        // make another type of network request.
    });
}
```
### Bursts
By specifying a number higher than 1 as the first parameter, you can dequeue multiple actions within the given interval:
```
var throttledQueue = require('throttled-queue');
var throttle = throttledQueue(10, 1000); // at most make 10 requests every second.

for (var x = 0; x < 100; x++) {

    throttle(function() {
        // This will fire at most 10 a second, as rapidly as possible.
    });
}
```
### Evenly spaced
You can space out your actions by specifying `true` as the third (optional) parameter:
```
var throttledQueue = require('throttled-queue');
var throttle = throttledQueue(10, 1000, true); // at most make 10 requests every second, but evenly spaced.

for (var x = 0; x < 100; x++) {

    throttle(function() {
        // This will fire at most 10 requests a second, spacing them out instead of in a burst.
    });
}
```

## Tests
Note: The tests take a few minutes to run. Watch the console to see how closely the actual rate limit gets to the maximum.
### Node.js
Run `npm test`.
### Browser
Open `test/index.html` in your browser.



