# Airbnb CSS / Sass Styleguide

*A mostly reasonable approach to CSS and Sass. Forked from Airbnb, and tweaked to suit ourselves a little better.*

## Table of Contents

  1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
  1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
  1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
    - [Media Queries](#media-queries)
  1. [Folder Structure](#folder-structure)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names. Underscores are OK if you're using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### OOCSS and BEM

We encourage some combination of OOCSS and BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**OOCSS**, or “Object Oriented CSS”, is an approach for writing CSS that encourages you to think about your stylesheets as a collection of “objects”: reusuable, repeatable snippets that can be used independently throughout a website.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**Example**

```html
<article class="listing-card listing-card--featured">

  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```css
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

  * `.listing-card` is the “block” and represents the higher-level component
  * `.listing-card__title` is an “element” and represents a descendant of `.listing-card` that helps compose the block as a whole.
  * `.listing-card--featured` is a “modifier” and represents a different state or variation on the `.listing-card` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

JS Hooks using data-attributes are also acceptable:

```html
<button class="btn btn-primary" data-request-to-book>Request to Book</button>
```

We allow this because you may want to make your JS Hooks more flexible by using a data-attribute's value. For example:

```html
<button class="btn btn-primary" data-ajax-submit="#request-to-book-form">Request to Book</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. `@include` declarations

    Grouping `@include`s at the beginning follows the programming practice of including "dependencies" first, and provides added flexibility if/when we want to overwrite rules defined in the `@include`.

    ```scss
    .btn-checkout {
      @include display-font;
      font-weight: 700;
      // ...
    }
    ```

2. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-checkout {
      background-color: #0f0;
      font-weight: 700;
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      @include display-font;

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

When creating a mixin, never use media queries inside of your mixin. Redefine the include definition in its own media query at the selector level.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

Where necessary, :before, :after, and @media queries can be added as a 4th level to a nesting chain for the 3rd level selector.

```scss
.page-container {

  .content {

    .profile {
      // STOP!

      &:before,
      &:after {
          content: ':before and :after elements can be used as a 4th 'selector';
      }

      @media #{$desktop} {
          // Media queries can be used as a 4th level 'selector'
          display: block;
      }
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

### Media Queries

*Always nest media queries in selectors*

*Never nest selectors in media queries*

**Good**

```scss
.mobile-menu {
  display: block;

  @media #{$desktop} {
    display: none;
  }
}
```

**Bad**

```scss
.mobile-menu {
  display: block;
}

@media #{$desktop} {

  .mobile-menu {
    display: none;
  }
}
```

Nesting media queries in selectors, and not the other way around, will keep your code readable, and save you from queryception and compilation bugs. For example:

**Bad**

```scss
.header {

  .desktop-menu {
    display: none;
  }

  @media #{$desktop} {

    .desktop-menu {
      display: block;

      // ... a bunch of other rules and selectors ...

      .menu-link {

        @media #{$tablet} {
          // Oh no! This media query is nested in a different media query!
        }
      }
    }
  }
}
```

## Folder Structure

We structure our Sass files in a certain way to keep our projects consistent and easy-to-use. The folder structure is this:

- `scss/core`

  Where we put the core parts of our project Sass. All/most other files will depend on these variables, mixins, functions, etc: color palette, fonts, utility functions, utility mixins, etc.

- `scss/vendor`

  Where we put our vendor library Sass: things like Foundation, Fontawesome, Slick, etc.

- `scss/components`

  Where we put our reusable Scss components. These might contain a mixin or a presentational CSS class (OOCSS, BEM, or otherwise). Whatever the case, the filename should match the output. For example, `button.scss` should output `.button {}` or `@mixin button {}`

- `scss/snippets`

  Where we put our styles applied to a specific `.liquid` snippet. The file should define styles for that class, should have the same name, and should exaclty match a corresponding liquid snippet. Take the following example:

  - there is a liquid snippet in the theme: `/theme/snippets/newsletter-popup.liquid`
  - that file is wrapped in a div: `<div class="newsletter-popup">Newsletter popup stuff in here</div>`
  - we have a Sass file: `/src/scss/snippets/newsletter-popup.scss`
  - that file defines styles for the `.newsletter-popup` class: `.newsletter-popup { /* styles in here */ }`

- `scss/templates`

  Where we put styles for our liquid templates. These follow the same conventions as with snippets, but with one difference:

  - `.liquid` template files get wrapped with a slightly different classname.
  - For example: `templates/page.liquid` should contain `<div class="page-template">Page template in here</div>`
  - similarly, `scss/templates/page.scss` should output `.page-template { /* styles here */}`

- `scss/layout`

  Where we put our layout styles. This should define the relationships between the various parts of the page, IE. header, body, and footer.

## Formatting Your Sass files

Keeping our Sass file format consistent will reduce Git merge conflicts and all-around make our lives easy. You can set the correct indentation for your Sass files by default in Sublime. We'll do this using syntax-specific settings files for sublime.

1. Add the `CSS.sublime-settings` and `Sass.sublime-settings` files to your computer's `~/Library/Application Support/Sublime Text 3/Packages/User/` folder.

When you get a file that's improperly formatted, you can simply:

1. make sure your file is using the correct syntax highlighting (select syntax from menu in bottom-right)
2. highlight all text in file (`cmd + shift + a`)
3. re-indent (`cmd + shift + r`)
