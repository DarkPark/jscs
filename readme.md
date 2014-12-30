# JavaScript Code Style

This is a guide for writing consistent and aesthetically pleasing JavaScript code.
It is inspired by what is popular within the community, and flavored with some personal opinions.

## <a name='TOC'>Table of Contents</a>

  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Conditional Expressions & Equality](#conditionals)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespaces](#whitespaces)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [DOM](#dom)
  1. [Modules](#modules)
  1. [Linting](#linting)
  1. [Commits](#commits)
  1. [Resources](#resources)


## <a name='objects'>Objects</a>

Use the literal syntax for object creation.
> Both ways do the same thing, but literal notation takes less space. It's clearly recognizable as to what is happening, so using `new object()`, is really just typing more.

```js
// bad
var item = new Object();
```

```js
// good
var item = {};
```

Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys.
> It won't work in [IE8](https://github.com/airbnb/javascript/issues/61).

```js
// bad
var superman = {
	default: {clark: 'kent'},
	private: true
};
```

```js
// good
var superman = {
	defaults: {clark: 'kent'},
	hidden: true
};
```

Use readable synonyms in place of reserved words.

```js
// bad
var superman = {
	klass: 'alien'
};
```

```js
// good
var superman = {
	type: 'alien'
};
```

`for-in` loop should be used only for iterating over keys in an object/map/hash.
> Such loops are often incorrectly used to loop over the elements in an Array. This is however very error prone because it does not loop from 0 to length - 1 but over all the present keys in the object and its prototype chain.

```js
// only for objects (not arrays)
for ( var key in obj ) {
	if ( obj.hasOwnProperty(key) ) {
		console.log(obj[key]);
	}
}
```

Modifying prototypes of builtin objects should be avoided.
> Modifying builtins like `Object.prototype` and `Array.prototype` are forbidden. Modifying other builtins like `Function.prototype` is less dangerous but still leads to hard to debug issues in production and should be avoided.


## <a name='arrays'>Arrays</a>

Use the literal syntax for array creation.
> `new Array(len)` creates an array with len holes. On some JavaScript engines, this operation lets you pre-allocate arrays, giving you performance benefits for small arrays (not for large ones). In most cases, performance doesn’t matter and you can avoid the redundancy introduced by a preallocation. If it fits the problem, an array literal initializing all elements at once is preferable.

```js
// bad
var items = new Array();
```

```js
// good
var items = [];
```

To append some value to array use [Array.push](http://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push).

```js
// bad
items[items.length] = 'abracadabra';
```

```js
// good
itmes.push('abracadabra');
```

When you need to copy an array use [Array.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice).

```js
itemsCopy = items.slice();
```

To convert an [array-like](http://www.2ality.com/2013/05/quirk-array-like-objects.html) object to an array, use it as well.

```js
function trigger() {
	var args = Array.prototype.slice.call(arguments);
	// ...
}
```

Use simple iteration for big data. `forEach` approach looks better but good only for small arrays.
> Anonymous iteration function call has a small cost but multiplied by millions gives bad results.

```js
// good for large arrays
var i, l = arr.length;
for ( i = 0; i < l; i++ ) {
	console.log(arr[i]);
}

// good for small arrays
arr.forEach(function ( item ) {
	console.log(arr[i]);
});
```


## <a name='strings'>Strings</a>

Use single quotes `''` for strings.
> The difference between single and double quotes is that you don't need to escape single quotes in double quotes, or double quotes in single quotes. That is the only difference, if you do not count the fact that you must hold the Shift key to type `"`.

```js
// bad
var name = "Bob Parr",
	fullName = "Bob " + this.lastName;
```

```js
// good
var name = 'Bob Parr',
	fullName = 'Bob ' + this.lastName;
```

Strings longer than 100 characters should be written across multiple lines using string concatenation.
> In general use of such long lines should be avoided. If overused, long strings with concatenation could impact performance.
> [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

```js
// good
var errorMessage = 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, \
sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. \
Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris.';

// also good
var errorMessage = 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, ' +
'sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ' +
'Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris.';
```

When programatically building up complicated strings, use [Array.join](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join). String concatenation is good for simple cases.
> Modern browsers are optimized to do string concatenations so performance is [not an issue](http://stackoverflow.com/questions/7299010/why-is-string-concatenation-faster-than-array-join) anymore.

```js
// good
var i, str = "";
for ( i = 30000; i > 0; i-- ) {
	str += 'String concatenation. ';
}

// also good
var i, str = '', sArr = [];
for ( i = 30000; i > 0; i-- ) {
	sArr.push('String concatenation.');
}
str = sArr.join(' ');
```


## <a name='functions'>Functions</a>

Function expressions:

```js
// anonymous function expression
var anonymous = function() {
	return true;
};

// named function expression
var named = function named() {
	return true;
};

// immediately-invoked function expression (IIFE)
(function() {
	return true;
})();
```

Never declare a function in a non-function block (`if`, `while`, etc).
> Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently.

```js
// bad
if ( currentUser ) {
	function test () {
		console.log('Nope.');
	}
}

// good
var test;
if ( currentUser ) {
	test = function test () {
		console.log('Yup.');
	};
}
```

Never name a parameter `arguments`.
> This will take precedence over the `arguments` object that is given to every function scope.

```js
// bad
function nope ( name, options, arguments ) {
	// ...
}

// good
function yup ( name, options, args ) {
	// ...
}
```

When possible, all function arguments should be listed on the same line. If doing so would exceed the 100-column limit, the arguments must be line-wrapped in a readable way. To save space, you may wrap as close to 100 as possible, or put each argument on its own line to enhance readability. The indentation may be either one tab, or aligned to the parenthesis.

```js
// works with very long function names, survives renaming without reindenting
some.name.space.withSomeVeryLongFunctionName = function (
	veryLongArg1, veryLongArg2,
	veryLongArg3, veryLongArg4 ) {
	// ...
};

// one argument per line, survives renaming and emphasizes each argument
some.name.space.withSomeVeryLongFunctionName = function (
	veryLongArg1,
	veryLongArg2,
	veryLongArg3,
	veryLongArg4 ) {
	// ...
};

// visually groups arguments
function test ( veryLongArg1, veryLongArg2,
				veryLongArg3, veryLongArg4 ) {
	// ...
}

// parenthesis-aligned, one emphasized argument per line
function test ( veryLongArg1,
				veryLongArg2,
				veryLongArg3,
				veryLongArg4 ) {
	// ...
}
```

A function should have a singular purpose that is easily defined.
Its size can vary but try to keep it in about 30-50 lines.


## <a name='properties'>Properties</a>

Use dot notation when accessing properties wherever possible.
> Dot notation is faster to write and clearer to read.
> Square bracket notation allows access to properties containing special characters and selection of properties using variables.

```js
// bad
var data = items['pear'];
```

```js
// good
var data = items.pear;

// also good
var name = 'pear',
	data = items[name];
```

Delete properties with assigning to `null`.
> In modern JavaScript engines, changing the number of properties on an object is much slower than reassigning the values. The delete keyword should be avoided except when it is necessary to remove a property from an object's iterated list of keys, or to change the result of `if ( key in obj )`.

```js
// think twice
Foo.prototype.dispose = function () {
	delete this.someProperty;
};
```

```js
// good in most cases
Foo.prototype.dispose = function () {
	this.someProperty = null;
};
```


## <a name='variables'>Variables</a>

Always use `var` to declare variables.
> When you fail to specify var, the variable gets placed in the global context, potentially clobbering existing values. Also, if there's no declaration, it's hard to tell in what scope a variable lives (e.g., it could be in the Document or Window just as easily as in the local scope). So always declare with var.

```js
// bad
superPower = new SuperPower();
```

```js
// good
var superPower = new SuperPower();
```

Use one `var` declaration for multiple variables in each scope.  

```js
// bad
var items   = getItems();
var isFresh = true;
var topHtml = 'z';
```

```js
// good
var items   = getItems(),
	isFresh = true,
	topHtml = 'z';
```

Declare unassigned variables last.
> This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

```js
// bad
var i, len,
	items = getItems(),
	goSportsTeam = true,
	dragonball;
```

```js
// good
var items = getItems(),
	goSportsTeam = true,
	dragonball, length, i;
```

Group your vars by meaning and try to align them.
```js
var items = [],
	goods = {},
	maps  = null,
	topMarks  = [5,6,7],
	topGrades = [56,57,58], 
	topCars   = null,
	// all best categories
	bestBooks, bestFilms, bestSongs,
	// everything about customer
	customerTickets, customerLogin, customerLinks;
```

Assign variables at the top of their scope.
> This helps avoid issues with variable declaration and assignment hoisting related issues.

```js
// bad
function nope () {
	test();

	// ...

	var name = getName();

	if ( name === 'test' ) {
		return false;
	}

	return name;
}
```

```js
// good
function yep () {
	var name = getName();

	test();

	// ...

	if ( name === 'test' ) {
		return false;
	}

	return name;
}
```

```js
// bad
function nope () {
	var name = getName();

	if ( !arguments.length ) {
		return false;
	}

	// ...

	return true;
}
```

```js
// good
function yep () {
	var name;
	
	if ( !arguments.length ) {
		return false;
	}

	name = getName();

	// ...

	return true;
}
```

In complex cases use JavaScript native approach.
> Variable and function declarations get hoisted to the top of their scope, their assignment does not.  
> For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)

```js
// bad
function nope ( isReady ) {
	if ( !isReady ) return;
	
	var name = getName(),
		a, b, c;

	// ...
}

// also bad
function nope ( isReady ) {
	var name = getName(),
		a, b, c;

	if ( !isReady ) return;

	// ...
}
```

```js
// good
function yep ( isReady ) {
	var name, a, b, c;

	if ( !isReady ) return;
	
	name = getName();

	// ...
}
```


## <a name='conditionals'>Conditional Expressions & Equality</a>

Use `===` and `!==` over `==` and `!=`.
> This helps to maintain data type integrity throughout your code and has a better performance.  
> For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108).

Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

+ `Objects` evaluate to `true`
+ `Undefined` evaluates to `false`
+ `Null` evaluates to `false`
+ `Booleans` evaluate to the value of the boolean
+ `Numbers` evaluate to `false` if `+0`, `-0`, or `NaN`, otherwise `true`
+ `Strings` evaluate to `false` if an empty string `''`, otherwise `true`

Use shortcuts.

```js
// bad
if ( name !== '' && collection.length > 0 ) {
	// ...
}
```

```js
// good
if ( name && collection.length ) {
	// ...
}
```

Use simple single `if` for complex expressions.

```js
// bad
if ( a === 1 ) {
	if ( b === 2 ) {
		if ( a1 === 1 ) {
			if ( b1 === 2 ) {
				c = 1;
			}
		}
	}
}

// also bad
(a === 1) && (b === 2) && (a1 === 1) && (b1 === 2) && (c = 1);
```

```js
// good
if ( a === 1 && b === 2 && a1 === 1 && b1 === 2 ) {
	c = 1;
}
```


## <a name='blocks'>Blocks</a>

Always use braces with all blocks.

```js
// bad
if ( isReady )
	return true;

// bad
if ( isReady ) return true;
```

```js
// good
if ( isReady ) {
	return true;
}

// good
if ( isReady ) { return true; }
```

Because of implicit semicolon insertion, always start your curly braces on the same line as whatever they're opening.

```js
// bad
if ( something )
{
	// ...
}
else
{
	// ...
}
```

```js
// good
if ( something ) {
	// ...
} else {
	// ...
}
```

`with ( someVar ) { ... }` should be avoided.
> Using with clouds the semantics of your program. Because the object of the with can have properties that collide with local variables, it can drastically change the meaning of your program.


## <a name='comments'>Comments</a>

Use `//` (with a trailing space) for both single line and multi line comments. Try to write comments that explain higher level mechanisms or clarify difficult segments of your code. Don't use comments to restate trivial things.

```js
//bad

/////// also bad
```

```js
// good

// good
// when there is a lot to comment

// also good
var active  = true,   // this var description
	single  = false,  // another var description
	isReady = false;  // and one more
```

For var/function/class declarations [JSDoc](http://usejsdoc.org/) should be used.

Use `//FIXME:` to annotate problems.
> Helps other developers quickly understand if you're pointing out a problem that needs to be revisited.
```js
function build () {
	//FIXME: shouldn't use a global here
	total = 0;
}
```

Use `//TODO:` to annotate solutions to problems.
> Helps other developers to see if you're suggesting a solution to the problem that needs to be implemented.
```js
function build () {
	//TODO: total should be configurable by an options param
	this.total = 0;
}
```


## <a name='whitespaces'>Whitespaces</a>

Use one single tab character for one-level indentation.

```js
// bad
function test () {
∙∙∙∙var name;
}

// bad
function test () {
∙∙var name;
}
```

```js
// good
function test () {
	var name;
}
```

No whitespace at the end of line or on blank lines.

Add spaces between operators (`=`, `>`, `*`, etc).

```js
// bad
if ( a>5 && b*8<345 ) { c=true; }
```

```js
// good
if ( a > 5 && b * 8 < 345 ) { c = true; }
```

Unary special-character operators (e.g., `!`, `++`) must not have space next to their operand.

```js
// bad
if ( ! isReady ) { ... }
```

```js
// good
if ( !isReady ) { ... }
```

Any `,` and `;` must not have preceding space.

```js
// bad
someCall(a , b , c) ;
```

```js
// good
someCall(a, b, c);
```

Any `;` used as a statement terminator must be at the end of the line.

Any `:` after a property name in an object definition must not have preceding space.

```js
// bad
var obj = {a : 1, b : 2, c : 3};
```

```js
// good
var obj = {a: 1, b: 2, c: 3};
```

The `?` and `:` in a ternary conditional must have space on both sides.

```js
// bad
var flag = isReady?1:0;
```

```js
// good
var flag = isReady ? 1 : 0;

// also good for long expressions
var flag = isReady
	? someVeryLongEvaluatedOrSomethingElseValue
	: anotherVeryLongEvaluatedOrSomethingElseValue;
```

No spaces in empty constructs like `{}`, `[]`.

Place one space before the leading brace.

```js
// bad
function test(){
	// ...
}

// bad
dog.set('attr',{
	age: '1 year',
	breed: 'Bernese Mountain Dog'
});
```

```js
// good
function test () {
	// ...
}

// good
dog.set('attr', {
	age: '1 year',
	breed: 'Bernese Mountain Dog'
});
```

Separate function name and its parameters in declaration.

```js
// bad
function test(){
	// ...
}

// bad
function test(a, b){
	// ...
}
```

```js
// good
function test ( a, b ) {
	// ...
}

// but no leading/trailing spaces on execution
test(128, 'qwe');
```

The same for `if`, `if/else`, `switch`, `try/catch` blocks.

```js
// bad
if(isReady){
	// ...
}

// bad
try{
	// ...
}catch(e){
	// error handling
}
```

```js
// good
if ( isReady ) {
	// ...
}

// good
try {
	// ...
} catch ( e ) {
	// error handling
}
```

Place an empty newline at the end of a file.
> Each line should be terminated with a newline character, including the last one. Some programs have problems processing the last line of a file if it isn't newline terminated.

Use line breaks and indentation when making long method chains.

```js
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();
```

```js
// good
$('#items')
	.find('.selected')
		.highlight()
		.end()
	.find('.open')
		.updateCount();
```

Single-line array and object initializers are allowed when they fit on a line.

```js
var arr = [1, 2, 3];           // No space after [ or before ].
var obj = {a: 1, b: 2, c: 3};  // No space after { or before }.
```

Use newlines to group logically related pieces of code.

```js
doSomethingTo(x);
doSomethingElseTo(x);
andThen(x);

nowDoSomethingWith(y);

andNowWith(z);
```

## <a name='commas'>Commas</a>

No Leading commas.

```js
// bad
var once
	, upon
	, aTime;

// bad
var hero = {
	firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};
```

```js
// good
var once,
	upon,
	aTime;

// good
var hero = {
	firstName:  'Bob',
	lastName:   'Parr',
	heroName:   'Mr. Incredible',
	superPower: 'strength'
};
```

No Additional trailing comma.
> This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in [ES5](http://es5.github.io/#D).

```js
// bad
var hero = {
	firstName: 'Kevin',
	lastName: 'Flynn',
};

// bad
var heroes = [
	'Batman',
	'Superman',
];
```

```js
// good
var hero = {
	firstName: 'Kevin',
	lastName: 'Flynn'
};

// good
var heroes = [
	'Batman',
	'Superman'
];
```


## <a name='semicolons'>Semicolons</a>

Semicolons use is mandatory.
> Relying on [ASI](http://es5.github.io/#x7.9) (Automatic Semicolon Insertion) can cause subtle, hard to debug [problems](http://benalman.com/news/2013/01/advice-javascript-semicolon-haters/).

```js
// bad
a = b + c
test()

// bad
(function () {
	var name = 'Skywalker'
	return name
})()
```

```js
// good
a = b + c;
test();

// good
(function() {
	var name = 'Skywalker';
	return name;
})();
```

Semicolons should be included at the end of function expressions, but not at the end of function declarations.

```js
var foo = function () {
	return true;
};  // semicolon here

function foo () {
	return true;
}  // no semicolon here
```


## <a name='type-coercion'>Type Casting & Coercion</a>

Perform type coercion at the beginning of the statement.

```js
// bad
var totalScore = intValue + '';

// bad
var totalScore = '' + intValue + ' total score';
```

```js
// good
var totalScore = '' + intValue;

// good
var totalScore = intValue + ' total score';
```

Use `parseInt` for Numbers and always with a radix for type casting.

```js
var inputValue = '4';

// bad
var val = new Number(inputValue);

// bad
var val = +inputValue;

// bad
var val = inputValue >> 0;

// bad
var val = parseInt(inputValue);
```

```js
// good
var val = Number(inputValue);

// good
var val = parseInt(inputValue, 10);
```

Booleans.

```js
var age = 0;

// bad
var hasAge = new Boolean(age);
```

```js
// good
var hasAge = Boolean(age);

// good
var hasAge = !!age;
```


## <a name='naming-conventions'>Naming Conventions</a>

Avoid single letter names. Be descriptive with your naming.
The only exception is well-known index variables `i`, `j` and similar.

```js
// bad
function q () {
	// ...
}
```

```js
// good
function query () {
	// ...
}
```

Use [lowerCamelCase](http://en.wikipedia.org/wiki/CamelCase) when naming objects, functions, and instances.

```js
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
function c() {};
var u = new user({
	name: 'Bob Parr'
});
```

```js
// good
var thisIsMyObject = {},
	user = new User({
		name: 'Bob Parr'
	});

function thisIsMyFunction() {
	// ...
};
```

Use PascalCase when naming constructors or classes.

```js
// bad
function user ( options ) {
	this.name = options.name;
}

var bad = new user({
	name: 'nope'
});
```

```js
// good
function User ( options ) {
	this.name = options.name;
}

var good = new User({
	name: 'yup'
});
```

If a value is intended to be constant and immutable, it should be given a name in `CONSTANT_VALUE_CASE`.

```js
var SECOND = 1 * 1000;
this.TIMEOUT_IN_MILLISECONDS = 60;
```

Use a leading underscore `_` when naming private properties.

```js
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
```

```js
// good
this._firstName = 'Panda';
```

When saving a reference to `this` use `self` or a corresponding meaningful name.

```js
// bad
function () {
	var that = this;
	return function () {
		console.log(that);
	};
}
```

```js
// good
function () {
	var self = this;
	return function () {
		console.log(self);
	};
}

// also good
function () {
	var parentFunc = this;
	return function () {
		console.log(parentFunc);
	};
}
```

Name your functions.
> This will produce better stack traces, heap and cpu profiles.

```js
// bad
var log = function ( msg ) {
	console.log(msg);
};
```

```js
// good
var log = function log ( msg ) {
	console.log(msg);
};
```

Use leading `$` to indicate that a DOM element is assigned to this variable.

```js
this.$body = document.querySelector('div.page');
```

Use dot as a separator for filenames. Only lowercase allowed in order to avoid confusion on case-sensitive platforms. Filenames should contain no punctuation.

```js
// bad
modelmain.js
ModelMain.js
model_main.js
model-main.js
model main.js

// good
model.main.js
```

## <a name='accessors'>Accessors</a>

Getters and setters methods for properties are not required. If you do make accessor functions use `getVal()` and `setVal('hello')`. It’s okay to create `get()` and `set()` functions, but be consistent.

```js
// bad
dragon.age();    // getter
dragon.age(25);  // setter
```

```js
// good
dragon.getAge();
dragon.setAge(25);
```

If the property is a boolean, use `isVal()` or `hasVal()`.

```js
// bad
if ( !dragon.age() ) {
	return false;
}
```

```js
// good
if ( !dragon.hasAge() ) {
	return false;
}
```


## <a name='constructors'>Constructors</a>

Assign methods to the prototype object, instead of overwriting the prototype with a new object.
> Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

```js
function Jedi () {
	console.log('new jedi');
}

// bad
Jedi.prototype = {
	fight: function fight () {
		console.log('fighting');
	},

	block: function block () {
		console.log('blocking');
	}
};
```

```js
// good
Jedi.prototype.fight = function fight () {
	console.log('fighting');
};

Jedi.prototype.block = function block () {
	console.log('blocking');
};
```

Methods can return `this` to help with method chaining.

```js
// bad
Jedi.prototype.jump = function () {
	this.jumping = true;
	return true;
};

Jedi.prototype.setHeight = function ( height ) {
	this.height = height;
};

var luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20) // => undefined
```

```js
// good
Jedi.prototype.jump = function () {
	this.jumping = true;
	return this;
};

Jedi.prototype.setHeight = function ( height ) {
	this.height = height;
	return this;
};

var luke = new Jedi();

luke.jump()
	.setHeight(20);
```


It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

```js
function Jedi ( options ) {
	options = options || {};
	this.name = options.name || 'no name';
}

Jedi.prototype.getName = function getName () {
	return this.name;
};

Jedi.prototype.toString = function toString () {
	return 'Jedi - ' + this.getName();
};
```


## <a name='events'>Events</a>

When attaching data to events it's always should be only one parameter.
Pass a hash instead of a raw value in case there are more then one parameter.
> This approach allows `EventEmitter` implementation with better performance.

```js
// bad
button.trigger('click', data1, data2, data3);

button.on('click', function ( data1, data2, data3 ) {
	// do something with arguments
});
```

```js
// good
button.trigger('click', value);

button.on('click', function ( value ) {
	// do something with value
});
```

```js
// good
button.trigger('click', {val1: data1, val2: data2, val3: data3});

button.on('click', function ( data ) {
	// do something with data.val1, data.val2 and data.val3
});
```


## <a name='dom'>DOM</a>

`Element.nodeName` must always be used in favor of `Element.tagName`.

`Element.nodeType` must be used to determine the classification of a node (not `Element.nodeName`).

**Iterating over Node Lists.**
> Node lists are often implemented as node iterators with a filter. This means that getting a property like length is O(n), and iterating over the list by re-checking the length will be O(n^2).

```js
// not this good
var paragraphs = document.getElementsByTagName('p');
for ( var i = 0; i < paragraphs.length; i++ ) {
	doSomething(paragraphs[i]);
}
```

This works well for all collections and arrays as long as the array does not contain things that are treated as boolean false.

```js
var paragraphs = document.getElementsByTagName('p');
for ( var i = 0, paragraph; paragraph = paragraphs[i]; i++ ) {
	doSomething(paragraph);
}
```

In cases where you are iterating over the `childNodes` you can also use the `firstChild` and `nextSibling` properties.

```js
var parentNode = document.getElementById('foo');
for ( var child = parentNode.firstChild; child; child = child.nextSibling ) {
	doSomething(child);
}
```


## <a name='modules'>Modules</a>

Use [CommonJS](http://wiki.commonjs.org/wiki/Modules) modules.
[Browserify](http://browserify.org/), [Webmake](https://github.com/medikoo/modules-webmake) or similar are advised to bundle modules for web browsers.

`eval()` is only for code loaders and in general should be avoided.
> It makes for confusing semantics and is dangerous to use if the string being eval()'d contains user input. There's usually a better, clearer, and safer way to write your code, so its use is generally not permitted.


## <a name='linting'>Linting</a>

Use [JSHint](http://www.jshint.com/) to detect errors and potential problems.

The options for JSHint are stored in a .jshintrc file.

```json
{
	"indent"      : 4,
	"bitwise"     : true,
	"camelcase"   : true,
	"curly"       : true,
	"eqeqeq"      : true,
	"forin"       : true,
	"freeze"      : true,
	"immed"       : true,
	"latedef"     : "nofunc",
	"newcap"      : true,
	"noarg"       : true,
	"noempty"     : true,
	"nonew"       : true,
	"undef"       : true,
	"unused"      : true,
	"globalstrict": true,
	"trailing"    : true,
	"quotmark"    : "single",
	"browser"     : true,
	"globals"     : {
		"console" : false
	}
}
```


## <a name='commits'>Commits</a>

Every commit message, pull request title or issue discussion must be in English.
> English is the universal language nowadays, if you use any other language you're excluding potencial contributors.

Don't use Past or Present Continuous tenses for commit messages, those should be in Imperative Present tense.
> This preference comes from [Git itself](http://git.kernel.org/cgit/git/git.git/tree/Documentation/SubmittingPatches?id=HEAD#n111).

```js
// bad
Added feature X
Adding feature X
```

```js
// good
Add feature X
Fix problem Y
```

Provide a brief description of the change in the first line.
Insert a single blank line after the first line.
Provide a detailed description of the change in the following lines, breaking paragraphs where needed.
Put the 'Change-id', 'Closes-Bug #NNNNN' and similar lines at the very end.
[More info](https://wiki.openstack.org/wiki/GitCommitMessages).

```
switch libvirt get_cpu_info method over to use config APIs

The get_cpu_info method in the libvirt driver currently uses
XPath queries to extract information from the capabilities
XML document. Switch this over to use the new config class
LibvirtConfigCaps. Also provide a test case to validate
the data being returned.

Closes-Bug: #1003373
Implements: blueprint libvirt-xml-cpu-model
Change-Id: I4946a16d27f712ae2adf8441ce78e6c0bb0bb657
```

All text files line endings are [LF](https://help.github.com/articles/dealing-with-line-endings).

on Linux and OS X

```shell
# for a project
git config core.eol lf
git config core.autocrlf input

# or system-wide
git config --global core.eol lf
git config --global core.autocrlf input
```

on Windows

```shell
# for a project
git config core.eol lf
git config core.autocrlf true

# or system-wide
git config --global core.eol lf
git config --global core.autocrlf true
```


## <a name='resources'>Resources</a>

* [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [jQuery JavaScript Style Guide](http://contribute.jquery.org/style-guide/js/)
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
