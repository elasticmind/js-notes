# Partial application

## Higher order functions
A higher order function is a function that takes a function as an argument, or returns a function.

## Arity
Arity is the number of parameters in a function declaration.

A function with arity 1 is also called "unary", a function with arity 2 is also called "binary", a function with higher arity is called "n-ary".

A common utility function in FP is the `unary`, which takes a function as an argument and returns another that cares only about one argument:
```js
function unary(fn) {
    return function onlyOneArg(arg){
        return fn( arg );
    };
}
```

## The partial function
```js
function partial(fn, ...presetArgs) {
    return function partiallyApplied(...laterArgs){
        return fn( ...presetArgs, ...laterArgs );
    };
}
```





Partial application binds (assigns) a function’s arguments to predefined values and generates a new function of fewer arguments. The resulting function con-
tains the fixed parameters in its closure and is completely evaluated on the subsequent call.



Partial application is an operation that initializes a subset of a nonvariadic function’s
parameters to fixed values, creating a function of smaller arity.


Closures are how partial applications get their fixed arguments. A fixed argument is an argument bound in the closure scope of a returned function.

## Examples
```js
function add(x, y) {
  return x + y;
}
var add10 = partial(add, 10);
add10(2) // 12
```
By definition, the value `10` is stored in the closure the `add10` function In add(1)(2), 1 is a fixed argument in the function returned by add2(1).

## Uses


## Bibliography
+ https://github.com/getify/Functional-Light-JS/blob/master/manuscript/ch3.md/#some-now-some-later
+ Functional Programming in JavaScript - How to Improve Your JavaScript Programs Using Functional Techniques (2016) - p.98-102
