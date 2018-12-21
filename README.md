# SCSS Base
A starting point for SCSS files and file architecture for any web project.

## File structure
```
├── app.scss
├── base
│   ├── _base.scss
│   ├── _fonts.scss
│   ├── _forms.scss
│   ├── _helpers.scss
│   ├── _layout.scss
│   ├── _lists.scss
│   ├── _media.scss
│   ├── _typography.scss
│   ├── _unicode.scss
│   └── _variables.scss
├── modules
├── reset
│   ├── _layout.scss
│   └── _normalize.scss
└── utilities
    ├── _skip-nav.scss
    └── _utilities.scss
```

## Overview
Use this as a springboard for your own projects so you're not mired in the initial setup of your SCSS files. File setup can take a lot of time and be quite intimidating if you're not sure where to start. My setup is not the paragon of SCSS file architecture (that doesn't exist), but I'm hoping it's a helpful tool to start you off so you can define a structure that fits your needs.

The `.scss` files are fairly agnostic with some recommendations here and there. The `_variables.scss` probably has the most "stuff" already built into it. This is **not a boilerplate**; edit to your liking, move things around, delete anything -- do what works for *you*.

With that said, here's some further information and recommendations based on my experience:


### `app.scss`
This is your vehicle to import all of your other [partials](https://sass-lang.com/guide#topic-4). This is the file that will be compiled into CSS with whatever tool you're using for that. `app` is a fairly generic name, but it can be renamed to whatever fits your project, just as long as it's recognizable. I've named this file `main.scss` and `config.scss` in other projects.


#### File order
Because CSS *cascades*, the `@import` order will matter.
```scss
// Resets
@import "reset/normalize";
@import "reset/layout";

// Dependencies
@import "bourbon";

// Base
@import "base/base";

// Utilities
@import "utilities/utilities";

// Vendor
// e.g. @import "vendor/bootstrap";

// Modules
// e.g. @import "modules/hero-card";
```
- **[Resets](#resets)** go at the top to provide a clean slate for your layout code. Putting it at the bottom might override some of your custom styles :(.
- **[Dependencies](#dependencies)** are any external library or framework that your custom SCSS may be couple with. This could be code that provides mixins, functions, variables, etc.
  - In this file system, I use [Bourbon](https://www.bourbon.io/).
- **[Base](#base)** files are local and custom-written dependncies for your scss. Base files include many global classes, helpers, and variables you'll need for your project (I've also seen this called `globals` instead of `base`).
- **[Utility](#utilities)** files have isolated classes that can be added to any HTML element and are usually fairly agnostic (e.g. `.u-padding-left--large` gives `50px` of left padding to *any* element). Utility classes can also go at the end of your structure, but I find it more readable here.
- **[Vendor](#vendors)** files are libraries that your custom SCSS is *not* dependent on. These libraries are usually straight imports so you can use their classes in your HTML. [Bootstrap](https://getbootstrap.com/) is an example of this.
- **[Modules](#modules)** are a catch-all of `@import`s. But I'd encourage you to break out more aptly-named folders if you're working on a particularly large project with lots of elements to style or a project that will need to scale. If it's taking you longer than a few seconds to read through the list of `@import`s in your list, it may be time to think about grouping some files into folders and having a file to import those other files in that folder (much like `_base.scss`).
  - Keep the `@import`s in **alphabetic order** for readability

***

### Resets
- I use [`normalize`](http://nicolasgallagher.com/about-normalize-css/), but some folks use [`reset`](https://meyerweb.com/eric/tools/css/reset/), and some folks don't use a reset at all!
- `layout.scss` is to provide some [box-sizing](https://css-tricks.com/box-sizing/) declarations to all elements. This appears in some resets, but I like to separate mine out.

***

### Dependencies
It can be incredibly helpful to use SCSS dependencies, especially if you want to [DRY](https://medium.com/backticks-tildes/keeping-your-scss-dry-5211a99be15c) out your scss. While your code may require writing some [custom helpers](#_helpersscss), you may find a 3rd party vendor has already done the heavy-lifting for you. These may provide mixins, functions, or any helpers for your custom SCSS. See what works for you, but don't fill this space with *too many* dependencies.

#### [Bourbon](https://www.bourbon.io/)
This is a pretty great lightweight tool full of handy mixins and functions for your project. I've used it on most of my projects and I've found it very helpful in cutting down on the amount of SCSS I'm writing (readability FTW!).

You'll need to [install it](https://github.com/thoughtbot/bourbon#installation) in some manner (gem, npm, bower, etc) and the import naming might differ depending on what you use.

**NOTE:** The `_skip-nav` utility uses a Bourbon mixin, so if you're not using this library but still want to use this utility, you'll need to write out the CSS fully.

***

### Base
These are the global layouts, utilities, mixins, and variables for your project.

#### `_base.scss`
`_base.scss` serves as a way to import all the files within that folder. Much like, `app.scss` there is a reason for the order of the files due to the nature of the cascade and dependencies within this folder.
- `_helpers`, `_variables` and `_fonts` should remain in that order.
- Everything underneath that chunk can go in alphabetic order.
- If you add any files that any other file in that folder is dependent on (i.e. uses a mixin or an extend from it), then put it within the top chunk of files.

#### `_fonts.scss`
Throw your font imports in here! I use a [bourbon mixin](https://www.bourbon.io/docs/latest/#font-face) for the import (the file types are defined in `_variables.scss).` If you're using google font imports or other files that aren't local to your asset folder, you can define them here.
- Wondering what `font-display` is? [Learn about it here!](https://css-tricks.com/font-display-masses/)

#### `_forms.scss`
Any global form styling should go here.
- Currently there's only some styling on `select`s and `button`s to add a pointer cursor to them because I always have to add those in!
- I find it's cleanest to keep definitions to *tags* instead of *classes*

#### `_helpers.scss`
A variety of mixins and functions I've pulled from other sources and written myself that I've found useful.

#### `_layout.scss`
This file is pretty open to intepretation (which is why it's left blank), but often I find myself putting global grid patterns or container declarations in there.

#### `_lists.scss`
Any global styles or resets for `ul`s, `ol`s, `li`s, `dd`s, and `dt`s.
- I find it's cleanest to keep definitions to *tags* instead of *classes*

#### `_media.scss`
This includes a few resets for how image media is displayed as well as [some accessibility additions for SVGs](https://css-tricks.com/accessible-svgs-high-contrast-mode/).

#### `_typography.scss`
This file is for global typographic definitions. Currently this has some base definitions for the `body`, but can be filled out more to define `h1`s, `h2`s, `p`s, or any tag that serves text.
- I find it's cleanest to keep definitions to *tags* instead of *classes*
- Remember these are *globals* so try to keep things as low-fi as possible so you're not stuck overriding a lot in your module class declarations.

#### `_unicode.scss`
This is a list of variables for a handful of unicode characters that are used fairly often.

#### `_variables.scss`
This is a base file for all variables. There are a lot of variables already defined, but this is just mean to be a starting point that can work for many projects. Edit and add to this depending on your project's needs.
- I find the easiest naming convention for variables uses a modified [BEM](http://getbem.com/introduction/) approach: `block`-`modifier`. So instead of using `$blue` and `$green`, you would define those colors as `$color-blue` and `$color-green`. This keeps variables predictable and readable.

***

### Utilities
Utilities are files that have isolated classes that can be added to any HTML element and are usually fairly agnostic.
- Some utility files only have one class declaration (e.g. `_skip-nav.scss` only has 1 class in that file), but that's totally fine -- being able to scan and find your utility is better than searching through a bulky file.
- I've seen many people define their utility classes by prepending them with a `u-` (e.g. `.u-padding-left`). I haven't done that here because most of my projects aren't so large that I end up with a lot of utility classes, but if you find that you have a lot of these classes or need to scale to that, it might be a good practice to put in place!

### `_utilities.scss`
Much like `_base.scss`, this serves as a way to import all the files within this folder.
- File imports should be in alphabetic order

### `_skip-nav.scss`
A class that can be put on a [skip nav link](https://webaim.org/techniques/skipnav/). This is the bare-bones of it and styling can be added to the *focus* state.

***

### Vendors
Much like the dependencies, this is a 3rd party installation, either through a package or downloaded locally to your machine. These files should **not** provide mixins, functions, or any helpers for your custom SCSS. They will likely be a straight import that allows you to use its classes in your HTML.
- If you're using any vendor code locally in your files (i.e. not through a package), you should make a `vendor` folder and call files from there in your `app.scss`.

***

### Modules
Any custom-defined modules go in here. Modules is a catch-all name and if your project is large or you're just very organized, you may want to define more folders with relevant files, or re-name `modules` to something else. 
- If you end up with multiple folders (e.g instead of `modules` you may have `hero` and `articles`), it may make sense to have a file within these folders to import all the files within so that `app.scss` stays readable.

Here are a few examples of module lists in `app.scss`:

**Simple list of files**
```scss
...
// Modules
@import "modules/about";
@import "modules/body";
@import "modules/content";
@import "modules/events";
@import "modules/hero";
@import "modules/footer";
@import "modules/nav";
```

**Complex list of files and folders by type of module**
```scss
...
// Admin
@import "admin/choice-card";
@import "admin/dropdown";
@import "admin/nav-tools";
@import "admin/options";

// User
@import "user/hero";
@import "user/login-form";
@import "user/messaging";

// Landing page
@import "landing/carousel";
@import "landing/cta";
...
```
**Complex list of files and folders by use within the layout**
```scss
...
// Template
@import "template/footer";
@import "template/header";
@import "template/menu";

// Layout
@import "layout/about";
@import "layout/article";
@import "layout/media-layout";
@import "layout/topic";

// Components
@import "components/annotations";
@import "components/aside-list";
@import "components/audio";
@import "components/discuss-cta";
@import "components/hero";
@import "components/newsletter";
...
```
