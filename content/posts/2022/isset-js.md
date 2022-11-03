---
title: Isset Equivalent in Javascript
date: 2022-10-25
image: /assets/images/2022/isset_php.png
---

## Isset in PHP

The isset() function in PHP checks whether a variable is set, which means that it has to be declared and is not NULL. This function returns true if the variable exists and is not NULL, otherwise, it returns false.

```php
// Referencing an undeclared variable
echo json_encode(isset($foo)); // false
$foo = 'bar';

// Declared but has no depth(not an array)
echo json_encode(isset($foo)); // true
echo json_encode(isset($foo['nested'])); // false

$foo = ['nested' => 'bar'];

// Declared as an array but not with the depth we're testing for
echo json_encode(isset($foo['nested'])); // true

```
Also, you can reference any variable at any depth - even trying to access a non-array as an array will return a simple true or false.

```php
$foo = ['nested' => 'bar'];

// works even though deeper is not defined
echo json_encode(isset($foo['nested']['deeper'])); // false

// works for any depth
echo json_encode(isset($foo['nested']['deeper']['more_deeper'])); // false
```

Note: I am wrapping the function isset with json_encode to print the boolean value.

## Problem in Javascript

Unfortunately, We don’t have that freedom in Javascript. We don't have a built-in isset function. Let's try to implement isset function which will check given variable or nested property of an object is set. We will create a simple function isset to mimic that of PHP.

```js
function isset (ref) { return typeof ref !== 'undefined' }
```

Let's use isset function we defined to check whether a variable or object's property is defined.

```js
// a simple object with no properties 
let foo = {}

// checking if we have a declared variable, isset works fine
isset(foo) // true

// checking if we have a top level property, still working
isset(foo.nested) // false
```

### The Problem

```js
// trying to access deep property
isset(foo.nested.deeper) // Error: Cannot read property 'deeper' of undefined
//         ^^^^^^ undefined
```

The problem is when you are trying to access the value of the nested property,
JS Engine is attempting to access it before we wrap it into our isset function. So, It is throwing cannot read property Error.

## Solution

To fix this we can use Optional Chaining but not all JS versions and browsers support Optional Chaining. Alternatively, we will implement isset with a different approach too. Below we see how we can implement isset equivalent in JS.

### 1. Using Optional Chaining

The new feature in JS known as [Optional Chaining](https://javascript.info/optional-chaining) eliminates the problem our isset function is facing. Optional chaining was introduced in ES2020. According to TC39 it is currently at stage 4 of the proposal process and is prepared for inclusion in the final ECMAScript standard. This means that you can use it, but note that older browsers may still require polyfill usage. 

The optional chaining ?. allows a safe way to access nested object properties, even if an intermediate property doesn’t exist. We can implement it in our code so that our isset function works perfectly.

We can use isset function defined earlier with optional chaining.

```js
function isset (ref) { return typeof ref !== 'undefined' }

// trying to access deep property
isset(foo?.nested?.deeper) // prints false

//trying to access very deep property with optional chaining
isset(foo?.nested?.deeper?.deeeper?.deeeeeper) // prints false
// 
```

### 2. Wrapping nested property in a Function

Different Javascript versions and browsers might not support Optional Chaining. The function we created might not be useful for all versions of javascript. So, we will create another isset function that takes a function as an argument.

```js
/**
 * Checks to see if a value is set.
 *
 * @param   {Function} accessor Function that returns our value
 * @returns {Boolean}           Value is not undefined or null
 */
function isset (accessor) {
  try {
    // Note we're seeing if the returned value of our function is not
    // undefined or null
    return accessor() !== undefined && accessor() !== null
  } catch (e) {
    // And we're able to catch the Error it would normally throw for
    // referencing a property of undefined
    return false
  }
}

isset(() => foo) // false

// Defining objects
let foo = { nested: { value: 'hello' } }

// More tests that never throw an error
isset(() => foo)) // true
isset(() => foo.nested.value) // true
isset(() => foo.nested.deeper.value)) // false

// without using arrow function
isset(function () { return some.nested.deeper.value }) // false
```

In this approach, we made use of try-catch to resolve the Error. We wrapped the nested property that we wanted to check in a function as a return value. If the return value is not undefined or null we return true otherwise false. Also, we have a catch block that returns false whenever there is `Cannot read property Error`.

## Conclusion

We looked into how isset works in PHP having the ability to reference any nested property in any depth. We analyzed the problem in JS and we implemented a working isset function equivalent to that in PHP.

You can make use of the utility function that we created and start using isset function in your code.

