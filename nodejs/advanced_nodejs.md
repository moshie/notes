# Advanced Node.js

[Course](https://app.pluralsight.com/library/courses/nodejs-advanced/table-of-contents)

### Concurrency model & event loop
*What is I/O*

    - IO is used to label the comunication between a process on a computer CPU and anything external e.g memory, disk, network or even another process
    - IO in terms of node usually means accessing disk and network resources which is the most time expensive
    - Handling slow I / O operations
        - Synchronous
            - One request holds up other requests
        - fork()
            - handle each request with a new fork but doesn't scale very well with a lot of requests
        - Threads
            - Popular but can get complicated when threads start accessing shared resources
            - a lot of popular libarays and frameworks use threads (apache)
        - Event Loop
            - Single threaded frameworks like node use Event loops to handle slow IO operations without blocking the main execution runtime

*Event loop*

    - The entity that handles external events and converts them into callback invocations
    - A loop that picks events from the event queue and pushes their callbacks to the call stack
    - Node starts the event loop node exits the loop when there are no more callbacks to perform
    - V8 Heap is where objects are stored in memory
    - Stack & Heap are part of the runtime engine not node

*Call stack*

    - Call stack is simply a list of functions
    - JavaScript is single threaded One stack

*Handling slow operations*

    - Nodes event loop exists to stop us from writing synchronus code

*How callbacks actually work*

    - node api is based around callbacks

    ```
        const slowAdd = (a, b) => {
            setTimeout(() => { // Stack (TWO & FOUR) then immediently removed 
                console.log(a+b); // STACK (FIVE & SIX)
            }, 5000)
        };

        slowAdd(3,3); // Stack (ONE)

        slowAdd(4, 4); // Stack (THREE)
    ```
    - Async functions interact with the Event loop
    - Event queue / Message queue / callback queue (List of things to be processed [EVENTS])
    - When we store an event on the queue we sometimes store a plain old function with it (Callback) < these get added to the Queue
    - 
    ```
        Event Loop Logic:

        When the call stack gets empty: 

        While the queue is not empty: 
            event = dequeue an event
            if there is a callback: 
                call the event's callback
    ```

*setImmediate and process.nextTick*

    - use setImmediate if you want something to run on the next tick of the event loop
    - Function should always be sync or async

### Node Event driven Architecture

*Callbacks, promises and Async/Await*

    - callbacks !== async
    - async function `node --harmony_async_await`

    ```
        async function countOdd() {
            try { // To handle errors wrap in a try catch
                const lines = await readFileAsArray('./numbers');
                // continue as if it was syncronus
            } catch (e) {
                log(e);
            }
        }
        countOdd();
    ```

*EventEmitter*
    
    - Events are not async or sync
    - 
    ```
        const EventEmitter = require('events'); // Import
        class Logger extends EventEmitter {} // extend
        const logger = new Logger(); // init
        logger.emit('event'); // Emit
        logger.on('event', listenerFunc); // addListener
    ```


Useful ES6 thing
```
    var test = {
        0: 'data for id 0',
        1: 'data for id 1',
        2: 'data for id 2'
    };
    Object.entries(test).forEach(([key, value]) => {
        // value
        // key
    });
```





