
<img src="../_images/var-let-const/cover.png" alt="var vs. let vs. const" title="var vs. let vs. const" style="max-height: 260px;"/>

# var & let & const

So you Googled **"should use var or let const es6 best practice good bad"**, and read enough Medium articles to feel certain you should *always* use `const` ...or maybe it was `let`. Either way, if an article du jour left you wondering whether `var` was silently removed from all the browser engines, there's a good chance [You Don't Know JavaScript&trade;](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md#review-tldr).

Let's talk about `var`, the issues `let` and `const` came to solve, and maybe get a better idea of when and where each of these should be used.

* * *

- [The problem with "var"](#the-problem-with-%22var%22)
- ["let" examples](#%22let%22-examples)
  - [The "for" loop](#the-%22for%22-loop)
  - [The "if" block](#the-%22if%22-block)
  - [The late-comer example](#the-late-comer-example)
- ["const" examples](#%22const%22-examples)
- [Hoisting](#hoisting)

* * *

<a name="the-problem-with-var"></a>
## The problem with "var"

JavaScript has always been painfully forgiving with its type casting, so it's on the developer to leave a trail of clues for themselves and future devs about the type and restrictions of any object they create.

Here are two examples of those clues:

1. ALL_CAPS_UNDERSCORING naming convention for values that are relied on to be unchanging. These objects are called **constants**.
2. Manually hoisting variable declarations to show that their later, block-scoped assignments are temporary; we **let** them have different values sometimes.

The common principle of any of these "clues" is hoisting. You can use any variable declaration trick in the book, but if a browser is allowed to hoist an undefined version of it to the top of your function, there's going to be trouble. Annoying, hard to debug trouble.

<a name="let-examples"></a>
## "let" examples

<a name="the-for-loop"></a>
### The "for" loop

[The classic example](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md#loops--closure) of misunderstood scoping is declaring your index variable in a `for` loop's argument.

    for (var x = 0; x < someArray.length; x++) {
        console.log(x) // 0, 1, 2 ...
    }

9 times out of 10, a `for` loop for will handle this okay, but on that 10th time you'll spend hours trying to figure out why `foo` is set to `5` six times.

The correct way to write a `for` loop is to first declare your indexer, preferably in a way that communicates the type you're going to **let** it be later:

    var x = 0;

    for (x = 0; x < someArray.length; x++) {
        console.log(x) // 0, 1, 2 ...
    }

With `let` this translates very nicely to:

    for (let x = 0; x < someArray.length; x++) {
        console.log(x) // 0, 1, 2 ...
    }

This is why `let` is a thing. There are circumstances when a temporary variable is needed and you don't necessarily want extra variable declaration bloat at the top of your function. There's just no reason to be globally aware of this one-off indexer.

<a name="the-if-block"></a>
### The "if" block

This is not a thing. I've seen this touted as a use case for `let`, but this is just enabling and often teaching an anti-pattern. It's a hard to read, easyily broken convention.

If you're declaring `var` inside of `if` statements there are [some things you should read](https://github.com/getify/You-Dont-Know-JS). If you're declaring `let` inside of an `if` statement ...who hurt you?

<a name="the-late-comer-example"></a>
### The late-comer example

Sometimes you don't want to create a variable until you've done something else. My favorite example of this is **the guard clause**:

    if (!thing1) {
        return;
    }

    var thing2 = 'abc';

Good on ya! Creating any object in JS takes up precious run time. Why create one if there's a chance this function won't even use it? Unfortunately, browsers will turn your good intentions into this:

    var thing2; // this is initialized as undefined

    if (!thing1) {
        return;
    }

    thing2 = 'abc';

The thing you were hoping to avoid happens anyway. Here's where `let` swoops in to save the day:

    if (!thing1) {
        return;
    }

    let thing2 = 'abc';

With `let`, your post-guard-clause variable is still hoisted, but it won't be initialized until it's actually needed.

<pre><code>var thing2; // this is <b>uninitialized</b>, initialize-undefined's cheaper cousin

if (!thing1) {
    return;
}

thing2 = 'abc';</code></pre>

For more info, check out [this explanation](https://github.com/getify/You-Dont-Know-JS/issues/1132#issuecomment-325695891).

<a name="const-examples"></a>
## "const" examples

To make a complicated concept easy, let's just say `const` creates a permanent version of a value you want to reuse. There are way too many ins and outs to explain here. It gets even more complicated when using `const` to declare Arrays or Objects, so instead I'll leave a rule of thumb that doesn't require technical knowledge:

> Is this a time you'd use the ALL_CAPS_UNDERSCORE naming convention to convey a **constant**? Cool, then use `const`.

`const` will enforce a **constant** value, but it also needs to be communicated in your code. ALL_CAPS_UNDERSCORE naming is still the most recognizable convention for this.

If your code was a letter, it should read like this:

> Dear future developer,
> 
> Here is a value that must never *ever* change.
> 
> Also, here is a global Object that references inconstant tuba data. The only thing constant about this Object is how *in*constant it will be.
> 
> Now that you know about these two things, I'm going to go get the tuba's info. It will take x amount of time.
> 
> With love,
> Some dev from the past
> 
> p.s. did you notice how much easier these paragraphs are to read with line-breaks between them?

And in browserspeak it looks like this:

    const API_URL = '/api/product/tuba';

    var tuba = {};

    getTuba().then((results) => {
        tuba = results.data;
        tuba.randomNumber = Math.random();
    });

    function getTuba() {
        var promise = fetch(API_URL);
        return promise;
    }

<a name="hoisting"></a>
## Hoisting

Like `let`, browsers hoist an uninitialized `var` of your `const`. Using these new tools hasn't relieved us of our responsibility to understanding how hoisting works. I think if anything, grasping this concept is the most effective way to prevent unintended or over use of these new variables.

So rest easy, `var` fans. Your old friend is [still in the ECMAScript spec](https://www.ecma-international.org/ecma-262/6.0/#sec-variable-statement), and it plays a more important role than ever: improving readability.
