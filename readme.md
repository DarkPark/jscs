# JavaScript Code Style

This is a guide for writing consistent and aesthetically pleasing JavaScript code.
It is inspired by what is popular within the community, and flavored with some personal opinions.

## <a name='TOC'>Table of Contents</a>

  1. [Events](#events)
  1. [Modules](#modules)
  1. [Resources](#resources)


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
// Bad
Added feature X
Adding feature X
```

```javascript
// Good
Add feature X
Fix problem Y
```


## <a name='resources'>Resources</a>

* [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [jQuery JavaScript Style Guide](http://contribute.jquery.org/style-guide/js/)
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
