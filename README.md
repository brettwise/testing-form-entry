H5BP/Jade/Bower/Sass Best Practices/gulp-starter
============

# What is this?
This combines [html5boilerplate](https://github.com/h5bp/html5-boilerplate), [jade](https://github.com/jadejs/jade)-[gulp](https://www.npmjs.com/package/gulp-jade) mostly for templating, [Bower](https://github.com/bower/bower) and Sass linting from [sass-guidelin.es](http://sass-guidelin.es/#scss-lint). All a top of the wonderous [gulp-starter](https://github.com/greypants/gulp-starter) from someone called greypants. Pretty much... it takes the best of everything.

## tldr or just tell me how to get going with this damn thing.
alright already.

```
    git clone git@github.com:brettwise/jade-bower-h5bp-scss-linter-gulp-starter.git
    cd jade-bower-h5bp-scss-linter-gulp-starter
    bash gogo-static-site.sh
```

#### That bash script keeps you from having type all these.

```
    cd .. 
    mv jade-bower-h5bp-scss-linter-gulp-starter $sitename
    divshot init // followed by 5 divshot prompts
    name: (my-app-name)                                 
    root directory: (current) public
    clean urls: (y/n) y
    error page: (error.html)
    Would you like to create a Divshot.io app from this app?: (y/n) y
    rm -rf .git
    git init
    git add -A
    git commit -m "Initial commit for $sitename"
    npm install
    cd src/sass
    bower install
    cd src/js
    bower install
    subl .
    gulp
```

## brett’s rationale for creating this
I wanted something minimal and something that allowed me to use partials. I thought about a static site generator like Jekyll but in the end it was way more than I wanted. I just wanted partials. So I ended up settling on Jade for that purpose.

### To-Do
- [ ] Add in SCSS linter - https://www.npmjs.com/package/gulp-scss-lint
- [ ] Add Rules from Sass-Guidelines to above linter - http://sass-guidelin.es/#scss-lint
- [x] Change Sass files to Scss file & Rename Folder
- [x] Add in base bower config with things like normalize.scss & modernizr.
- [ ] Possibly add support for Sassdoc. - http://sassdoc.com/
- [ ] Possibly add support for creating and pushing to github via api. - https://stackoverflow.com/questions/2423777/is-it-possible-to-create-a-remote-repo-on-github-from-the-cli-without-ssh/10325316#10325316 - https://gist.github.com/robwierzbowski/5430952

### Branch Guide
sass-i-mean-scss-linting: This is the branch where I’m trying to add in scss linting per http://sass-guidelin.es/. I have stashed changes with the scss-lint npm package installed and inserted into sass.js for gulp but it keeps erroring out. Don’t know why. Do git stash apply to retreive changes.

## gulp-starter readme
Starter Gulp + Browserify project with examples of how to accomplish some common tasks and workflows. Read the [blog post](http://viget.com/extend/gulp-browserify-starter-faq) for more context, and check out the [Wiki](https://github.com/greypants/gulp-starter/wiki) for some good background knowledge.

Includes the following tools, tasks, and workflows:

- [Browserify](http://browserify.org/) (with [browserify-shim](https://github.com/thlorenz/browserify-shim))
- [Watchify](https://github.com/substack/watchify) (caching version of browserify for super fast rebuilds)
- [SASS](http://sass-lang.com/) (super fast libsass with [source maps](https://github.com/sindresorhus/gulp-ruby-sass#sourcemap), and [autoprefixer](https://github.com/sindresorhus/gulp-autoprefixer))
- [CoffeeScript](http://coffeescript.org/) (with source maps!)
- [BrowserSync](http://browsersync.io) for live reloading and a static server
- [Image optimization](https://www.npmjs.com/package/gulp-imagemin)
- Error handling in the console [and in Notification Center](https://github.com/mikaelbr/gulp-notify)
- Shimming non common-js vendor code with other dependencies (like a jQuery plugin)
- **New** Multiple bundles with shared dependencies
- **New** Separate compression task for production builds
- **New** Icon Font generation

If you've never used Node or npm before, you'll need to install Node.
If you use homebrew, do:

```
brew install node
```

Otherwise, you can download and install from [here](http://nodejs.org/download/).

### Install npm dependencies
```
npm install
```

This runs through all dependencies listed in `package.json` and downloads them to a `node_modules` folder in your project directory.

### The `gulp` command
To run the version of gulp installed local to the project, in the root of your this project, you'd run

```
./node_modules/.bin/gulp
```

**WAT.** Why can't I just run `gulp`? Well, you could install gulp globally with `npm install -g gulp`, which will add the gulp script to your global bin folder, but it's always better to use the version that's specified in your project's package.json.  My solution to this is to simply alias `./node_modules/.bin/gulp` to `gulp`. Open up `~/.zshrc` or `~./bashrc` and add the following line:

```
alias gulp='node_modules/.bin/gulp'
```
Now, running `gulp` in the project directory will use the version specified and installed from the `package.json` file.

### Run gulp and be amazed.

```
gulp
```

This will run the `default` gulp task defined in `gulp/tasks/default.js`, which has the following task dependencies: `['sass', 'images', 'markup', 'watch']`
- The `sass` task compiles your css files.
- `images` moves images copies images from a source folder, performs optimizations, the outputs them into the build folder
- `markup` doesn't do anything but copy an html file over from src to build, but here is where you could do additional templating work.
- `watch` has `watchify` as a dependency, which will run the browserifyTask with a `devMode` flag that enables sourcemaps and watchify, a browserify add-on that enables caching for super fast recompiling. The task itself starts watching source files and will re-run the appropriate tasks when those files change.

#### Icon Fonts

```
gulp iconFont
```

Generating and re-generating icon fonts is an every once and a while task, so it's not included in `tasks/default.js`. Run the task separately any time you add an svg to your icons folder. This task has a couple of parts.

##### The task
The task calls `gulp-iconfont` and passes the options we've configured in [`gulp/config.js`](https://github.com/greypants/gulp-starter/blob/icon-font/gulp/config.js#L27). Then it listens for a `codepoints` that triggers the generation of the sass file you'll be importing into your stylesheets. [`gulp/iconFont/generateIconSass`](./gulp/tasks/iconFont/generateIconSass.js) passes the icon data to [a template](./gulp/tasks/iconFont/template.sass.swig), then outputs the resulting file to your sass directory. See the [gulp-iconFont docs](https://github.com/nfroidure/gulp-iconfont) for more config details. You may reconfigure the template to output whatever you'd like. The way it's currently set up will make icons usable as both class names and mixins.

```sass
.twitter-button
  +icon--twitter // (@include in .scss syntax)
```

or 

```html
<span class="icon -twitter"></span>
```

#### gulp production

There is also a `production` task you can run with `gulp production`, which will re-build optimized, compressed css and js files to the build folder, as well as output their file sizes to the console. It's a shortcut for running the following tasks: `['images', 'minifyCss', 'uglifyJs']`.

### Configuration
All paths and plugin settings have been abstracted into a centralized config object in `gulp/config.js`. Adapt the paths and settings to the structure and needs of your project.


-- 

Social icons courtesy of [icomoon.io](https://icomoon.io/#icons-icomoon)</small>

Made with ♥ at [Viget](http://viget.com)!
