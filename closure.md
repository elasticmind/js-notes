# Closure

## Definitions
> Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

> A closure is a data structure that binds a function to its environment at the moment it’s declared.

> Closure is the bundling of a function with its lexical environment.

Closures are created at function creation time. When a function is defined inside another function, it has access to the variable bindings in the outer function, even after the outer function exits.

## Example
Consider:
```js
function outer(argument1, argument2) {
	var outerVar1 = 'outerVarContent1';
	var outerVar2 = 'outerVarContent2';

	function inner() {
		var innerVar1 = 'innerVarContent1';
		var innerVar2 = 'innerVarContent2';
		console.log(argument1);
		console.log(innerVar1);
		console.log(outerVar1);
	}

	return inner;
}

var returned = outer('argument1Value', 'argument2Value');
returned(); // argument1Value
            // innerVarContent1
            // outerVarContent1
```
Exploring `returned`, the console shows how `outerVar` is remembered:
```js
ƒ inner()
  arguments: null
  caller: null
  length: 0
  name: "inner"
  prototype: {constructor: ƒ}
  __proto__: ƒ ()
  [[FunctionLocation]]: VM186:4
  [[Scopes]]: Scopes[2]
    0: Closure (outer)
      argument1: "argument1Value"
      outerVar1: "outerVarContent1"
    1: Global {...}
```
Closed over variables cannot be reassigned.

## Uses
### Remember
```js
function makeCounter(start = 0) {
	var count = start;

	return function () {
		console.log(`count is ${count++}`);
	}
}

var counter1 = makeCounter();
counter1(); // count is 0
counter1(); // count is 1
counter1(); // count is 2
var counter2 = makeCounter(10);
counter2(); // count is 10
counter2(); // count is 11
```
### Module pattern
The module pattern can be used to emulate privacy, through an IIFE (Immediately Invoked Functional Expression).
```js
var api = (function () {
	var innerVar = 'innerVarInitialContent';

	return {
		setInner(newValue) {
			innerVar = newValue;
		},
		logInner() {
			console.log(innerVar);
		}
	};
})();

api.logInner(); // innerVarInitialContent
api.setInner('innerVarNewContent');
api.logInner(); // innerVarNewContent
```
The variable `innerVar` is not accessable directly, however, `setInner` and `logInner` both close over it.

### Block scope emulation
Consider:
```js
for (var i = 1; i <= 5; i++) {
	setTimeout(function log() {
		console.log(i);
	}, i*1000);
}
```
The previous snippet logs the value `6` every second for 5 seconds. This happens because all 5 `log` functions close over the same `i` variable and its value is `6` by the time these functions are invoked. To solve this, we can implement an IIFE inside the for loop, and provide different `i` variables for the functions to close over:
```js
for (var i = 1; i <= 5; i++) {
	(function (j) {
        setTimeout(function log() {
            console.log(j);
        }, j*1000);
    })(i);
}
```
This works as intended.
> Note: The `let` and `const` keywords provide this functionality.

## Bibliography
+ https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md
+ Functional Programming in JavaScript - How to Improve Your JavaScript Programs Using Functional Techniques (2016) - p.45-53
