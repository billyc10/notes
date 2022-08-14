# Javascript

[[Front End]]

## Variables
### Scoping
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

## `...` Operator
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
