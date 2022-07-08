# Javascript

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

## Functions
### Arguments Object
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