<img src="../_images/javascript-badge.png" alt="JavaScript badge" title="JavaScript badge" height="260px"/>

# The Case for *Not* Hoisting Function Declarations

Function declarations are a major part of code organization. ESLint enforces its "no-use-before-defined" setting (and its alias "nofunc") to warn when a variable, function, or class has been used before being defined. For example

    foo();

    function foo() {
        console.log('bar');
    }

...will cause the build warning **`error  'foo' was used before it was defined`.** There is a list of reasons for this rule's existence and I would like to examine each one and offer some counter-points.

While I'll be obviously taking a side for this conversation, I'd like to begin by saying both sides are equally valid because both sides offer a path to codebase consistency. More than anything, I would like the takeaway from this discussion to be **"stick to your architecture."** Nothing defeats readability more than inconsistency between a team's developers.

* * *



* * *

<a id="markdown-" name=""></a>
##
