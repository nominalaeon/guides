<img src="../_images/javascript-badge.png" alt="JavaScript badge" title="JavaScript badge" height="260px"/>

# ECMAScript 5 Class pattern

There is something wrong with me. I love writing JavaScript Classes. In ES5.

When written correctly, a Class script is expressive and readable in a way many other patterns can't be. A Class's method and property assignments can become a table of contents that abstract away lengthy, hard to navigate functionality. Some of these concepts are hard to get at first, but, like an exotic parasite, once they tunnel into your brain they are impossible to remove.

* * *

- [Organizing](#organizing)
  - [Declaration](#declaration)
  - [Methods](#methods)
  - [Properties](#properties)
  - [Named functions: Public](#named-functions-public)
  - [Named functions: Private](#named-functions-private)
- [Naming](#naming)
  - [Methods](#methods-1)
  - [Properties](#properties-1)
- [All together now](#all-together-now)
- [FAQ You](#faq-you)
  - ["My linter is throwing up ***error 'myFunc' was used before it was defined***"](#%22my-linter-is-throwing-up-error-myfunc-was-used-before-it-was-defined%22)

* * *

<a id="markdown-organizing" name="organizing"></a>
## Organizing

All of the items inside a Class are more readable if you follow the same pattern every time. Here's an overview for how I recommend you order these items, and below we'll go over some reasoning for each one.


**example-class.js**

    (function () {

        'use strict'; /** because you're not a monster. */

        function ExampleClass() {
            this.init.apply(this, arguments);
        }

        /**
         * Assign methods to the Function object's prototype
         */
        _.extend(ExampleClass.prototype, assignMethods());

        /**
         * Assign properties to the Function object's prototype
         */
        Object.defineProperties(ExampleClass.prototype, assignProperties());

        ////

        assignMethods() { ... }

        assignProperties() { ... }

        ////

        /**
         * Named functions used for public methods
         */

        ////

        /**
         * Named functions used for private helpers
         */

    })();

> &nbsp;
>
> **Line-breaks:** I'm a huge fan of white space. Some people are comfortable jamming everything together, but a simple double line-break is an easy way to visually separate concepts.
>
> &nbsp;

<a id="markdown-declaration" name="declaration"></a>
### Declaration

        function ExampleClass() {
            this.init.apply(this, arguments);
        }

In the Class's declaration, an init method that we'll define later is invoked. It uses `Function.prototype.apply()` to pass along whatever arguments it gets. Handing off the constructor functionality to a separate method allows us to keep this top portion of the script slim and readable.

        _.extend(ExampleClass.prototype, assignMethods());

        Object.defineProperties(ExampleClass.prototype, assignProperties());

The assignment of the Class's methods and then properties is delegated to a couple of functions so that these statements act as a table of contents for the order of the code that will follow. `assignMethods()` will come first followed by `assignProperties()` and those two functions will display what is publicly available.

> &nbsp;
>
> Using an extend() function from a third-party library or an in-project utility helps keep prototype assignments more readable. Otherwise you're staring down a laundry list of `ExampleClass.prototype.whatever = whatever;` which will blow out this top section and ruin the whole point.
>
> &nbsp;

<a id="markdown-methods" name="methods"></a>
### Methods

The common theme for organizing everything across all languages is alphabetization. Your list of methods, properties, attributes, etc. may be short now, but you don't know how unweildy they'll become in the future.

    function assignMethods() {
        return {
            addExample: addExample,
            assignExamples: assignExamples,
            collectExamples: collectExamples,
            deleteExample: deleteExample,
            init: init
        };
    }

And that's it. The `assignMethods()` function is just a collection of references to **named functions** that will be defined below. Now, instead of being an unknown collection of hard to read anonymous function assignments, the method declarations are a table of contents for the named functions written below. Ta-da!

<a id="markdown-properties" name="properties"></a>
### Properties

The properties are a collection of getter/setter values. When a property is assigned (set), the new value is put onto a static object that I prefer to call `_vm` just to stay consistent. When a property's value is requested (get), what is returned is the value previously assigned to the static ViewModel object. This allows us to return a default value if that property is unassigned (undefined).

The default value returned is a great opportunity to improve readability by showing what type we expect a property to be.

    function assignProperties() {
        return {
            firstName: {
                get: function () {
                    return this._vm.firstName || '';
                },
                set: function (firstName) {
                    this._vm.firstName = firstName;
                }
            },
            fullName: {
                get: function () {
                    return this._vm.fullName || `${this.firstName} ${this.lastName}`;
                },
                set: function (fullName) {
                    this._vm.fullName = fullName;
                }
            },
            id: {
                get: function () {
                    return this._vm.id || null;
                },
                set: function (id) {
                    this._vm.id = id;
                }
            },
            lastName: {
                get: function () {
                    return this._vm.lastName || '';
                },
                set: function (lastName) {
                    this._vm.lastName = lastName;
                }
            },
            luckyNumber: {
                get: function () {
                    return this._vm.luckyNumber || _.random(1, 1000);
                },
                set: function (luckyNumber) {
                    this._vm.luckyNumber = luckyNumber;
                }
            }
        }
    }

<a id="markdown-named-functions-public" name="named-functions-public"></a>
### Named functions: Public

Any method that we made a reference to a function needs to be defined in this section. Remeber the `assignMethods()` function is a table of contents for this section so keep both in alphabetic order and finding your public functions will be a snap.

The function used to initialize a Class is special. We need to take any args sent in when the Class was new'ed and put them on the ViewModel object.

    function init(example) {
        this._vm = {};

        for (var prop in example) {
            if (!example.hasOwnProperty(prop)) {
                continue;
            }
            this[prop] = example[prop];
        }
    }

<a id="markdown-named-functions-private" name="named-functions-private"></a>
### Named functions: Private

The functions you used as public methods are going to need helper functions. "B-b-but I don't want to desynchronize my named functions from assignMethods() :frowny-face: :frowny-face:" Exactly. So below the section of Public named functions, create a section for the Private helper functions you're going to need.

I like to break up each section with a short string of slashes `////`.

<a id="markdown-naming" name="naming"></a>
## Naming

<a id="markdown-methods" name="methods"></a>
### Methods

ES6 has introduced an interesting hiccup in what used to be a no-brainer naming convention. It's a bit tricky to remember (I'm still forgetting to do it) and, while it doesn't break anything, the potential for this naming convention to proliferate across your codebase is extremely high.

**So what's the deal?**

`getSomething()` and `setSomething()`.

I love those method names. Like. A lot. I really conditioned myself to know what to expect from either function name. `getSomething()` is going to call an API and `setSomething()` is going to assign the results to the appropriate property.

**So what's the deal?**

In ES6, the getter/setter syntax was shorthanded in a way that makes these method names daaaangerously close to a property name: `get something() { ... }` and `set something() { ... }`.

Not a huge deal, but worth keeping in mind as you create your own habits in the Class pattern. I've started changing these names to `collectSomething()` and `assignSomething()` respectively.

<a id="markdown-properties" name="properties"></a>
### Properties

Proeprty names need to be communicative yet concise. Stick to JavaScript's lowerFirstThenCamelCase convention and you should be good.

There are no hard rules for naming stuff, so there are no wrong answers. Just try to put yourself in the shoes of future devs who will have to read your code without the full context of knowledge you are currently blessed with.

That said, never hold up a pull request for a namespace squabble. If you're willing to hold up code for a subjective naming preference, you are in dire need of some perspective.

<a id="markdown-all-together-now" name="all-together-now"></a>
## All together now

    (function () {

        'use strict';

        function ExampleClass() {
            this.init.apply(this, arguments);
        }

        _extend(ExampleClass.prototype, assignMethods());

        Object.defineProperties(ExampleClass.prototype, assignProperties());

        ////

        function assignMethods() {
            return {
                addExample: addExample,
                assignExamples: assignExamples,
                collectExamples: collectExamples,
                deleteExample: deleteExample,
                init: init
            };
        }

        function assignProperties() {
            return {
                firstName: {
                    get: function () {
                        return this._vm.firstName || '';
                    },
                    set: function (firstName) {
                        this._vm.firstName = firstName;
                    }
                },
                fullName: {
                    get: function () {
                        return this._vm.fullName || `${this.firstName} ${this.lastName}`;
                    },
                    set: function (fullName) {
                        this._vm.fullName = fullName;
                    }
                },
                id: {
                    get: function () {
                        return this._vm.id || null;
                    },
                    set: function (id) {
                        this._vm.id = id;
                    }
                },
                lastName: {
                    get: function () {
                        return this._vm.lastName || '';
                    },
                    set: function (lastName) {
                        this._vm.lastName = lastName;
                    }
                },
                luckyNumber: {
                    get: function () {
                        return this._vm.luckyNumber || _.random(1, 1000);
                    },
                    set: function (luckyNumber) {
                        this._vm.luckyNumber = luckyNumber;
                    }
                }
            };
        }

        ////

        function addExample() { ... }

        function assignExamples() { ... }

        function collectExamples() { ... }

        function deleteExample() { ... }

        function init(example) {
            this._vm = {};

            for (var prop in example) {
                if (!example.hasOwnProperty(prop)) {
                    continue;
                }
                this[prop] = example[prop];
            }
        }

        ////

        function _extend(out) {
            out = out || {};

            for (var i = 1; i < arguments.length; i++) {
                if (!arguments[i]) { continue; }

                for (var key in arguments[i]) {
                    if (!arguments[i].hasOwnProperty(key)) { continue; }
                    out[key] = arguments[i][key];
                }
            }

            return out;
        }

    })();

* * *

<a id="markdown-faq-you" name="faq-you"></a>
## FAQ You

<a id="markdown-my-linter-is-throwing-up-error-myfunc-was-used-before-it-was-defined" name="my-linter-is-throwing-up-error-myfunc-was-used-before-it-was-defined"></a>
### "My linter is throwing up ***error  'myFunc' was used before it was defined***"

This is a part of a larger, longer, more boringer conversation. The short version is, update your ".jshintrc" file to include `latedef: nofunc`.

The slighlty longer version is, only do this if you truly understand variable scoping. If you're still new to JavaScript, keep your default linter settings and rearrange the function declarations of this pattern to be at the top. Basically reverse order the entire pattern, it's okay, it's still very useful.

If you create and then proliferate a misqueue of variable declarations you are in for a world of debugging hell, but it's my opinion that linting for this cures the symptom instead of the horrible life choices that created them.

> &nbsp;
>
> "For more in-depth understanding of scoping and hoisting in JavaScript, read [JavaScript Scoping and Hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html) by Ben Cherry."
>
> -- [JSHint Docs](http://jshint.com/docs/options/#latedef)
>
> * * *
