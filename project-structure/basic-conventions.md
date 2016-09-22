# Project Structure Basic Conventions

Project structure is subjective. Aside from personal preference, the choices made for your project's directory structure relies on too many factors to list here. While I intend for this document to eliviate conflicts and indecision, none of this is above altering based on a project's needs.

* * *

<!-- TOC -->

1. [Separation of concerns](#separation-of-concerns)
    1. [SRC](#src)
    2. [APP](#app)
2. [Components](#components)
    3. [Scripts](#scripts)
    4. [SCSS](#scss)
3. [Templates](#templates)
4. [Committing "app" content](#committing-app-content)

<!-- /TOC -->

* * *

<a id="markdown-separation-of-concerns" name="separation-of-concerns"></a>
## Separation of concerns

It will be necessary to keep compiled/completed files separated from the raw counterparts you use for development. Neither compiled or raw files need each other, and they will only clutter each other's directory space.

There are many strategies regarding project structure, and the rabbit hole can get pretty deep in advanced examples. For this discussion we'll keep it pared down to two folders, "src" and "app".

<a id="markdown-src" name="src"></a>
### SRC

The **"src"** folder will hold all of the raw files that need to have tasks run against them. To keep things simple we'll consider Sass, JavaScript, and HTML files, but the possibilities are vast.

Accomodating these filetypes to build a project, the "src" folder will look something like this:

* &#128194; project-name
    * &#128194; src
        * &#128194; images
            * &#128196; cat.png
        * &#128194; scripts
            * &#128196; app.js
            * &#128196; utils.js
        * &#128194; scss
            * &#128196; _fonts.scss
            * &#128196; app.scss
        * &#128194; templates
            * &#128196; home.tmpl.html
        * &#128196; index.html
    * &#128196; package.json

<a id="markdown-app" name="app"></a>
### APP

The **"app"** folder will be the output destination for our task runner. Source ("src" get it?) files that are concatenated, compiled, minified, etc. will be delivered to the "app" folder. The result will look something like this:

* &#128194; project-name
    * &#128194; app
        * &#128194; assets
            * &#128194; images
                * &#128196; cat.png
            * &#128194; js
                * &#128196; app.js
            * &#128194; css
                * &#128196; app.css
        * &#128194; templates
            * &#128196; home.tmpl.html
        * &#128196; index.html
    * &#128193; src
    * &#128196; package.json

This naming convention for the destination folder makes sense for most cases since "app" represents your ***app***lication, whether it's web or native. However, depending on the framework you're working in, this folder may have to follow a predestined naming structure like "build" or "dist" or "public". Either way the concept is the same, keep the working files as segregated from the build files as possible.

<a id="markdown-components" name="components"></a>
## Components

Within a working directory (scripts, scss), there will be groups of files that make sense to isolate and group together. Even if these pieces aren't completely stand-alone components, it will help the project's readability to keep them nested in their own directories.

Angular files are the easiest example but the naming conventions and patterns I'll go over here are not at all unique to that framework.

<a id="markdown-scripts" name="scripts"></a>
### Scripts

Using the "app/scripts" directory from before let's say we have all of these files:

* &#128194; scripts
    * &#128196; app.js
    * &#128196; app.routes.js
    * &#128196; home.controller.js
    * &#128196; loading-spinner.directive.js
    * &#128196; loading-spinner.service.js
    * &#128196; mega-menu.directive.js
    * &#128196; offices.service.js
    * &#128196; profile.controller.js
    * &#128196; user.class.js
    * &#128196; users.service.js
    * &#128196; utils.js

These files should be grouped based on these kinds of concepts:

    * Stand-alone, component
    * Dependency of multiple, different scripts
    * Functionality: viewmodel, directive, config
    * Script type: Angular, jQuery, vanilla JS

Keep your folder structure as flat as you can, but go as deep as you need to be expressive and allow for growth. Here's how I would sort the previous example:

* &#128194; scripts
    * &#128194; _utils
        * &#128196; google-analytics.js
        * &#128196; lodash-shims.js
    * &#128194; components
        * &#128194; loading-spinner
            * &#128196; loading-spinner.directive.js
            * &#128196; loading-spinner.service.js
        * &#128194; mega-menu
            * &#128196; mega-menu.directive.js
    * &#128194; services
        * &#128194; session
            * &#128196; offices.service.js
            * &#128196; user.class.js
            * &#128196; users.service.js
    * &#128194; views
        * &#128194; home
            * &#128196; home.controller.js
        * &#128194; profile
            * &#128196; profile.controller.js
    * &#128196; app.js
    * &#128196; app.routes.js

**"components"** are the easier files to identify and coral. For Angular this typically means Directives all though that can easily include a route/view Controller. Let's assume the **mega-menu** Directive uses the **users** Service, but lots of other scripts will also use that service so we don't partition it with the mega-menu. Conversely, the **loading-spinner** Service serves only the loading-spinner Directive, so it makes sense to store them together.

**"services"** will likely stand out next. For Angular a "service" can be a switch-board style collection of values and methods that separate Directives can use to communicate. It could also be a Factory that exists to provide Class templates for anything, even a Service. If a service script is too prolific to store in a "component", there's a good chance it's sharing functionality with other services that might provide a communicative way to nest them together. For the **offices** and **users** Services, they share common functionality related to the app's "session".

**"views"** are often just the Controller file declared for a given route. Outside of separating them into the "views" folder, nesting them in their own folders becomes less important. I prefer to just in case I do come across a use case for storing additional files with the view controller ...but I'm lamely not coming up with an example right now :\

**"_utils"** are for everything that isn't related to the common framework. For this example that means all non-Angular files. It's important to separate these, not just visually for readability, but to make it easier to include or exclude them during the task runner's build process. I prefer to use an underscore to keep this folder as split from the rest of the pack as possible.

<a id="markdown-scss" name="scss"></a>
### SCSS

*Read: CSS preprocessor files*

The common convention with Sass is to have a main file that is an index of the external files it needs to import when it's compiled. Here is an example of a Sass directory:

* &#128194; scss
    * &#128196; _colors.scss
    * &#128196; _fonts.scss
    * &#128196; _footer.scss
    * &#128196; _grid.scss
    * &#128196; _header.scss
    * &#128196; _home.scss
    * &#128196; _loading-spinner.scss
    * &#128196; _mega-menu.scss
    * &#128196; _profile.scss
    * &#128196; app.scss

Following the same concepts from "scripts" here's how I would sort these files:

* &#128194; scss
    * &#128194; components
        * &#128196; _loading-spinner.scss
        * &#128196; _mega-menu.scss
    * &#128194; general
        * &#128196; _fonts.scss
        * &#128196; _layout.scss
    * &#128194; mixins
        * &#128196; _grid.scss
    * &#128194; partials
        * &#128196; _footer.scss
        * &#128196; _header.scss
    * &#128194; variables
        * &#128196; _colors.scss
        * &#128196; _columns.scss
    * &#128194; views
        * &#128196; _home.scss
        * &#128196; _profile.scss
    * &#128196; app.scss

For the most part there will only be one Sass file per component or concept, so a single level of nesting is good enough. Most of this organization is self-evident (loading-spinner and mega-menu in the "components" folder), but the "mixins" and "variables" directories are worth discussing.

**"variables"** should be single-responsibility collections of Sass variables used throughout the project. For example, the "\_grids.scss" file we started with may have contained variable declarations for column widths, for example. These variables should be separated and put into their own file. In this case, I would create a "\_columns" file so it's clear where those global values are defined.

**"mixins"** should be single-responsibility collections of Sass mixins used throughout the project. For example, the "\_grids.scss" file we started with may have contained classes for controlling page layout and defined the mixins it used to calculate column width, for example. Those mixins should be parsed out to a file for the "mixins" folder, and its classes should go into a "\_grids" or (what I prefer) a "\_layouts" file. 

The "variables" and "mixins" files contain important, global helpers, so they will be imported first in the main "app.scss" file. Since mixins will often use global variables, they should be imported in the order of *variables* then *mixins*.

<a id="markdown-templates" name="templates"></a>
## Templates

Staying with the illustrative-but-in-no-way-unique example of Angular, route views and Directives often declare an external template file that will be loaded in. I suggest keeping these files separated in their own directory.

A common convention is to keep the HTML files with the JavaScript in the "scripts" directory, but I find it's more intuitive to keep them just as separated as the Sass files. Aside from improving the readability of the project, this has the added benefit of fascilitating task runner functionality.

Sorting template files is easy once the scripts are in order. Here is an example of some template files we could expect to accompany the example scripts:

* &#128194; templates
    * &#128196; footer.tmpl.html
    * &#128196; header.tmpl.html
    * &#128196; home.tmpl.html
    * &#128196; loading-spinner.tmpl.html
    * &#128196; mega-menu.tmpl.html
    * &#128196; profile.tmpl.html

With such a small amount of template files it's tempting to leave them like this, but as soon as the project has fleshed out even a little bit more, pushing the templates to nested folders will make a lot of sense and take more time and effort to correct all of the existing template location declarations in the scripts. So, just nest the templates from the start, future you will be grateful.

* &#128194; templates
    * &#128194; components
        * &#128194; loading-spinner
            * &#128196; loading-spinner.tmpl.html
        * &#128194; mega-menu
            * &#128196; mega-menu.tmpl.html
            * &#128196; <b>mega-menu-child.tmpl.html</b>
    * &#128194; partials
        * &#128194; footer
            * &#128196; footer.tmpl.html
        * &#128194; header
            * &#128196; header.tmpl.html
    * &#128194; views
        * &#128194; home
            * &#128196; home.tmpl.html
        * &#128194; profile
            * &#128196; profile.tmpl.html

Now each template is separated out by concern and then housed in their parent thing's folder. Nested views, child Directive, they all have a place to go related to where they are used. Even if a folder only has one file in it, follow the pattern to leave yourself open to automating namespaces either in your application scripts or in the task runner: "home": "home/home.tmpl.html", "footer": "footer/footer.tmpl.html". Sweet pattern, bruh.  

<a id="markdown-committing-app-content" name="committing-app-content"></a>
## Committing "app" content

Some people leave stuff that doesn't require the task runner in a "app" folder. If you're not running a compression task on images, for example, it's tempting to keep them in the "app" folder's asset directory.

My suggestion is to leave all of it in "src", even stuff that will only ever require the *copy* task to move from "src" to "app". I suggest this for these reasons:

1. "That file doesn't need a task runner" ...***yet.***
2. If nothing needs to permanently live in the "app" folder, your *clean* task is as simple as deleting that entire folder.
3. If nothing needs to permanently live in the "app" folder, you don't have to *commit* the "app" folder. A good project should rely on the build to produce the exact same environment for everyone who runs it. It's like a "some assembly required" label except the only assembly required is you push the Go button on a task runner.


