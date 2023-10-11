# Javascript
Notes on JavaScript development

- [Javascript](#javascript)
- [Variables](#variables)
  - [Scoping](#scoping)
- [Functions](#functions)
  - [IIFE (Immediately Invoked Function Expression)](#iife-immediately-invoked-function-expression)
  - [Closures](#closures)
  - [Arrow Functions](#arrow-functions)
  - [Call, Apply, Bind](#call-apply-bind)
    - [Call](#call)
    - [Apply](#apply)
    - [Bind](#bind)
  - [Spread Operator](#spread-operator)
    - [Spread Syntax](#spread-syntax)
- [Promises](#promises)
  - [Examples](#examples)
    - [Pending State](#pending-state)
    - [Fulfilled State](#fulfilled-state)
    - [Rejected State](#rejected-state)
  - [Chaining Promises](#chaining-promises)
    - [Chained error handling](#chained-error-handling)
  - [Creating Promises](#creating-promises)
    - [Nested promise](#nested-promise)
  - [Asynchronous Programming](#asynchronous-programming)
    - [.allSettled()](#allsettled)
    - [Race](#race)
  - [Async/Await](#asyncawait)
    - [Error Handling](#error-handling)
    - [Awaiting concurrent requests](#awaiting-concurrent-requests)
    - [Awaiting parallel requests](#awaiting-parallel-requests)
- [Generators and Iterators](#generators-and-iterators)
  - [Iteratable](#iteratable)
  - [Generator Functions](#generator-functions)
    - [Yield](#yield)
  - [Async flows](#async-flows)
- [Modules](#modules)
  - [Named export](#named-export)
  - [Default Exports](#default-exports)
  - [Aggregating modules](#aggregating-modules)
  - [Enabling Modules](#enabling-modules)


# Variables
## Scoping
`let` is block scoped, `var` is function scoped
```js
function scoping() {
    {
        var foo = 'foo';
        let bar = 'bar';
    }

    console.log('foo'); // foo
    console.log('bar'); // ReferenceError
}
```


```
# Functions
## Arguments Object
The arugments object is a lcoal variable within a function that contains the values of the arguments passed to that function

```js
function func1(a,b,c) {
    console.log(arguments[0]); // will log the value of 'a'
}
```
```js
function displayAll() {
    for(let i = 0; i < arguments.length; i++) {
        console.log(arguments[i]);
    }
}

displayAll(1, 2, 5); // 1 2 5
```
Same behaviour is achievable in ES6 using rest parameters

# Functions
## IIFE (Immediately Invoked Function Expression)
Calling a function as soon as it is declared
```js
(function() {
    console.log("Hello world!");
})(); // Hello WOrld
```
Note this function is not assigned to any name

## Closures
A closure is a combination of a function bundled with its lexical environment (its surrounding state). For example:

```js
function outerFunc() {
    const name = "Billy";
    function innerFunc() {
        console.log(name);
    }

    return innerFunc;
}

const myFunc = outerFunc();
myFunc(); // Billy
```
Here `outerFunc` returns `innerFunc`, without actually executing `innerFunc`. So at first glance, it may seem that the constant `name` is not accessible to `myFunc` at runtime. However, JavaScript functions form *closures*. So in the above code, `myFunc` is really a reference to `innerFunc` that was createdwhen `outerFunc` was run, and the constant `name` is part of the lexical environment which is saved in the closure alongside `innerFunc`.

Here's another example of using closures:
```js
function initAdder(x) {
    return function (y) {
        return x + y
    };
}

const add5 = initAdder(5);
const add10 = initAdder(10);

add5(2); // 7
add10(2); // 12
```
The `add5` and `add10` functions both have access to the `x` argument as part of the closure.

## Arrow Functions
We can also define funcions using the arrow shorthand
```js
const myFunc = (args) => {
    // Do Something
}
```
However a few important things to note is that arrow functions don't have access to `this`, `arguments` or `super`. This means they should not be used as methods.
```js
const person = {
    name: 'Billy',
    getName: function() {
        return this.name;
    }
    getNameArrow: () => {
        return this.name;
    }
}

person.getName(); // 'Billy'
person.getNameArrow(); // nothing, as 'this' in the arrow function refers to the global window
```
Arrow functions also aren't suitable for `call`, `apply` and `bind` methods.

## Call, Apply, Bind
### Call
Call and apply are ways to call a function, while specifying a `this` value. For example:
```js
function displayName() {
    return this.name;
}

const PersonA = {
    name: "Billy",
    age: 24
};

const PersonB = {
    name: "Tom",
    age: 23
};

displayName.call(PersonA)); // "Billy"
displayName.call(PersonB); // "Tom"
```

Call can also take in arguments
```js
function displayName(age, address) {
    return `${this.name} is ${age} and lives in ${address}`;
}

// "Billy is twenty and lives in Melbourne"
displayName.call(PersonA, "twenty", "Melbourne");
```
### Apply
Apply works exactly the same way, except the arguments are passed in as an array
```js
displayName.apply(PersonA, ["twenty", "Melbourne"]);
```

### Bind
Bind works like `apply`, but does not immediately invoke the function
```js
const displayBilly = displayName.bind(PersonA, "twenty", "Melbourne");
displayBilly(); // "Billy is twenty and lives in Melbourne"
```

## Spread Operator
### Spread Syntax
The spread allows you to expand an iterable in a place where individual items are expected. 

Spread in a function:
```js
function addThree(x, y, z) {
    return x + y + z;
}

const myArray = [1, 2, 3];

addthree(...myArray) // 6;
```

Spread in arrays:
```js
a = [3, 4];
b = [5, 6];

c = [...a]; // one-level deep copy of array
d = [1, 2, ...a, ...b]; // [1, 2, 3, 4, 5, 6]
```

Spread in objects:
```js
originalObj = { key1: "value1", key2: "value2" };
shallowClone = { ...originalObj }; // { key1: "value1", key2: "value2" };
newObject = { ...originalObj, key1: "valueNew" }; // { key1: "valueNew", key2: "value2" };
```

# Promises
*Object that represents the eventual completion (or failure) of an asynchronous operation, and its resulting value*

Promises can have 3 states:
- **Pending**: This is a promise's initial state upon creation. For an API call, while the call is happening, the promise will be pending.
- **Fulfilled**: A promise moves from the pending to fulfilled state when the asynchronous call has been completed. A fulfilled promise will return a single value (e.g. the result of the API call)
- **Rejected**: If the asynchronous call has failed. It returns a rejection reason.

## Examples

### Pending State
If we use calling an endpoint as an example:
```js
const myPromise = axios.get('https://www.google.com/');
```

```js
console.log(myPromise);

> Promise {[[PromiseState]]: 'pending', [[PromiseResult]]: undefined, Symbol(async_id_symbol): 4692, Symbol(trigger_async_id_symbol): 4691}
```

### Fulfilled State

To work with the promise upon the _fulfilled_ state, we use `.then()`
```js
axios.get('https://www.google.com/')
    .then(({data}) => {
        console.log(data);
    });
```
and we can see the Google page being returned
```
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en-AU"> ...
```

### Rejected State
We use `.catch()` to handle rejected promise state. Let's make our promise fail by supplying it an invalid URL:
```js
axios.get('INVALID_URL')
    .then(({data}) => {
        console.log(data);
    });
```
```
Process exited with code 1
Uncaught AxiosError AxiosError
    at processPromiseRejections
    at processTicksAndRejections
```

However, if we add in a `.catch()` statement to handle the rejected promise, we can handle an error like this:

```js
const a = axios.get('INVALID_URL')
    .then(({data}) => {
        console.log(data);
    })
    .catch(error => {
        console.log(error);
    });
```
```
> AxiosError { ... } // Errror sucessfully handled and logged
```
## Chaining Promises
Remember our original promise example?
```js
const myPromise = axios.get('https://www.google.com/');
console.log(myPromise);
```
```
> Promise {[[PromiseState]]: 'pending', [[PromiseResult]]: undefined, Symbol(async_id_symbol): 4692, Symbol(trigger_async_id_symbol): 4691}
```

What kind of object does this become when we have a `.then()` or `.catch()` block?
```js
const myPromise = axios.get('https://www.google.com/')
    .then(({data}) => {
        console.log(data);
    })
console.log(myPromise);
```
```
> Promise {[[PromiseState]]: 'pending', [[PromiseResult]]: undefined, Symbol(async_id_symbol): 4692, Symbol(trigger_async_id_symbol): 4691}
```
We get the exact same object. A _promise returns a promise_. This allows us to chain together promises

```js
axios.get('SOME_API_URL')
    .then(({ data }) => {
        return axios.get(`SOME_OTHER_URL/${data.valueFromPrevCall}`);
    })
    .then(({ data }) => {
        console.log(data);
    });
```
This will ensure our first API call finishes first (promise fulfilled), before moving onto our second API call that is dependent on a return value from the first.

### Chained error handling
Adding a `.catch()` into a promise change will catch any errors from promises above it

`.finally()` will execute at the end of the promise chain, after all `.then` and `.catch` have been performed.

## Creating Promises
We've looked at using HTTP libraries like Axios to investgiate promise behaviours, but how do we create our own promises?

We mentioned that a Promise as, first and foremost, an _object_. It takes an _executor function_ as a parameter, that has a `resolve` parameter that the promise can use to call and resolve its state.

```js
let myPromise = new Promise((resolve) => resolve());
```
This will create a promise that is resolved straight away.

```js
let myPromise = new Promise((resolve) => {
    setTimeout(resolve, 1500);
})
```
This will create a promise that is resolved after 1.5 seconds

But what exactly does _resolved_ mean? Resolving a promise means settling its state so that further resolving or rejecting will have no effect.

For example consider:
```js
let myPromise = new Promise((resolve) => {
    setInterval(() => {
        console.log('Interval');
        resolve('RESOLVED');
    }, 1500);
})

myPromise.then((val) => console.log(val));
```
`setInterval` will fire off the resolved function every 1.5s, but only the first one will have any effect on the promise
```
> Interval
> RESOLVED
> Interval
> Interval
```

We could use a `.finall()` block to clear the interval if we wanted to.

### Nested promise
```js
new Promise((resolveOuter) => {
  resolveOuter(
    new Promise((resolveInner) => {
      setTimeout(resolveInner, 1000);
    }),
  );
});
```
Here's an example from MDN, where the main promise is already _resolved_ at the time of creation (as the `resolveOuter` function is called synchronously), however it is resolved with another promise, and therefore won't be _fulfilled_ until 1 second later when the inner promise fulfills.

## Asynchronous Programming
With promises, we don't have to wait for individual calls to finish before moving onto the next (although we do often do this). So how can we use several promises asynchronously?

`Promise.all()` takes an iterable of promises as its input, and returns a single Promise that only fulfills when all of the input's promises fulfill. The `.then()` function's results is an array of results that matches the order of the _iterable_ we provided to the `.all()`, and _not_ the order the individual promises are resolved:

```js
let users = axios.get('localhost:3000/users');
let orders = axios.get('localhost:3000/orders');

Promise.all([users, orders])
    .then(([u, o]) =>
        processData(u, o)
    ).catch((reasons) =>
        console.log(reasons)
    );
```

We must still remember to `.catch()` errors, and note that the `.all` promise will be rejected when any one promise is rejected.

### .allSettled()
`.allSettled()` is similar to `Promise.all()`, except you don't need a catch block as it will return when all promises have _settled_: Even if some are rejected. Note that `.all()` returns an array of fulfillment values, whereas `.allSettled()` returns an array of object with two keys, that will either be:
```js
{
    status: "fulfilled",
    value: {}
}
```
```js
{
    status: "rejected",
    reason: {}
}
```

An example will be:
```js
let users = axios.get('localhost:3000/users');
let orders = axios.get('localhost:3000/orders');

Promise.allSettled([users, orders])
    .then((results) => {
        results.forEach((result) =>{
            if (result.status === 'fulfilled'){
                console.log(result.value);
            } else {
                console.log(result.reason);
            };
        });
    });
```

### Race
`Promise.race()` is another function, settles with the state of the _first_ promise to settle that is passed in. That is, if the first promise is fulfilled it will fulfill, otherwise it will reject.

## Async/Await
Syntactic sugar to make it easier to use promises in a synchronous format.

`async` is a keyword that creates an asynchronous function that aims to avoid the need for promise chains (you simply need to write successive `await` calls). An async function returns an implicit promise: Whatever is returned is wrapped inside of a promise.

`await` pauses execution of an async function while it waits for the promise to be fulfilled. It can only be used inside an async function. The resolved value of the promise is the return value of the awaited expression.

**NOTE**: `await` only blocks the _current_ function. For example:

```js
const func1 = async () => {
    await someFunc();
    doSomethingElse();
}

func1();
func2();
```

The `await` keyword will block `doSomethingElse()` but _not_ block `func2()`;

### Error Handling
Using async and await allows us to use try/catch blocks to catch errors! Nothing special here :)

### Awaiting concurrent requests
If we have two API requests that we await as such:
```js
const orderStatus = await axios.get("/orderStatsues");
const orders = await axios.get("/orders");
```
The code will prevent the `/orders` endpoint from executing before `/orderStatuses` returns. However, what if `/orderStatuses` takes a while? What if we want both API calls to occur concurrently? We can simply delay the await.

```js
const orderStatus = axios.get("/orderStatuses");
const orders = axios.get("/orders");

const {data: statuses} = await orderStatuses;
const {data: order} = await orders;
```

As promises are _eager_, they will be fired as soon as we run `axios.get`. As we `await` later, the two promises are already concurrently making their requests. By the time `await orderStatuses` is done, `orders` API has also already fulfilled its promise, so we waste no time.

### Awaiting parallel requests
We can use `Promise.all()` in conjunction with async/await syntax to handle promises in the order that they return (rather than the order we await them in). We simply need to put our async functions in as arguments to `Promise.all()`.

```js
await Promise.all([
    (async () => 
        const {data} = await axios.get("/orderStatuses");
        console.log(data);
    )(),
    (async () => 
        const {data} = await axios.get("/orders");
        console.log(data);
    )(),
]);
```

# Generators and Iterators
We are familiar with basic loops such as:
```js
for (let i = 0; i < 10; i++) {
    if (i > 2) break;
    console.log(i);
}
```
But in these basic scenarios, once we `break` from the loop we can't resume it from the same place simply.

An _iterator_ lets you iterate through a collectoin's content one at at time, pausing at each item. It is an object that implements the Iterator protocol by having a `next()` method that returns a `value` property and `done` property.

We can manually create an iterator as follows:

```js
function myIterator(start, finish) {
  let index = start;
  let count = 0;

  return {
    next() {
      if (index < finish) {
        return { value: ++index, done: false };
      } else {
        return { value: count, done: true };
      }
    }
  };
}

const it = myIterator(0, 5);
res = it;

while(!res.done) {
  console.log(res.value);
  res = it.next();
};

```

## Iteratable
Although we can iterate through our above iterator, it is not an _iterable_. For an object to be an iterable it must implement the @@iterator method. An iterable will work with `for... in`. A basic example of an iterable is an array

```js
const arr = ["a", "b", "c", "d", "e"];
const arrIter = arr[Symbol.iterator]();
console.log(arrIter.next().value); // a
console.log(arrIter.next().value); // b
console.log(arrIter.next().value); // c
console.log(arrIter.next().value); // d
console.log(arrIter.next().value); // e
```

As we see from the above example, we can use the `[Symbol.iterator]` method of an iterable to retrieve an iterator to work with.

## Generator Functions
A generator function is a function that can be paused and resumed at a later time, while having the ability to pass values to and from the function at each pause point.

A generator function has an `*` keyword before the name of the function, and _returns an iterable_.

The `yield` keyword signals the pause point of a generator function. Consider the following example

```js
function* myGenerator() {
  console.log('Step 1');
  yield;
  console.log('step 2');
};

const it = myGenerator();
it.next(); // Step 1
it.next(); // Step 2
```

### Yield
`yield` can also output a value, or take in a value

1. Outputting a value with yield

```js
function* incrementer() {
  let i = 0

  while (true) {
    yield i++
  }
}

// Initiate the generator
const counter = incrementer()
counter.next() // 0 
counter.next() // 1
```
2. Inputting a vlue into yield
```js
function* generatorFunction() {
  console.log(yield)
  console.log(yield)

  return 'The end'
}

const generator = generatorFunction()

generator.next()
generator.next(100) // 100
generator.next(200) // 200
```

## Async flows
We can use generators to work with async code

# Modules
ES6 has introduced modules natively in JavaScript.
- Modules are singletons
- Exports are static
- Modules are file-based (one module per file);

## Named export
```js
// myModule.js
export function myExport() {
    ...
}
function notExported() { ... }
export const someStr = "...."
```
or we can
```js
// myModule.js
function myExport() {
    ...
}
function notExported() { ... }
const someStr = "...."

export {myExport, someStr};
```

We can also rename exports via
```js
export {myExport as customExport}
```

To import, you need to specify what you're importing:
```js
import {customExport} from './myModule.js'
```

Otherwise you can import it all in one go
```js
import * as myModule from './myModule.js'

myModule.customExport();
```

## Default Exports
```js
// myModule.js
export default function myExport() {...}
```
```js
import anyName from './myModule.js'
```
or
```js
// myModule.js
export {myExport as default, otherExports};
```
```js
// consumingApp.js
import anyName, {otherExports} from './myModule.js'
```

You can only have one default export in a module, but you can import it using any name.

## Aggregating modules
In a module, we can also export an external module (for making imports easier later on)
```js
export {myExport, someStr};
export {externalExport} from './anotherFile.js'
```
## Enabling Modules
```HTML
<script src="js/app.js" type="module"></script>
```