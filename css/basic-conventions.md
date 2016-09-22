![CSS badge](../_images/css-badge.png "CSS badge")

# CSS Basic Conventions

**Ground rule #1:** When I say *Sass*, I'm talking about *Sassy CSS (SCSS)*. Sass***&trade;*** was the CSS compiler for last-gen CSS. SCSS, however, is CSS3-friendly and needed to be differentiated during the dark times before IE had caught up with the rest of the world. Those times are basically over, saying "Sass" is the same as saying "SCSS".

**Ground rule #2:** When I say *Sass*, I'm talking about CSS compilers in general. I'm not going to cover any Sass concept that doesn't exist in Sass, SCSS, Less, Stylus, or whatever. Anything I use Sass syntax to illustrate is a Google search away from becoming the syntax you require.

If you're *not* writing CSS with a pre-processor, stop all the things and go install one. They fit any project and will make your life the opposite of the living hell it is right now.

* * *

<!-- TOC -->

1. [Organizing](#organizing)
    1. [Compiler methods](#compiler-methods)
        1. [@extend](#extend)
        2. [@include](#include)
    2. [Attributes](#attributes)
    3. [Pseudo-classes](#pseudo-classes)
    4. [Pseudo-elements](#pseudo-elements)
    5. [Elements & Classes](#elements-classes)
    6. [ID's](#ids)
2. [Naming](#naming)
3. [Nesting](#nesting)
4. [FAQ You](#faq-you)
    7. ["I've got my own system. Hasn't failed me yet."](#ive-got-my-own-system-hasnt-failed-me-yet)
    8. ["Sass in *not* the same as SCSS"](#sass-in-not-the-same-as-scss)

<!-- /TOC -->

* * *


<a id="markdown-organizing" name="organizing"></a>
## Organizing

All of the items nested inside of an element are more readable if you follow the same pattern every time. Here's an overview for how I recommend you order these items, and below we'll go over some reasoning for each one.

    element {
        @extend .classname;         /** 1. Compiler method @extend */
        @include mixin-name();      /** 2. Compiler method @include */
                                    
        background-color: $blue;
        position: relative;
        width: auto;                /** 3. Attributes */
                                    
        &:hover { ... }             /** 4. Pseudo-classes */
                                    
        &::before { ... }           /** 5. Pseudo-elements */
                                    
        a { ... }
        h2 { ... }
        span { ... }                /** 6. Elements */
                                    
        .selector-class             /** 7. Classes */
    }

> **Line-breaks:** I'm a huge fan of white space. Some people are comfortable jamming everything together, but a simple double line-break is an easy way to visually separate concepts.


<a id="markdown-compiler-methods" name="compiler-methods"></a>
### Compiler methods


<a id="markdown-extend" name="extend"></a>
#### @extend

This is Sass's way of saying, "Copy all the attributes of this selector and paste it right here." It's like a mixin without any control over the outcome or a variable that gives you lots of CSS all at once. Most importantly, **@extend** is perfectly communicative. Here's a quick example:

    .lipstick {
        font-style: italic;
        font-weight: 600;
        text-align: center;
    }
    .pig {
        @extend .lipstick;
    }

    /** .pig compiles as
    .pig {
        font-style: italic;
        font-weight: 600;
        text-align: center;
    }
    */

Keep **@extend** declarations at the very top so their specificity can be overpowered with attributes defined afterward.


<a id="markdown-include" name="include"></a>
#### @include

This is how we invoke a mixin. They often take arguments, but they can just as easily be used as factories to pump out blobs of attributes. Ideally they are used with arguments to return unique results for each caller's needs. If you're needing a way to hand over just a bunch of static attributes, I would argue that **@extend** is the more expressive way to do so.

<pre>@mixin alignment($direction) {
    float: $direction;
    text-align: $direction;
}
.pig {
    @extend .lipstick;
    @include alignment(left);
}

/** .pig compiles as
.pig {
    font-style: italic;
    font-weight: 600;
    text-align: center;
    <b>float: left;</b>
    <b>text-align: left;</b> /** value from @include will override @extend */
}
*/</pre>

Keep **@include** declarations second so they can overwrite any **@extend** declarations but still be in a position where their specificity can be overpowered.


<a id="markdown-attributes" name="attributes"></a>
### Attributes

**Alphabetize your attributes.** Every modern text editor has extensions or plug-ins or even built in functionality to help if you're not already in the habit of alphabetizing.

<pre>element {
    background-color: $blue;
    border: 1px solid $red;
    border-top-color: $green;
    border-right-width: 2px;
    border-bottom-color: $yellow;
    border-bottom-width: 2px;
    border-left-color: $green;
    display: inline;
    height: auto;
    min-height: 100px;
    max-height: 200px;
    margin: 2px 4px 6px 8px;
    padding: 0;
    padding-right: 20px;
    padding-bottom: 10px;
    position: absolute;
    top: 0; right: 0; bottom: 0; left: 0; // <b>when space allows, position directions can be inlined.</b>
    vertical-align: top;
    width: auto;
    max-width: 100%;
}</pre>

You'll have noticed there are a couple exceptions to alphabetic ordering:

***Position* attributes:** The shorthand rule for positioning (top, right, bottom, left) is tricky to learn and remember at first. Order positioning properties in a way to facilitate learning and retaining that convention because it can feel very counter-intuitive at first.

<pre>element {
    margin: <b>2px 4px 6px 8px</b>; // <b>top, right, bottom, left</b>
}</pre>

***Min/Max* attributes:** In a long block of properties, having "min" and "max" in the M's instead of with their respective "height"-or-"width" counterpart is confusing. They should go directly below their respective dimension property. I prefer to order them min-then-max, because whenever I say it in my head, "min/max" is the first thing I hear.

    element {
        height: auto;
        min-height: 100px;
        max-height: 200px;
    }


<a id="markdown-pseudo-classes" name="pseudo-classes"></a>
### Pseudo-classes

Pseudo-classes are selectors for an element's state. As long as you're defining your pseudo-classes *after* the extends, includes, and attributes, you're golden. The order of these are not concrete, sometimes you'll need a **:hover** to be stronger than a **:focus** and vice versa, so it's really case by case. There aren't an overwhelming amount of pseudo-classes available so **alphabetize when possible, otherwise order as needed to achieve the desired specificity**.

See the full, official, curated-by-someone-else list of pseudo-classes at [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/CSS/pseudo-classes).


<a id="markdown-pseudo-elements" name="pseudo-elements"></a>
### Pseudo-elements

You'll often be working with a max of two of these, **::before** and **::after**. I prefer to put ::before ahead of ::after but you'd have a hard time mucking up readability between these two. Keep their declarations after the pseudo-classes and that's good enough.

See the full, official, curated-by-someone-else list of pseudo-elements at [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/CSS/pseudo-elements).


<a id="markdown-elements-classes" name="elements-classes"></a>
### Elements & Classes

**Alphabetize your selectors.** This one is tougher to swallow, but the best stuff often is. It's always tempting to group selectors together, letting their hierarchy be a self-serving table of contents. And aye, that will work. For a time. But, lying in your beds, years from now, would you not trade all of your days from then to now for one chance, just one chance, to enforce an alphabetized list of elements and class names ***before* they became too many to manage!**

<img src="../_images/lying-in-your-beds.jpg" alt="Lying in your beds" title="Lying in your beds" width="480px;"/>


<a id="markdown-ids" name="ids"></a>
### ID's

You might have noticed that I forgot to mention ID selectors. If no one has ever sat you down and given you *the talk*, allow me the honor. ID's are for **form element** identification. For everything else, there's class names. Selecting DOM elements for manipulation with CSS (or JavaScript, I would half-heartedly argue and concede as soon as I faced any opposition) is a pretty good indication that you need to revisit your [naming conventions](#naming).


<a id="markdown-naming" name="naming"></a>
## Naming

**Use [BEM](https://css-tricks.com/bem-101/) naming conventions**

Body, Element, Modifier (BEM) is a well-established convention that helps reduce specificity conflicts. For a pre-processor like Sass, this means reducing the need to nest selectors which, above everything else, improves code readability.

Let's use an old-school example. Here's some HTML followed by its Sass:

    <div class="pig">
        <div class="leg left"></div>
        <div class="leg right"></div>
    </div>

    .pig {
        .leg {
            &.left {
                float: left;
            }
        }
    }

That Sass compiles to `.pig .leg.left`, a Frankenstein selector with a specificity of **three**! Good luck overriding that without `!important`. If we trade off using a little more verbosity in the markup, the Sass can be distilled to:

    <div class="pig">
        <div class="pig__leg pig__leg--left"></div>
        <div class="pig__leg pig__leg--right"></div>
    </div>

    .pig__leg--left {
        float: left;
    }

Now we have `.pig__leg--left` handling the unique positioning of the pig's left leg. The name's a little longer but it's 1) more readable, 2) rocking a specificity of **one**, 3) not hiding in a Sass nest, and 4) can be easily overridden.

> ***["If you follow strict BEM conventions, you will be able to update and add to your CSS in the future with the full confidence that your changes will not have side effects."](https://philipwalton.com/articles/side-effects-in-css/)***

There are other, equally respected and established conventions, and, while my preference is BEM, a project benefits from having any of them in place. A project that's dedicated to a naming-convention framework reduces conflict and improves consistency.


<a id="markdown-nesting" name="nesting"></a>
## Nesting

**Only nest when necessary**

One of the main benefits of using BEM is moving away from adding more than one selector for any element. However, the opposite side of over-nesting is over-BEM'ing. It takes some getting used to but the point of BEM is to reduce nesting, not eliminate it. Not every element needs a class name, it's okay to use element selectors and even unrelated class selectors inside of a defined element. For example:

    <div class="pig">
        <h2>Some pig.</h2>

        <div class="pig__leg pig__leg--left"></div>
        <div class="pig__leg pig__leg--right"></div>
        
        <div class="saddle">
            <h3>Yee-haw!</h3>
        </div>
    </div>

    .pig {
        h2 {
            color: $pink;
        }

        .saddle {
            background-color: $brown;

            h3 {
                color: $white;
            }
        }
    }

Like anything else, BEM's meant to be used in moderation. Even with the `.saddle` class nested in the `.pig`, the code is infinitely more manageable than it was with the nested `.left .leg`. Additionally, with `.saddle` nested like this, it now has a specificity of **two** which is strong enough to override other declarations to maintain its unique styling when it's on the `.pig` but weak enough that it can be overridden with a modifier:

    .pig--dirty {
        .saddle {
            display: none;
        }
    }

Protip: Don't put your saddle on a dirty pig.


<a id="markdown-faq-you" name="faq-you"></a>
## FAQ You


<a id="markdown-ive-got-my-own-system-hasnt-failed-me-yet" name="ive-got-my-own-system-hasnt-failed-me-yet"></a>
### "I've got my own system. Hasn't failed me yet."

<img src="../_images/my-own-system.jpg" alt="I've got my own system" title="I've got my own system" width="480px"/>

There's an outside chance that you're on of the dozens of developers that have never alphabetized their attributes and right now you're thinking "Not broke? Don't fix." There are a couple explanations for this response. 1) Your attribute blocks are still small enough that readability isn't effected or 2) you're totally in tune with the piles of clutter you've created for yourself. Both explanations are arguments against themselves. They are short-sighted and would be hard to say out-loud to your boss, but I totally dare you to anyway.

I will concede that **there is value in personal and team habits**. If you have your own system and it truly hasn't failed you yet, kudes to you. I recommend you reconsider the code's readability; make sure your way is lowering your bus factor and creating an easy on-boarding process for new devs. If it is, then great. Mad props, outlier.

<img src="../_images/mad-props-outlier.png" alt="Mad props, outlier." title="Mad props, outlier." width="480px;"/>


<a id="markdown-sass-in-not-the-same-as-scss" name="sass-in-not-the-same-as-scss"></a>
### "Sass in *not* the same as SCSS"

If you want to quibble over the whole *saying "Sass" is the same as saying "SCSS"* thing, prove your point by installing a Sass pre-processor and use `.sass` files.

Yeah, "who does that anymore", right?

Right.

* * *