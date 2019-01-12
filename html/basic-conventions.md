<img src="../_images/html-badge.png" alt="HTML badge" title="HTML badge" style="max-height: 260px;"/>

# HTML Basic Conventions

* * *

<!-- TOC -->

1. [Organizing Attributes](#organizing-attributes)
    1. [Class](#class)
    2. [Standard](#standard)
    3. [Data](#data)
    4. [Framework](#framework)

<!-- /TOC -->

* * *

<a id="markdown-organizing-attributes" name="organizing-attributes"></a>
## Organizing Attributes

All of the attributes of an element are more readable if you follow the same pattern every time. Here's an overview for how I recommend you order these attributes, and below we'll go over some reasoning for each one.

    <element class="class-name"                 /** 1. Class */
        alt="Some alt text"                     /**
        aria-hidden="true"                       **
        id="did-I-really-need-an-id"             **
        name="element-name"                      **
        style="border: 0; display: block;"       **
        title="Element's Title"                  ** 2. Standard */
        data-custom-attr="A custom val"         /**
        data-plugin-attr="Val for a plugin"      **
        data-yet-another-attr="Another val"      ** 3. Data */
        ng-class="{                             /**
            'active': hasValue(),                **
            'hidden': !hasValue()                **
        }"                                       **
        ng-click="onClick()"                     **
        ng-repeat="item in items" >              ** 4. Framework */

> **Line-breaks:** I'm a huge fan of white space. Some people are comfortable jamming everything together, but a simple line-break is an easy way to visually separate concepts.

Group an element's attributes by *standard* and then *framework* properties, then alphabetize those attributes. The two exceptions (**class** and **data** attributes) are explained below.

<a id="markdown-class" name="class"></a>
### Class

The **class** property is an element's identification. Even more than an **id** or a **name**, the class attribute should convey an element's nature, purpose, and sometimes even appearance. It's the attribute that should be used for DOM selection and manipulation either through CSS or JavaScript.

Because of this attribute's ability and responsibility for element identification, keep it at the top, in-line with the element tag opener, out of the alphabetic order of the other standard attributes.

For class name instructions, refer to CSS Basic Conventions' ["Naming" section](../css/basic-conventions.md#naming). 

<a id="markdown-standard" name="standard"></a>
### Standard

With the exceptions of **class** and **data** attributes, the myriad of other element attributes should be alphabetized. More advanced articles will discuss how attributes should be used most effectively or be avoided entirely. 

<a id="markdown-data" name="data"></a>
### Data

**Data** attributes are used to deliver custom data from the markup to JavaScript, typically because a back-end language was used as an HTML templating engine and it's the most efficient and readable way to hand-off variable information.

This attribute is limitless in its variety and has potential to grow to a point where it confuses readability of the other standard attributes. Because of this, keep them below the other standard attributes, and remember to alphabetize them.

<a id="markdown-framework" name="framework"></a>
### Framework

An easiest example of framework attributes is Angular. Like the **data** attribute, **framework** attributes have potential of growing to a point where they confuse the readability of the other attributes. Keep them below the standard and data attributes, and remember to alphabetize them.

Framework attribute values often use JavaScript syntax. While it's best to abstract larger chunks of logic to a framework controller, writing some JavaScript logic in the markup is unavoidable. Take advantage of the leniency given by a browser's rendering engine and put in line-breaks and indentions so attributes are identifiable as blocks of code.
