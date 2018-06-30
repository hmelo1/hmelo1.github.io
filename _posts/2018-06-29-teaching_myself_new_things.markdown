---
layout: post
title:      "Teaching Myself New Things"
date:       2018-06-29 10:44:24 -0400
permalink:  teaching_myself_new_things
---


I've always had a bit of trouble understanding 3 things in Javascript. So we'll go over them so noone makes the same mistakes I do!

1. The differences between "var", "let", and "const"
2. Closures
3. Function declarations vs function expressions.

Javascript is an ever changing language. That’s both one of its best features and one of its worst features. A few things have changed over the years, thanks to the introduction of ES5 and ES6. Originally, Javascript handled variable declarations in a weird way. Initially, the class "var" was used for declarations.

Variable declarations are essentially brought to the top of the current scope its in regardless of where you actually declared the variable. The official term for this is called **hoisting**.

To get a better idea of this the following two code examples give the same results.

```
//Example 1.
 num = 5;
 console.log(num)
 var num;
```

```
//Example 2.
var num;
num = 5;
console.log(num)
```


By default Javascript moves all declarations towards the top. So Javascript reads Example 1 exactly the same way Example 2 is written.  However, Javascript only hoists the declarations, not the initializations. The next two code examples do **NOT** give the same results.

```
//Example 1.
var num = 10;
var num2 = 15;  
console.log(num + num2);
```

```
//example 2.
var num = 10;
console.log(num + num2); //Error message that num2 is Not a number.
var num2 = 15;
```

Example 2 will spit out a undefined error. Due to num2 existing but not having a definition. This one's a bit easier to see in the next example:

```
//Rewritten Example2:
var num = 10;
var num2;
console.log(num + num2); //num2 is NaN
num2 = 15;
```

Now that we understand the basics of hoisting and variable declarations we can add a few more to keywords to our toolbox. Let and const are the two new toolbox additions added in from ES6. 

I've found it easiest to stick with 3 simple rules:
* 1. Use const by default.
* 2. Use let if you have to rebind a variable.
* 3. use var to signal untouched legacy code.
* 
(Thanks, https://twitter.com/raganwald/status/564792624934961152) 

**'let'**, essentially "overrides" our need for var. It allows the variable to be reassigned (similar to var) and the variable can only be used within the block(and sub-blocks) its defined.

Our old friend, **'var'** is the weakest way to identify a variable. It allows the variable to be reassigned, however, the scope of the 'var' is the entire enclosed function.

Quick example to show the differences:
```
function showVar() {
  var num1 = 1;
  if (true) {
    var num1 = 2;  // same variable!
    console.log(num1);  // 2
  }
  console.log(num1);  // 2
}
```

```
function letTest() {
  let num1 = 1;
  if (true) {
    let num1 = 2;  // different variable
    console.log(num1);  // 2
  }
  console.log(num1);  // 1
}
```

Our good friend **'const'**, signals that the variable can't be reassigned. However, it can be changed within another block.
Example:
```
function showVar() {
  const num1 = 1;
  if (true) {
    const num1 = 2;  // different variable!
    console.log(num1);  // 2
  }
  console.log(num1);  // 1
}
```

```
function showVar() {
  	const num1 = 1;
    
	const num1 = 2;  // same variable!
    
	console.log(num1);  // error num1 has already been declared!
}
```

As long as you're within the same scope as the already declared const, it can't be changed. On the topic of scope. Javascript variables can belong to the **“local”** or **“global”** scope. Global variables can be made local with a term called closure. Consider the two following pieces of cope.

```
function addNumbers(){
Var a = 4;
Var b = 5;
Return a + b 
}
```

All the variables are defined inside the function. So they’re local or private to the scope of addNumbers(). However, if we declare the variables outside.

```
Var a = 4;
Var b = 5;
Function addNumbers(){
	Return a + b;
}
```
The variables are declared outside of the function and available in the global scope. However, variables have a pretty short lifespan. They’re created when a function is invoked and deleted when the function finishes running. Consider the few following code examples.

```
Var num = 4;

Function multiplyFour(){
	num *= 4;
}

multiplyFour()
multiplyFour()
//the variable a should be 64
```

The problem arises that any following function can change the num variable without calling multiplyFour.

We can try putting the declaration and the initialization into the function.

```
Function addFour(){
	Var num;
	Num +=4;
	Return num
}
addFour()
addFour()
//num should be = to 4. But nope, its 4.
```
However, this won’t work because the num is getting reset each time we call the function.

Global variables are available to all the functions within it. This means local variables are available to all the variables within the function. We can try using a nested function (a function within a function).

```
Function addFour(){
	Var num = 0;
	Function addThem(){
	num +=4}
	addThem();
	Return num;
}
```

No luck again. We have no way to reach the addThem() function and every time we run addFour() num is reset.

We’ll need to use self-invoking functions, closures and a function expression(more on this later)  to get our results.
```
var addFour = function(){
	var num = 0;
	return function() {
		num+=4; 
		return num}
}()

addFour();
addFour()
//num is now 8!
```

Now this one’s a bit of a doozy. So let’s explain it, we declare the function as an expression (a bit more of this if you scroll down). The self-invoked function only runs once (So it doesn’t reset our num each time it runs), sets the num to 0, and returns a function expression. The returned function has access to the num but the global scope doesn’t! This is called a **closure**, where a function has access to the parent scope, even after the parent function has finished running. 


Now you might be asking, “What’s a function expression?!” Javascript has two ways to create a function. 

```
Function Declartions:
function funcDeclarations() {
    return 'As a declaration';
}
```

```
Function Expressions
var funcExpressions = function () {
    return 'As an expression';
}
```

Normally these can be used interchangeably. 

Just to bring back an old friend. Javascript's default behavior is to "hoist" the declaration to the top of the scope.
Because of this, Javascript declaration functions can be called before they're declared.
```
funcDeclarations(); // returns "As a declaration"

function funcDeclarations() {
    return 'As a declaration';
}
```

However, functions defined using an expression is not hoisted.
funcExpressions(); //returns "funcExpressions is not a function"

```
var funcExpressions = function () { // returns "As an expression"
    return 'As an expression';
}
```
Function expressions give us access to the nifty little closures from before.

They're also needed when trying to use a function as arguments for another function. Such as:

```
var addFour = function(){
	var num = 0;
	return function() {
		num+=4; 
		return num
	}
}()

console.log(addFour())
```

Or when incorporating Immediately Invoked Function Expressions (IIFE) or self-invoked functions. Same as the above function.

