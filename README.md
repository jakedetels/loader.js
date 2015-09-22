loader.js [![Build Status](https://travis-ci.org/ember-cli/loader.js.png?branch=master)](https://travis-ci.org/ember-cli/loader.js)
=========

Minimal AMD loader mostly stolen from [@wycats](https://github.com/wycats).

## Tests

To run the test you'll need to have
[testem](https://github.com/airportyh/testem) installed. Install it with `npm
install -g testem`.

_(You'll also have to install the bower components, which you can do by running
`bower install`)_

You may run them with:
```bash
testem ci
```

You can also launch testem development mode with:
```bash
testem
```

## Mocking module dependencies for unit testing
The optional second argument to `require` accepts an object for one or more module dependencies to mock. The object's keys should be the name (path) of the dependency modules to mock, and the values should be the exports value of the mocked module.
```js
define('foo', ['./bar/baz', './buzz'], function(baz, buzz) {
    return function() {
        if ( baz() ) {
            alert('Hello ' + buzz);
        }
    };
});

var foo = require('foo', {
    './bar/baz': function() { return true; },
    './buzz': 'World!'
});

foo(); // alerts 'Hello World!'
```

Below is a simple example of mocking a module dependency for a QUnit styled unit test:
```js
// quiz.js
define('quiz', ['./answer-key'], function(AnswerKey) {
    function Quiz() {}
    Quiz.prototype.checkAnswer = function(question, answer) {
        return AnswerKey[question] === answer;
    };
    return Quiz;
});

// tests.js
test('checkAnswer returns boolean', function() {
    var Quiz = require('quiz', {
        './answer-key': { foo: 'bar' }
    });
    var quiz = new Quiz();
    
    ok( quiz.checkAnswer('foo', 'bar') );
    ok( ! quiz.checkAnswer('foo', 'baz') );
});
```

## License

loader.js is [MIT Licensed](https://github.com/ember-cli/loader.js/blob/master/LICENSE.md).
