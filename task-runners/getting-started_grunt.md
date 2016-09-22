![Grunt](../images/grunt.png "Grunt")

# Getting Started with Grunt

Grunt uses pre-made and custom package tasks to define a firing order of actions that need to run on a project's files. The best way to talk us through all of this is going to be by example, so buckle up, we're about to scaffold a project!

* * *

<!-- TOC -->

1. [System setup](#system-setup)
    1. [Installing NodeJS/npm](#installing-nodejsnpm)
    2. [Installing packages](#installing-packages)
2. [Project setup](#project-setup)
    3. [Make a project directory](#make-a-project-directory)
    4. [npm init](#npm-init)
    5. [Project folders](#project-folders)
3. [Grunt setup](#grunt-setup)
    6. [Installing](#installing)
    7. [Scaffolding](#scaffolding)
    8. [Configuring](#configuring)
        1. [Install some Grunt packages](#install-some-grunt-packages)
    9. [Registering main tasks](#registering-main-tasks)
4. [Running Grunt](#running-grunt)

<!-- /TOC -->

* * *

<a id="markdown-system-setup" name="system-setup"></a>
## System setup

> **Just a heads up, I'll be throwing out a lot of terminal command instructions, but I'm not going over basic terminal how-to's here.** These instructions are geared for *OSX*, but I'll keep it generic enough for Windows users when I can. Here's a [quick reference guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/4/html/Step_by_Step_Guide/ap-doslinux.html).

This project will need two programs to get started: Node and Grunt. You can check for these programs by running `npm -version` and `grunt -version`. If the results say something like "-bash: command not found", follow these steps to get going

<a id="markdown-installing-nodejsnpm" name="installing-nodejsnpm"></a>
### Installing NodeJS/npm

This one's pretty easy, go to [NodeJS.org](https://nodejs.org/en/), download the recommended package, click to activate the downloaded installer, and follow the prompts.

<a id="markdown-installing-packages" name="installing-packages"></a>
### Installing packages

Now that you've installed the Node Package Manager (npm), you can use it to install Node packages. These can be installed in two contexts: global or local.

Locally installed packages get associated with the current directory/location of your terminal. This is beneficial when used correctly and kind of confusing if used incorrectly. If you've installed a package you need to use globally but accidentally installed it locally (or vice versa), it can be frustrating to understand why it's not working.

**Local npm installs** looks like this: `npm install pkg-name`

**Global npm installs** looks this this: `npm install pkg-name -g` That `-g` tells npm to put the package in your user's root directory. Later when you try to use the package, Node's going to look for it locally, if it's not there it's going to look for the global package. If you accidentally installed it locally in some other location it won't find it and you'll probably cry.

> If you're having permissions issues or receiving errors that say something about "EACCESS", try John Papa's (guide to using npm global without sudo for OSX)[https://johnpapa.net/how-to-use-npm-global-without-sudo-on-osx/]. Once complete stop using `sudo` to do anything. If you're needing to use `sudo`, your bash stuff probably needs straightening out. Or you're a sysadmin. So what are you doing in this beginner's guide?

* * *

<a id="markdown-project-setup" name="project-setup"></a>
## Project setup

Okay, your machine is ready to build some sweet projects now. Let's just jump right in, we'll walk through new stuff as we build it!

<a id="markdown-make-a-project-directory" name="make-a-project-directory"></a>
### Make a project directory

First we're going to need a project folder. I assume you know how to make folders with Finder, but here's how to do it with the terminal *like a boss*.

1.  I like to keep my projects in a directory on my desktop so change your current directory to where ever you're wanting to build a new project folder.
    * `cd ~/Desktop/Projects`
2.  Now we'll make a shiny, new directory  
    * `mkdir project-name`
3.  Now retrieve a list of all the folders in your current directory. You should see "project-name" listing among them.
    * `ls`
4.  Let's get into the new project folder!  
    * `cd project-name`

<a id="markdown-npm-init" name="npm-init"></a>
### npm init

Now we'll lay the foundation for our project. Run `npm init` then follow the prompts. If you're unsure of an answer, just push Enter and npm will use a default answer. Don't over-think it, it's all editable later. Once you're done, you'll see that npm just created a "package.json" file in the project folder. Go ahead and open that up in your handy-dandy text editor and you should see something like this

    {
        "name": "project-name",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
            "test": "echo \"Error: no test specified\""
        },
        "author": "",
        "license": "ISC"
    }

This is the config file npm will use when building your project. These default values are either self-explanatory or not really important for now, so we can take this as read for now. If you're unhappy with any of your initial answers, this is the place to change it.

<a id="markdown-project-folders" name="project-folders"></a>
### Project folders

If you haven't already, please read the overview for [project structure](../project-structure/basic-conventions.md). If you have, then great, let's set this thing up. We'll need to create our site's skeleton folders so do it with Finder or go back to your terminal and, working from your project's directory, run these commands:

1.  Create your development directory  
    * `mkdir src`
2.  Change locations to that directory  
    * `cd src`
3.  Make directories for some basic assets: images, JavaScripts, and SCSS's  
    * `mkdir images scripts scss`
4.  Make base HTML, JavaScript, and SCSS files
    * `touch index.html scripts/app.js scripts/app.service.js scss/app.scss scss/_fonts.scss`
5.  Get any image and copy it into the src/images folder

Now your project folder should look exactly like this

* &#128194; project-name
    * &#128194; src
        * &#128194; images
            * &#128196; any-image.png
        * &#128194; scripts
            * &#128196; app.js
            * &#128196; app.service.js
        * &#128194; scss
            * &#128196; app.scss
            * &#128196; fonts.scss
        * &#128196; index.html
    * &#128196; package.json


<a id="markdown-grunt-setup" name="grunt-setup"></a>
## Grunt setup

<a id="markdown-installing" name="installing"></a>
### Installing

Let's install Grunt. Grunt's pretty big so that may take a minute. From the project's root directory run:

`npm install grunt --save-dev`  

If you're getting unhelpful errors run the install with the **debug** and **verbose** parameters to get better info:

`npm install grun --save-dev --debug --verbose`

Assuming that installed okay, two noteable things just happened: Grunt was installed in the "node-modules" folder in the project's root and the `--save-dev` parameter added Grunt as a devDependency to the "package.json" file:

    /** packages.json */
    {
      ...
      "license": "ISC",
      "devDependencies": {
        "grunt": "^1.0.1"
      }
    }

Dependencies defined in "package.json" tell npm what dependencies to install when someone installs the project. Want to see it in action? Go delete the "node-modules" folder from the project, then run `npm install` again. Ta-da, the "node-modules" folder and Grunt are back! I'm trying to make that sound awesome, but the reality is pretty anti-climactic.

<a id="markdown-scaffolding" name="scaffolding"></a>
### Scaffolding

Now we need to create the task commands we want Grunt to handle for us. Grunt gets these commands from a specific file that we need to create. From the project's root, type the following command:

`touch Gruntfile.js`

Now open the newly creaed "Gruntfile.js" in a text editor and copy/paste this skeleton code. We'll walk through what's actually happening, don't worry.

    'use strict';

    module.exports = function (grunt) {

        grunt.initConfig({
            
            /** Tasks start */

            /** Tasks end */

        });

        /** Default task */
        grunt.registerTask('default', []);

    };

The thing that sets Grunt apart from other task runners is how we define and invoke its tasks. I like Grunt because it has an intuitive and well-defined split between its configuration and registration stages.

<a id="markdown-configuring" name="configuring"></a>
### Configuring

Inside the `grunt.initConfig` argument, we're going to put configuration objects for all of the little tasks that we'll use to later define our official tasks.

For this example we're going to have Grunt run three of the most frequent tasks you would rather not have to do by hand (trustme).

<a id="markdown-install-some-grunt-packages" name="install-some-grunt-packages"></a>
#### Install some Grunt packages

First, we just have to install some packages. Don't worry, this one we can do with one command (don't forget `--save-dev` to define these as project dependencies!):

`npm install --save-dev grunt-contrib-copy grunt-contrib-concat grunt-contrib-compass grunt-contrib-watch`

Here's what those packages let us to do:

1.  [Copy images](#copy-images)
2.  [Concatenate JavaScript](#concatenate-javascript)
3.  [Compile SCSS](#compile-scss)
4.  [Watch our stuff for us](#watch-our-stuff-for-us)

Inside the `grunt.initConfig` section, copy/paste the following code for each task (**be sure they're comma separated!**):


<a id="markdown-copy-images" name="copy-images"></a>
##### Copy images

    copy: {
        images: {
            // allows us to use dynamic options like "cwd"
            expand: true,
            // make-pretend "src" is the root folder
            cwd: 'src',
            // grab every file that is in the "images" folder...
            src: 'images/**',
            // and copy it to "app"
            dest: 'app'
        }
    },

> <small>Check out [grunt-contrib-copy's docs](https://github.com/gruntjs/grunt-contrib-copy) for more advanced options!</small>


<a id="markdown-concatenate-javascript" name="concatenate-javascript"></a>
##### Concatenate JavaScript

    concat: {
        scripts: {
            // grab every file in the "scripts" folder with a ".js" extension
            src: ['src/scripts/*.js'],
            // merge all of that content into one file in the "app" folder
            dest: 'app/js/app.js',
        },
    },

> <small>Check out [grunt-contrib-concat's docs](https://github.com/gruntjs/grunt-contrib-concat) for more advanced options!</small>


<a id="markdown-compile-scss" name="compile-scss"></a>
##### Compile SCSS

    compass: {
        scss: {
            options: {
                // leaves CSS with line-breaks, spacing, and comments
                outputStyle: 'expanded',
                // Compass is smart, just point it at a folder...
                sassDir: 'src/scss',
                // and tell it where to spit out the results
                cssDir: 'app/css'
            }
        }
    },

> <small>Check out [grunt-contrib-compass's docs](https://github.com/gruntjs/grunt-contrib-compass) for more advanced options!</small>


<a id="markdown-watch-our-stuff-for-us" name="watch-our-stuff-for-us"></a>
##### Watch our stuff for us

    watch: {
        // watch "images" folder for changes
        images: {
          files: ['src/images/**'],
          tasks: ['copy:images']
        },
        // watch "scss" folder for changes
        scss: {
          files: ['src/scss/**'],
          tasks: ['compass:scss']
        },
        // watch "scripts" folder for changes
        scripts: {
          files: ['src/scripts/**'],
          tasks: ['concat:scripts']
        }
    }

> <small>Check out [grunt-contrib-watch's docs](https://github.com/gruntjs/grunt-contrib-watch) for more advanced options!</small>

* * *

<a id="markdown-registering-main-tasks" name="registering-main-tasks"></a>
### Registering main tasks

Here's the meat & potatoes of the whole thing. Now that we've configured the incremental tasks, we can register a main task that combines them all.

At the bottom of "Gruntfile.js" there's this line: `grunt.registerTask('default', []);` This is saying that if you run the command `grunt`, its default behavior will be to execute all of the tasks defined in the second argument array. So let's put some tasks in there.

<pre>grunt.registerTask('default', [
    <b>'copy'</b>,
    <b>'concat'</b>,
    <b>'compass'</b>,
    <b>'watch'</b>
]);</pre>

That array tells the *default* Grunt task to do those tasks, the ones we configured above, in that specific order.

To register custom tasks, follow the same pattern used by `default`. A common namespace for development environments is `build` and it typically follows the same task list used by `default`. Simply add another `registerTask` block below the `default` to accomplish this.

As an example, here's a `build` command that runs the tasks in a different order than `default`:

<pre>grunt.registerTask(<b>'build'</b>, [
    'concat',
    'compass',
    'copy',
    'watch'
]);</pre>

<a id="markdown-running-grunt" name="running-grunt"></a>
## Running Grunt

All together, your Gruntfile.js should now look like this:

* * *

    'use strict';

    module.exports = function (grunt) {

        grunt.initConfig({
            
            compass: {
                scss: {
                    options: {
                        outputStyle: 'expanded',
                        sassDir: 'src/scss',
                        cssDir: 'app/css'
                    }
                }
            },
            
            concat: {
                scripts: {
                    src: ['src/scripts/*.js'],
                    dest: 'app/js/app.js',
                },
            },
            
            copy: {
                images: {
                    expand: true,
                    cwd: 'src',
                    src: 'images/**',
                    dest: 'app'
                }
            },

            watch: {
                images: {
                    files: ['src/images/**'],
                    tasks: ['copy:images']
                },
                scss: {
                    files: ['src/scss/**'],
                    tasks: ['compass:scss']
                },
                scripts: {
                    files: ['src/scripts/**'],
                    tasks: ['concat:scripts']
                }
            }
            
        });

        /** Default task */
        grunt.registerTask('default', [
            'copy',
            'concat',
            'compass',
            'watch'
        ]);

        /** Build task */
        grunt.registerTask('build', [
            'concat',
            'compass',
            'copy',
            'watch'
        ]);
    };

* * *

Now that all the tasks are configured and the main task is registered, we can run the default Grunt command from the project root. Use **debug** and **verbose** to recieve extra console feedback to help troubleshoot any issues that arise.

`grunt --debug --verbose`

This command will run through the tasks we've assigned. Since we've told it to `watch`, the command will continue running, rerunning the appropriate tasks when a watched filetype is altered.

To end this command's watch and take your console back to neutral, simply press `ctril + c`.

To run the custom build task, use the same command but call for `build`:

`grunt build --debug --verbose`

