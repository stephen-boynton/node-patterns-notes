# Async Control Flow Patterns

Used to avoid callback hell.

## Promises

Promise is an abstraction that allows a function to return an object called
"promise", which represents the eventual result of an asynchronous operation.
Promise states:

* pending
* fulfilled
* rejected
* settled

`promise.then([onFulfilled], [onRejected])`

```js
asyncOperation(arg).then(
  result => {
    //do stuff
  },
  err => {
    //uh-oh
  }
);
```

The `then()` method synchronously returns another promise. If either the
`onFulfilled()` or `onRejected()` functions return a value _x_, the promise
returned by the `then()` method will be as follows:

* Fulfill with x if x is a value
* Fulfill with the fulfillment value of x if x is a promise or a thenable
* Reject with the eventual rejection reason of x if x is a promise or a thenable

What is a `thenable`??? It is a promise-like object with a `then()` method. The
is a promise that is not the same promise implementation already in use.

This allows us to build chains of promises, allowing easy aggregation and
arrangement of async operations in several configurations.

It also allows us to pass errors down the chain until handled by an
`onRejected()` method in the chain.

```js
asyncOperation(arg)
  .then(result1 => {
    asyncOperation(arg2);
  })
  .then(result2 => {
    return "done";
  })
  .then(undefined, err => {
    //handle error
  });
```

The `onFullfilled()` and `onRejected()` methods are guaranteed to be invoked
asynchronously! This protects our code from releasing Zalgo...

If an error is thrown, it will propagate through the chain until it hits a
method handling it.

## Parallel Execution

or Promise.all().
