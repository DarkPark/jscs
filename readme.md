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
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditionals)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [Resources](#resources)


## <a name='objects'>Objects</a>

Use the literal syntax for object creation.
> Both ways do the same thing, but literal notation takes less space. It's clearly recognizable as to what is happening, so using `new object()`, is really just typing more.

```javascript
// bad
var item = new Object();
```

```javascript
// good
var item = {};
```

Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys.
> It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61)

```javascript
// bad
var superman = {
    default: {clark: 'kent'},
    private: true
};
```

```javascript
// good
var superman = {
    defaults: {clark: 'kent'},
    hidden: true
};
```

Use readable synonyms in place of reserved words.

```javascript
// bad
var superman = {
    klass: 'alien'
};
```

```javascript
// good
var superman = {
    type: 'alien'
};
```


## <a name='arrays'>Arrays</a>

Use the literal syntax for array creation.
> `new Array(len)` creates an array with len holes. On some JavaScript engines, this operation lets you pre-allocate arrays, giving you performance benefits for small arrays (not for large ones). In most cases, performance doesn’t matter and you can avoid the redundancy introduced by a preallocation. If it fits the problem, an array literal initializing all elements at once is preferable.

```javascript
// bad
var items = new Array();
```

```javascript
// good
var items = [];
```

To append some value to array use [Array.push](http://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push).

```javascript
// bad
items[items.length] = 'abracadabra';
```

```javascript
// good
itmes.push('abracadabra');
```

When you need to copy an array use [Array.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice).

```javascript
itemsCopy = items.slice();
```

To convert an [array-like](http://www.2ality.com/2013/05/quirk-array-like-objects.html) object to an array, use it as well.

```javascript
function trigger() {
    var args = Array.prototype.slice.call(arguments);
    ...
}
```


## <a name='strings'>Strings</a>

- Use single quotes `''` for strings

```javascript
// bad
var name = "Bob Parr";

// good
var name = 'Bob Parr';

// bad
var fullName = "Bob " + this.lastName;

// good
var fullName = 'Bob ' + this.lastName;
```

- Strings longer than 80 characters should be written across multiple lines using string concatenation.
- Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

```javascript
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
var errorMessage = 'This is a super long error that \
was thrown because of Batman. \
When you stop to think about \
how Batman had anything to do \
with this, you would get nowhere \
fast.';


// good
var errorMessage = 'This is a super long error that ' +
  'was thrown because of Batman. ' +
  'When you stop to think about ' +
  'how Batman had anything to do ' +
  'with this, you would get nowhere ' +
  'fast.';
```

- When programatically building up a string, use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

```javascript
var items,
	messages,
	length,
	i;

messages = [{
	state: 'success',
	message: 'This one worked.'
},{
	state: 'success',
	message: 'This one worked as well.'
},{
	state: 'error',
	message: 'This one did not work.'
}];

length = messages.length;

// bad
function inbox(messages) {
  items = '<ul>';

  for (i = 0; i < length; i++) {
	items += '<li>' + messages[i].message + '</li>';
  }

  return items + '</ul>';
}

// good
function inbox(messages) {
  items = [];

  for (i = 0; i < length; i++) {
	items[i] = messages[i].message;
  }

  return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
}
```

**[[⬆]](#TOC)**


## <a name='functions'>Functions</a>

- Function expressions:

```javascript
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
  console.log('Welcome to the Internet. Please follow me.');
})();
```

- Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
- **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

```javascript
// bad
if (currentUser) {
  function test() {
	console.log('Nope.');
  }
}

// good
var test;
if (currentUser) {
  test = function test() {
	console.log('Yup.');
  };
}
```

- Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

```javascript
// bad
function nope(name, options, arguments) {
  // ...stuff...
}

// good
function yup(name, options, args) {
  // ...stuff...
}
```

**[[⬆]](#TOC)**



## <a name='properties'>Properties</a>

- Use dot notation when accessing properties.

```javascript
var luke = {
  jedi: true,
  age: 28
};

// bad
var isJedi = luke['jedi'];

// good
var isJedi = luke.jedi;
```

- Use subscript notation `[]` when accessing properties with a variable.

```javascript
var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
```

**[[⬆]](#TOC)**


## <a name='variables'>Variables</a>

- Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

```javascript
// bad
superPower = new SuperPower();

// good
var superPower = new SuperPower();
```

- Use one `var` declaration for multiple variables and declare each variable on a newline.

```javascript
// bad
var items = getItems();
var goSportsTeam = true;
var dragonball = 'z';

// good
var items = getItems(),
	goSportsTeam = true,
	dragonball = 'z';
```

- Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

```javascript
// bad
var i, len, dragonball,
	items = getItems(),
	goSportsTeam = true;

// bad
var i, items = getItems(),
	dragonball,
	goSportsTeam = true,
	len;

// good
var items = getItems(),
	goSportsTeam = true,
	dragonball,
	length,
	i;
```

- Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

```javascript
// bad
function() {
  test();
  console.log('doing stuff..');

  //..other stuff..

  var name = getName();

  if (name === 'test') {
	return false;
  }

  return name;
}

// good
function() {
  var name = getName();

  test();
  console.log('doing stuff..');

  //..other stuff..

  if (name === 'test') {
	return false;
  }

  return name;
}

// bad
function() {
  var name = getName();

  if (!arguments.length) {
	return false;
  }

  return true;
}

// good
function() {
  if (!arguments.length) {
	return false;
  }

  var name = getName();

  return true;
}
```

**[[⬆]](#TOC)**


## <a name='hoisting'>Hoisting</a>

- Variable declarations get hoisted to the top of their scope, their assignment does not.

```javascript
// we know this wouldn't work (assuming there
// is no notDefined global variable)
function example() {
  console.log(notDefined); // => throws a ReferenceError
}

// creating a variable declaration after you
// reference the variable will work due to
// variable hoisting. Note: the assignment
// value of `true` is not hoisted.
function example() {
  console.log(declaredButNotAssigned); // => undefined
  var declaredButNotAssigned = true;
}

// The interpreter is hoisting the variable
// declaration to the top of the scope.
// Which means our example could be rewritten as:
function example() {
  var declaredButNotAssigned;
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;
}
```

- Anonymous function expressions hoist their variable name, but not the function assignment.

```javascript
function example() {
  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function() {
	console.log('anonymous function expression');
  };
}
```

- Named function expressions hoist the variable name, not the function name or the function body.

```javascript
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {
	console.log('Flying');
  };
}

// the same is true when the function name
// is the same as the variable name.
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  var named = function named() {
	console.log('named');
  }
}
```

- Function declarations hoist their name and the function body.

```javascript
function example() {
  superPower(); // => Flying

  function superPower() {
	console.log('Flying');
  }
}
```

- For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

**[[⬆]](#TOC)**



## <a name='conditionals'>Conditional Expressions & Equality</a>

- Use `===` and `!==` over `==` and `!=`.
- Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

+ **Objects** evaluate to **true**
+ **Undefined** evaluates to **false**
+ **Null** evaluates to **false**
+ **Booleans** evaluate to **the value of the boolean**
+ **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
+ **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

```javascript
if ([0]) {
  // true
  // An array is an object, objects evaluate to true
}
```

- Use shortcuts.

```javascript
// bad
if (name !== '') {
  // ...stuff...
}

// good
if (name) {
  // ...stuff...
}

// bad
if (collection.length > 0) {
  // ...stuff...
}

// good
if (collection.length) {
  // ...stuff...
}
```

- For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

**[[⬆]](#TOC)**


## <a name='blocks'>Blocks</a>

- Use braces with all multi-line blocks.

```javascript
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function() { return false; }

// good
function() {
  return false;
}
```

**[[⬆]](#TOC)**


## <a name='comments'>Comments</a>

- Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

```javascript
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param <String> tag
// @return <Element> element
function make(tag) {

  // ...stuff...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param <String> tag
 * @return <Element> element
 */
function make(tag) {

  // ...stuff...

  return element;
}
```

- Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

```javascript
// bad
var active = true;  // is current tab

// good
// is current tab
var active = true;

// bad
function getType() {
  console.log('fetching type...');
  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}

// good
function getType() {
  console.log('fetching type...');

  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}
```

- Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

- Use `// FIXME:` to annotate problems

```javascript
function Calculator() {

  // FIXME: shouldn't use a global here
  total = 0;

  return this;
}
```

- Use `// TODO:` to annotate solutions to problems

```javascript
function Calculator() {

  // TODO: total should be configurable by an options param
  this.total = 0;

  return this;
}
```

**[[⬆]](#TOC)**


## <a name='whitespace'>Whitespace</a>

- Use soft tabs set to 2 spaces

```javascript
// bad
function() {
∙∙∙∙var name;
}

// bad
function() {
∙var name;
}

// good
function() {
∙∙var name;
}
```
- Place 1 space before the leading brace.

```javascript
// bad
function test(){
  console.log('test');
}

// good
function test() {
  console.log('test');
}

// bad
dog.set('attr',{
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});

// good
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});
```
- Place an empty newline at the end of the file.

```javascript
// bad
(function(global) {
  // ...stuff...
})(this);
```

```javascript
// good
(function(global) {
  // ...stuff...
})(this);

```

- Use indentation when making long method chains.

```javascript
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// good
$('#items')
  .find('.selected')
	.highlight()
	.end()
  .find('.open')
	.updateCount();

// bad
var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
	.attr('width',  (radius + margin) * 2).append('svg:g')
	.attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
	.call(tron.led);

// good
var leds = stage.selectAll('.led')
	.data(data)
  .enter().append('svg:svg')
	.class('led', true)
	.attr('width',  (radius + margin) * 2)
  .append('svg:g')
	.attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
	.call(tron.led);
```

**[[⬆]](#TOC)**

## <a name='commas'>Commas</a>

- Leading commas: **Nope.**

```javascript
// bad
var once
  , upon
  , aTime;

// good
var once,
	upon,
	aTime;

// bad
var hero = {
	firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};

// good
var hero = {
  firstName: 'Bob',
  lastName: 'Parr',
  heroName: 'Mr. Incredible',
  superPower: 'strength'
};
```

- Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

> Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

```javascript
// bad
var hero = {
  firstName: 'Kevin',
  lastName: 'Flynn',
};

var heroes = [
  'Batman',
  'Superman',
];

// good
var hero = {
  firstName: 'Kevin',
  lastName: 'Flynn'
};

var heroes = [
  'Batman',
  'Superman'
];
```

**[[⬆]](#TOC)**


## <a name='semicolons'>Semicolons</a>

- **Yup.**

```javascript
// bad
(function() {
  var name = 'Skywalker'
  return name
})()

// good
(function() {
  var name = 'Skywalker';
  return name;
})();

// good
;(function() {
  var name = 'Skywalker';
  return name;
})();
```

**[[⬆]](#TOC)**


## <a name='type-coercion'>Type Casting & Coercion</a>

- Perform type coercion at the beginning of the statement.
- Strings:

```javascript
//  => this.reviewScore = 9;

// bad
var totalScore = this.reviewScore + '';

// good
var totalScore = '' + this.reviewScore;

// bad
var totalScore = '' + this.reviewScore + ' total score';

// good
var totalScore = this.reviewScore + ' total score';
```

- Use `parseInt` for Numbers and always with a radix for type casting.

```javascript
var inputValue = '4';

// bad
var val = new Number(inputValue);

// bad
var val = +inputValue;

// bad
var val = inputValue >> 0;

// bad
var val = parseInt(inputValue);

// good
var val = Number(inputValue);

// good
var val = parseInt(inputValue, 10);
```

- If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.
- **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109)

```javascript
// good
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
var val = inputValue >> 0;
```

- Booleans:

```javascript
var age = 0;

// bad
var hasAge = new Boolean(age);

// good
var hasAge = Boolean(age);

// good
var hasAge = !!age;
```

**[[⬆]](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a>

- Avoid single letter names. Be descriptive with your naming.

```javascript
// bad
function q() {
  // ...stuff...
}

// good
function query() {
  // ..stuff..
}
```

- Use camelCase when naming objects, functions, and instances

```javascript
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
function c() {};
var u = new user({
  name: 'Bob Parr'
});

// good
var thisIsMyObject = {};
function thisIsMyFunction() {};
var user = new User({
  name: 'Bob Parr'
});
```

- Use PascalCase when naming constructors or classes

```javascript
// bad
function user(options) {
  this.name = options.name;
}

var bad = new user({
  name: 'nope'
});

// good
function User(options) {
  this.name = options.name;
}

var good = new User({
  name: 'yup'
});
```

- Use a leading underscore `_` when naming private properties

```javascript
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```

- When saving a reference to `this` use `_this`.

```javascript
// bad
function() {
  var self = this;
  return function() {
	console.log(self);
  };
}

// bad
function() {
  var that = this;
  return function() {
	console.log(that);
  };
}

// good
function() {
  var _this = this;
  return function() {
	console.log(_this);
  };
}
```

- Name your functions. This is helpful for stack traces.

```javascript
// bad
var log = function(msg) {
  console.log(msg);
};

// good
var log = function log(msg) {
  console.log(msg);
};
```

**[[⬆]](#TOC)**


## <a name='accessors'>Accessors</a>

- Accessor functions for properties are not required
- If you do make accessor functions use getVal() and setVal('hello')

```javascript
// bad
dragon.age();

// good
dragon.getAge();

// bad
dragon.age(25);

// good
dragon.setAge(25);
```

- If the property is a boolean, use isVal() or hasVal()

```javascript
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

- It's okay to create get() and set() functions, but be consistent.

```javascript
function Jedi(options) {
  options || (options = {});
  var lightsaber = options.lightsaber || 'blue';
  this.set('lightsaber', lightsaber);
}

Jedi.prototype.set = function(key, val) {
  this[key] = val;
};

Jedi.prototype.get = function(key) {
  return this[key];
};
```

**[[⬆]](#TOC)**


## <a name='constructors'>Constructors</a>

- Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

```javascript
function Jedi() {
  console.log('new jedi');
}

// bad
Jedi.prototype = {
  fight: function fight() {
	console.log('fighting');
  },

  block: function block() {
	console.log('blocking');
  }
};

// good
Jedi.prototype.fight = function fight() {
  console.log('fighting');
};

Jedi.prototype.block = function block() {
  console.log('blocking');
};
```

- Methods can return `this` to help with method chaining.

```javascript
// bad
Jedi.prototype.jump = function() {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
};

var luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20) // => undefined

// good
Jedi.prototype.jump = function() {
  this.jumping = true;
  return this;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
  return this;
};

var luke = new Jedi();

luke.jump()
  .setHeight(20);
```


- It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

```javascript
function Jedi(options) {
  options || (options = {});
  this.name = options.name || 'no name';
}

Jedi.prototype.getName = function getName() {
  return this.name;
};

Jedi.prototype.toString = function toString() {
  return 'Jedi - ' + this.getName();
};
```

**[[⬆]](#TOC)**


## <a name='events'>Events</a>

When attaching data to events it's always should be only one parameter.
Pass a hash instead of a raw value in case there are more then one parameter.
> This approach allows `EventEmitter` implementation with better performance 

```javascript
// bad
button.trigger('click', data1, data2, data3);

button.on('click', function ( data1, data2, data3 ) {
    // do something with arguments
});
```

```javascript
// good
button.trigger('click', value);

button.on('click', function ( value ) {
    // do something with value
});
```

```javascript
// good
button.trigger('click', {val1: data1, val2: data2, val3: data3});

button.on('click', function ( data ) {
    // do something with data.val1, data.val2 and data.val3
});
```


## <a name='modules'>Modules</a>

Use [CommonJS](http://wiki.commonjs.org/wiki/Modules) modules.
[Browserify](http://browserify.org/), [Webmake](https://github.com/medikoo/modules-webmake) or similar are advised to bundle modules for web browsers.


## <a name='commits'>Commits</a>

Every commit message, pull request title or issue discussion must be in English.
> English is the universal language nowadays, if you use any other language you're excluding potencial contributors.

Don't use Past or Present Continuous tenses for commit messages, those should be in Imperative Present tense.
> This preference comes from [Git itself](http://git.kernel.org/cgit/git/git.git/tree/Documentation/SubmittingPatches?id=HEAD#n111).

```javascript
// bad
Added feature X
Adding feature X
```

```javascript
// good
Add feature X
Fix problem Y
```


## <a name='resources'>Resources</a>

* [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [jQuery JavaScript Style Guide](http://contribute.jquery.org/style-guide/js/)
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
