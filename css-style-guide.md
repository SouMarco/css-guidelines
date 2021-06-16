#CSS Style Guide


## 1. Introduction

To keep the CSS code written for the project consistent and maintainable, we provide CSS guidelines to follow during implementation and maintenance. 
The aim of these guidelines is to speed up development, make it easy for new developers to start working on the project, keep the code as it was written by a single developer, keep files short and readable.

### 1.1	Preprocessor

The CSS code will be written using <a href="https://sass-lang.com/" target="_blank">**SASS preprocessor**</a> with SCSS syntax. We don't use **LESS** or any other preprocessor.

### 1.2	Tabs and spaces

For indentation and formatting of the code, **we don’t use tabs but spaces**. Please set up your code editor with these settings:

 - **no tabs**;
 - **tab size = 4**;
 - **indent size = 4**.

Spaces are the only way to guarantee code renders the same in any person's environment. Please setup your VS Code to adopt these formatting rules.

### 1.3	HTML inline styles

**Never** write inline style in the HTML code.
Inline styles generate critical **<a href="http://www.smashingmagazine.com/2007/07/27/css-specificity-things-you-should-know/" target="_blank">specificity problems</a>**
that might be very difficult to fix and impose the use of `!important`.

### 1.4 Avoid undoing/overriding

**You're undoing CSS when you are writing declarations that override other declarations written somewhere else in the code.** This means that HTML elements are affected by too many conflicting CSS declarations and you'll need to rely on specificity to style correctly the element.

**Undoing is a bad way of coding** and we must always minimize undoing from the above levels. This typically happens in projects where we start with a CSS framework and then we realize we are writing a lot of declarations to overwrite the default framework's style (see 2.6 External frameworks).

### 1.5 80 characters wide

Where possible, limit CSS files width to 80 characters. Reasons for this include:

  - the ability to have multiple files open side by side;
  - viewing CSS on sites like GitHub, or in terminal windows;
  - providing a comfortable line length for comments.

### 1.6 Style Lint

SASS files must be verified with the linting tool <a href="http://stylelint.io/" target="_blank">Style Lint</a>. Style linting must be set up as a task in the task manager and run whenever a SASS file is modified. Our stylelint file is available <a href="https://github.com/gandreini/css-guidelines/blob/master/stylelintrc" target="_blank">here</a>.

### 1.7	HTML 5

HTML 5 tags must be used when writing HTML markup. These semantic elements clearly describe their meaning to both the browser and the developer.

---

## 2. SASS file organization

Our approach to managing CSS of the project is based on Modular CSS on which you can find more information <a href="https://spaceninja.com/2018/09/17/what-is-modular-css/?utm_source=CSS-Weekly&utm_campaign=Issue-332&utm_medium=email" target="_blank">here</a>. We also adopt the Inverted Triangle CSS (ITCSS) philosophy, in a slightly simplified version. More info about ITCSS <a href="https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/" target="_blank">here</a>.

SASS files are organized in 5 main folders that organize them in a logical structure:

**Generic, Global, Components, Layouts, Utilities**

### 2.1 Generic

**Files that don't directly style elements in the HTML**: **color-scheme**, **variables**, **mixins**. Examples of **mixins** include:

A mixin to keep floating elements consistent, without using `overflow: hidden;`:

	@mixin consistent {
	    &:after {
	        content: ' ';
	        clear: both;
	        display: block;
	    }
	}

A mixin to use icon fonts:

	@mixin icon-font($icon) {
	    content: $icon;
	    font-family: 'icon-font';
	    speak: none;
	    font-style: normal;
	    font-weight: normal;
	    font-variant: normal;
	    text-transform: none;
	    line-height: 1;
	    -webkit-font-smoothing: antialiased;
	    -moz-osx-font-smoothing: grayscale;
	}

### 2.2 Global

Common elements that style HTML tags. **No use of classes in here**, only HTML tags. Some examples: **normalize**, **typography**, **forms**, **tables**.

### 2.3 Components

Styles for components (or modules). What is a component? **A component is a distinct, independent unit**, that can be combined with other components to form a more complex structure. **Components are built to be reusable** in different contexts, can be nested one into the other or directly inserted into layouts. Components' style is also **independent** of the parent elements that wrap them.

### 2.4 Utilities

Utilities and helper classes with the ability to override the styles defined in Global, Components and Layouts. Utilities include classes for **hidden elements**, **label and value** elements to show metadata, classes to set up a **sticky footer**.

### 2.5 External frameworks

If using a framework like Bootstrap and Foundation import the SASS files in the main **style.scss** and select only the files you really need (don't import the whole CSS). Be aware that using the theming options of the framework (like bootstrap-theme of Bootstrap) will add another layer of style.

### 2.6 Color scheme

It's a good habit to use a limited set of colors: using too many colors can lead to having a very incoherent interface that confuses the user. For this reason, we have a **color-scheme.scss** file that contains the variables **of all the colors used in the project**. This will limit the number of colors used that should be less than 15, including all the possible shades of grey used for borders and backgrounds. The file should be imported before the **variables.scss**.

### 2.7 Style.scss file

Outside the folders **Generic, Global, Components, Layouts, Utilities** we create the **style.scss** file which is responsible for importing all the needed SCSS files (that will be compiled by SASS).

The **style.scss** file starts with a comment where we write the name of the project and we explain what this file is responsible for:

	//
	// <NAME_OF_THE_PROJECT>
	//
	// Sass styles import for whole project
	//

The next section is where we import external libraries (e.g. Bootstrap). This is how it looks like:

	// ------------------------------------ //
	// #LIBRARIES
	// ------------------------------------ //
	@import "jspm_packages/npm/hint.css@2.2.1/src/hint.scss";
	@import "jspm_packages/npm/susy@2.2.9/sass/susy";

Then we have 5 sections for **Generic, Global, Components, Layouts, Utilities** which are opened by a comment and followed by all the **import** instructions. **Imports are ordered in alphabetical order to make it easier to check if a file is imported or not**.

Here we provide a partial example of the **components** import section:

	// ------------------------------------ //
	// #COMPONENTS
	// ------------------------------------ //
	@import "components/boxview";
	@import "components/boxview-item";
	@import "components/buttons";
	@import "components/carousel";
	@import "components/document-reader";

The other sections (**Generic, Global, Layouts, Utilities**) will look the same.

---

## 3. Naming conventions

Naming conventions in CSS are useful in making your code consistent and more informative. **Name something based on what it is, not on how it looks or behaves**. Never use a class like `.red`: this will cause you problems the day you want that element to be green. Never use a class like `.left-bar`: this won't make sense the day you decide to move that bar on the right.

**Camel case must not be used for classes**.

### 3.1 Class prefix

In some projects, our CSS selectors could conflict with other stylesheets: this happens when we use a CSS framework or we are developing an application that must be embedded in other HTML pages.

In these cases it's a good habit to add a short **prefix** to all our classes to avoid any kind of conflict: the prefix should be short (2 or 3 letters) and delimited by a hyphen `-`. If the project is called **MY PROJECT**, we will add a prefix `mp-` to all the classes and the pagination component will be:

```
<div class="mp-pagination">
	...
</div>
```

---

## 4. Formatting

Before we discuss how we write our rulesets, let’s first familiarise ourselves with the relevant terminology:

    [selector] {
        [<--declaration--->]
        [property]: [value];
    }

Here are some formatting guidelines:

  - a **space before** the opening brace **( { )**;
  - the opening brace **( { )** on the **same line as the last selector**;
  - the first declaration **on a new line after the opening brace** **( { )**;
  - **properties and values always on the same line**;
  - a **space after** the property–value delimiting colon **( : )**;
  - each **declaration on its own new line** (line breaks between declarations);
  - the **closing brace** **( } )** on its own **new line**;
  - each **declaration indented** by four (4) spaces.

Let’s see an example:

    .footer, .footer-bar {
        display: block;
        background-color: @color-page-background;
        color: @color-footer-text;
    }

Avoid specifying **units for zero values**:

    margin: 0; /* Good */
    margin: 0px; /* No good */    

For nested ruleset, we also leave a blank line before the nested ruleset. Here’s an example:

    .footer {
        color: @color-footer-text;

        .bar {
            color: @color-footer-bar-text;
        }
    }

Nesting in SASS should be avoided wherever possible and used only when necessary to have the desired specificity: as a rule of thumb, we have the root class at the beginning of each SCSS file and all the child elements at the first level.

### 6.1 Whitespaces

We have **one** empty line between rulesets and **two** empty lines at
the end of each main section of code.

### 6.2 Colors

We use hex color codes (`#f0f0f0`). Short hex colors and color names (e.g. **red**) are not allowed. Letters in the hex color must be lowercase.

---


## 7. Comments and code sections

CSS is not telling its own story very well and **it is a language that benefits from being heavily commented**. You should comment anything that isn’t immediately obvious from the code alone: there is no need to tell someone that `display: none;` will hide an element, but if you’re using `overflow: hidden;` to clear floats — as opposed to clipping an element’s overflow — this is probably something worth documenting.

In SASS we can use comments in two different ways:

    /* */

and

    //

The main difference is that the **first will be processed and included in the resulting .css file** while the second **will be ignored by the preprocessor**. For this reason, we must use the `/* */` syntax for comments we want to include in the .css file while we use `//` for comments we don’t want to be compiled.
This will make no difference when we compile a **minified** and no-comments version of the file for production, but while we’re developing we’ll probably have a not-minified CSS file and comments can be very useful.

A clear example is the **variables.scss**: this will be transparent in the compiled CSS file so it’s a good practice to insert all comments using `//` otherwise, there will be comments floating around with no related code.

We identify **four** different types of comments.

### 7.1 Document level comments

These comments are **mandatory** and must explain what the current SASS document is about: we place them at the beginning of each SASS file.

    /**
     * DOCUMENT TITLE
     *
     * Text describing in detail what this file is doing...
     */

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    //
    // DOCUMENT TITLE (COMMENT HIDDEN FROM COMPILER, LIKE VARIABLES)
    //
    // Text describing in detail what this file is doing...
    //
        

### 7.2 Section level comments

The CSS declarations inside each file must be divided and organized in logical sections.
These sections must be introduced by a **mandatory** comment with a title of the section.
Only if the content is extremely hard to understand reading the code please provide some additional lines of text.

    /* ------------------------------------ *\
       #SECTION-TITLE
    \* ------------------------------------ */

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    // ------------------------------------ //
    // #SECTION-TITLE
    // ------------------------------------ //

### 7.3 Selector level comments

These comments are inserted before the selector: they are not always mandatory but needed when writing CSS that's not easily understandable.

Example:

    /* This set of declarations is doing this and that */
    .selector {
    }

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    // This set of declarations is doing this and that
    .selector {
    }

### 7.4 Inline comments

These are comments written at the level of each CSS declaration that we
need when writing hard-to-understand code. We write it like this:

    color: blue; /* The text color id blue */

If we want to write the same comment but this must not be compiled by the preprocessor, please write it this way:

    color: blue; // The text color id blue


---


## 8. Media Queries

Media queries **shouldn’t be written in a separate file** but divided in each SASS file to define media queries behavior for each specific component, section or layout.


---


## 9. Specificity

Keep **specificity low**.

There will always be times you need to override the style of an element, so the lower the specificity is on a selector, the easier it is to override it. Not only that, but override in such a way you might even be able to override it again without going crazy with an **ID selector** or **!important**.

To keep specificity low:

- **limit nesting** in SCSS files. The rule of thumb for components and layouts is to have a two-level nesting.
- never use **!important**;
- never use **ID selectors**.

---

## 10. Sizing

Units and sizes must be set using pixels.

**Font-size** will be expressed in pixels too (why? <a href="https://hackernoon.com/rems-and-ems-and-why-you-probably-dont-need-them-664b9ce1e09f" target="_blank">read here</a>).

**Line-height**: unit-less **line-height** is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.


---


## 11. Resources

  - <a href="http://cssguidelin.es" target="_blank">High-level advice and guidelines for writing sane, manageable, scalable CSS</a>
  - <a href="http://codepen.io/chriscoyier/blog/codepens-css" target="_blank">CodePen's CSS</a>
  - <a href="http://blog.trello.com/refining-the-way-we-structure-our-css-at-trello/?utm_source=CSS-Weekly&utm_campaign=Issue-128&utm_medium=email" target="_blank">Refining The Way We Structure Our CSS At Trello</a>
  - <a href="https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06" target="_blank">Medium’s CSS is actually pretty f***ing good.</a>
  - <a href="https://github.com/evernote/sass-build-structure" target="_blank">Evernote SASS build structure</a>
  - <a href="http://maintainablecss.com/" target="_blank">Maintainable CSS</a>
