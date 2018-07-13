async-interval-timer
====================

Use Case
--------
You want some piece of code to run repeatedly, with an n-millisecond wait in between completed runs. The piece of code that you want to run may be asynchronous. Example: every 30 seconds, asynchronously pull an update from a REST API and check for some condition; stop the timer when the condition is true. 

Info
----
The main point of the timer is this: the callback is not guaranteed to be called every n milliseconds, but there is guaranteed to be a wait of n milliseconds between the end of the callback's execution, and the next calling of the callback. Optionally works with asyncawait. 

How it Works
------------
Instantiate a Timer instance 
- specify an interval (number of milliseconds) 
- specify a callback 
- specify any other (non-required) options 
- start the timer 
- stop the timer when desired 

Options
------- 
runFirst: optionally, call the callback immediately upon starting the timer. Otherwise, the callback will be called for the first time, after one interval duration (the default).

Async Callbacks
---------------
Following the asyncawait paradigm, if your callback is an awaitable (async()), you can optionally start the timer with startAsync() instead of start(); this will ensure that the wait interval doesn't start until execution of the async callback is completely done. 

Usage:
------

### Instantiation
```javascript
const Timer = require('async-interval-timer'); 

//prints 'hi' every 1000 ms 
const timer = new Timer(1000, () => { console.log('hi')}); 

// - or - 
const timer = new Timer(1000); 
timer.setCallback(() => { console.log('hi')});
```

### Starting
```javascript
//if interval && callback already specified 
timer.start();

//if callback not yet specified (or to change it) 
timer.start(() => { console.log('hi')});

//call with 'runFirst' option
timer.start(() => { console.log('hi')}, true);
```

### Change the Interval While Running
```javascript
//if interval && callback already specified 
timer.start();

//increase interval to every minute
timer.changeInterval(60000);
```

### Stop the Timer
```javascript
const duration = timer.stop();

console.log ('timer has run for ' + duration + ' ms'); 
```

### Run Async Code 
```javascript
const Timer = require('async-interval-timer'); 

//asynchronous method 
const asyncMethod = async(() => {
    //some asynchronous operation 
});

//call asyncMethod every second
const timer = new Timer(1000, () => asyncMethod);

//start timer 
timer.startAsync(asyncMethod); 

```
