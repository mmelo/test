<img src="cobalt.png" height="100" width="100"/>

# Style Guidelines

*A mostly reasonable approach to CSS and Sass for the development of this toolkit*

## Useful URLs

- [Cobalt V2 Spec](https://talkdesk.atlassian.net/wiki/spaces/COB/pages/1154680579/Design%2BDocumentation)
- [Cobalt V1 Source](https://github.com/Talkdesk/cobalt_design/)
- [Cobalt v1 Documentation](https://cobalt-design.talkdeskapp.com/)


## Table of Contents

  1. ***[Terminology](#terminology)***
      - [Rule Declaration](#rule-declaration)
      - [Selectors](#selectors)
      - [Properties](#properties)
  2. ***[CSS](#css)***
      - [Formatting](#formatting)
      - [Comments](#comments)
      - [Naming & BEM](#naming-and-bem)
      - [ID Selectors](#id-selectors)
      - [Border & Outlines](#border-and-outlines)
  3. ***[Sass](#sass)***
      - [Syntax](#syntax)
      - [Ordering](#ordering-of-property-declarations)
      - [Variables](#variables)
      - [Mixins](#mixins)
      - [Extend directive](#extend-directive)
      - [Nested selectors](#nested-selectors)
  4. ***[Misc.](#misc)***
      - [Over-qualification](#over-qualification)
      - [Colors](#colors)
  5. ***[Component Styles folder structure](#component-styles-folder-structure)***
      - [Folder Structure](#folder-structure)

## Terminology

### Rule declaration

<details>

<summary>Rule declaration</summary>

A “_rule declaration_” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```scss
.co-button {
  ...
  position: relative;
  min-height: 42px;
  ...
}
```

### Selectors

In a rule declaration, “_selectors_" are the bits that determine which elements in the _DOM tree_ will be styled by the defined properties. Selectors can match HTML elements, as well as an element's _class_, _ID_, or any of its attributes. Here are some examples of selectors:

```scss
.co-button {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```scss
/* some selector */ {
  position: relative;
  display: block;
}
```
</details>

## CSS

<details>

<summary>CSS Formating</summary>

- Use soft tabs (2 spaces) for indentation.
- Use dashes over camelCasing in class names. Underscores are OK if you're using BEM (see [BEM](#bem) below).
- When using multiple selectors in a rule declaration, give each selector its own line.
- Put a space before the opening brace `{` in rule declarations.
- In properties, put a space after, but not before, the `:` character.
- Put closing braces `}` of rule declarations on a new line.
- Put blank lines between rule declarations and between logical groups of properties.
- Do not use ID selectors (ever *).

\* - special exception is made for Cobalt Reacts Portals

***Bad***

```scss
.co-button {
  position: relative;
  min-height: 42px; ... }

.no, .nope, .not_good {
    // ...
}

#lol-no {
  // ...
}
```

***Good***

```scss
.co-button {
  position: relative;
  min-height: 42px;
  ...
}

.one,
.selector,
.per-line {
  // ...
}
```


**Property Ordering Declaration**

***Good***

```scss
.co-button {
  @include type-setting(smll);
  @include font-weight(medium);
  box-sizing: border-box;
  position: relative;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  line-height: initial;
  vertical-align: middle;
  min-height: 42px;
  padding: 0 24px;
  border-radius: $border-radius;
  ...
}
```

***Acceptable***

```scss
.co-button {
  @include type-setting(small);
  @include font-weight(medium);

  box-sizing: border-box;
  position: relative;
  display: inline-flex;

  justify-content: center;
  align-items: center;
  line-height: initial;
  vertical-align: middle;

  min-height: 42px;
  padding: 0 24px;
  border-radius: $border-radius;
  ...
}
```

- Although, while not fully enforced, divide properties in logical groups within a selector block, in the following order:
  - Layout properties
  - Element geometry properties
  - Cosmetic properties
  - Behavioural properties
- Prefer line comments (`//` in Sass-land) to block comments.
- Prefer comments on their own line. Avoid end-of-line comments.
- Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks
</details>

<details>

<summary>Naming and BEM</summary>

All styles developed through Cobalts Design System should be prefixed with `co-`. This communicates that the class developed specifically in/from this project and used only in its scope.

```scss
.co-card { }

.co-card--featured { }

.co-card__title { }

.co-card__content { }

.co--small {}
```

We encourage the use of BEM notation for these reasons:

- It helps create clear, strict relationships between CSS and HTML
- It helps us create reusable, composable components
- It allows for less nesting and lower specificity
- It helps in building scalable stylesheets

***BEM***, or “_Block-Element-Modifier_”, is a _naming convention_ for classes in _HTML_ and _CSS_. It was originally developed by _Yandex_ with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing _OOCSS_.
<br/>
- CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
- Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)


***Example***

```html
<article class="co-card co-card--featured">

  <h1 class="co-card__title">GLorem ipsum dolor</h1>

  <div class="co-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```css
.co-card { }

.co-card--featured { }

.co-card__title { }

.co-card__content { }
```

- `.co-card` is the “block” and represents the higher-level component
- `.co-card__title` is an “element” and represents a descendant of `.co-card` that helps compose the block as a whole.
- `.co-card--featured` is a “modifier” and represents a different state or variation on the `.co-card` block.
- However in select use cases `.co-card.co--small` (even though it's an overqualification) is allowed - this is referrend within the theam as a global modiffier with a specific context;
</details>

<details>

<summary>ID Selectors</summary>

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.
</details>

<details>

<summary>Borders and Outlines</summary>

Since we do not know at first glance wether an certer border/outline or any other similar property is going to be transitioned it is deemed best practice to avoid _named_ empty values
Use `0` instead of `none` to specify that a style has no border:

***Bad***

```scss
.big-no-no {
  border: none;
}
```

***Good***

```scss
.good {
  border: 0;
}
```
</details>

## Sass

<details>

<summary>Sass Syntax</summary>

- Use the `.scss` syntax, never the original `.sass` syntax
- Order your regular CSS and `@include` declarations logically (see below)


### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .co-button {
      background: var(--primary-600);
      // ...
    }
    ```

2. `@include` declarations & variables

    Grouping `@include`s at the start makes it easier to read the entire selector as well as allow for easier overriding of a giver elements styles.

    ```scss
    .co-button {
      @include example-mixin-include();
      // ...
      background-color: var(--primary-600);
      transition: background $default-anim-duration $default-anim-timing;
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add empty line between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .co-button {
      //...

      &.co-button--modifier {
        margin-right: 10px;
      }
    }
    ```

### Variables

Use dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).


### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.


### Extend directive

`@extend` should be minimized because it has unintuitive behavior, especially when used with nested selectors. Instead resort to placeholders (using the notation provided below) if the `@extend` behavior is required to help DRY your code and have a single source of truth.


### Nested selectors

***Do not nest selectors more than two levels deep!**

```scss
.main-container {
  .content {
    // STOP!
  }
}
```

When selectors become this long, you're likely writing CSS that is:

- Strongly coupled to the HTML (fragile) *—OR—*
- Overly specific (powerful) *—OR—*
- Not reusable

Again: ***never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.
</details>

## Misc.

<details>

<summary>Overqualification & Colors</summary>

### Colors

**Prefer longhand uppercase version of HEX colors whenever possible as it improves searchability.**

***Bad***

```scss
.wrong {
  background-color: #FFFFF;
}

.not-it-either {
  background-color: rgba(255,0,0,1);
}
```

***Good***

```scss
.correct {
  background-color: #FFFFFF;
}

.ta-da {
  background-color: #FF0000;
}
```

***Best***

```scss
.co-button {
  background-color: var(--primary-600);
}
```


### Over-qualification

***Avoid selector over-qualification***

CSS over-qualification of selectors adds unnecessary complexity and makes it harder to maintain simple styles.
If global styling is required opt for a classless approach, unless it's you absolutely require a more specific selector. (i.e. creating a global reset styles file or set of rules) (*)

***Bad***

```scss
a.wrong-link {
  text-decoration: underline;
}
```

***Good***

```scss
.correct-link {
  text-decoration: underline;
}
// OR (*)
a {
  text-decoration: underline;
}
```
</details>

### Component Styles folder structure


<!-- @TODO: make clear colors, imports and forlder structure will be fundamentally different here!!! (review new REPO - ask Nuno Maia or Daniel Sil) -->

<details>

<summary>Folder Structure</summary>

**WARNING:** _Do note that the folder structure is going to change moving on to Cobalt Design's V2 - and this is only a guideline for the existing folder structure and folder/file creation rules._

Everything you need is inside the _source_ folder`

- `sourc/assets` contains all the images and icons that Cobalt needs
- `sourc/sass` contains the SCSS files that will be the source to generate the CSS bundle.
- `source/web` contains the files that will be the source to generate the static web site and the components examples and documentation.


Rules to follow when organizing Components (`scss folder`).

- When a component has dependencies of another components, it needs to be on its own folder, under its parent.
- The main style file should be the component's name. This file is imported by `main.scss`. This should import all dependencies files. _(check navbar example)_.
- If the component is compatible with global modifiers, its scss needs to be placed on a separate file named `global-modifiers.scss`.

<br/>

***See the structure example below:**

```bash
...

CHANGELOG.md
/source
|-- /sass
    |-- /component
        |-- _component.scss
        |-- _component-dependencyA.scss
        |-- _component-global-modifiers.scss(*)
...
```
<br/>
\* - (this needs to be the last to be imported.)
</details>

# Creating or migrating a component step by step

## Creating the main folder and scss file

The first step is to create the main folders of the new component. The folder should follow the structure described above and the main scss file should be named after the component. The new cobalt design already has a few comands that create the right component folders. (see link or somethign)

The Buttom will be used as an example throughout these explanations. See [Button Documentation](https://talkdesk.atlassian.net/wiki/spaces/COB/pages/1173192818/New+page+Buttons)

The first steo is to create a folder with `Button` and and a `button.scss` file inside. This file will contain all the default styles of the component, including styles related to the sub-components of button and imports of other files that are related to button, like for example, modifiers and mixins.

example:
```scss
@import 'button-mixins';

.co-button {
  //Default Styles
}

@import 'button-global-modifiers';
@import 'button-modifiers';
```

If the component has sub-components and hover states, they must follow the following example:

```scss
.co-component {
  //Default Styles
}

.co-component-subcomponent {
  //Default Styles

  &:hover {}
  &:active {}
}
```

## Creating the modifiers file

As described in the documentation, the button component has multiple variations of style. There variations are created by combining the component with modifier classes and props. Some of these classes are global and others are specific to the component.

All modifiers that are specific to the component should be in the same `button-modifiers.scss` file while the global modifiers should be in the `button-global-modifiers.scss` file.

Example of a modifiers file:
```scss
.co-button {
  .co-button--icon {
    // Css properties
  }

  .co-button--loader {
    // Css properties
  }
}
```

Example of a global modifiers file:
```scss
.co-button {
  .co--small {
    // Css properties
  }

  .co--large {
    // Css properties
  }
}
```

Components that are more complex may need multiple files with different groups of modifiers. These should be added to a new flolder with the name of the type of modifiers they are. For example for the button, since there are a lot of global modifiers, they should be grouped inside a folder with the name `global-modifiers`.

Bellow there is an example of the structure:

```
button
  |-- _button.scss
  |-- _button-modifiers.scss
  |-- /global-modifiers
      |-- _button-sizes.scss
      |-- _button-variations.scss
      |-- _button-loading.scss
      // etc...
```

## Mixings and  Functions.
Some components may need to repeat parts of the code and logic. For these cases, mixins and functions should be used to make things easier.

Add file for that with the component base folder or in a sub-folder if there are multiple files

The button component makes use of a size mixin for the loader size. This mixin is placed in a `button-mixin.scss` file, in the button css base folder.


## White Labeling

All components must be compatible with white labeling. To make this possible, some key css properties must use css variables. Each component has different needs so the documentation will provide the correct list of properties that will need to be configurable.
For the example of the button, the things that may change are the colors and the shape.

How to declare a variable:

```scss
--color-varible: #{palette-color('gray', 300)};
--padding-variable: 0 24px;

.co-button {
  background-color: var(--color-variable);
  padding: var(--padding-variable);

  .co--primary {
    --color-varible: #{palette-color('gray', 300)};
  }

  .co--large {
    --padding-variable: 0 32px;
  }
}
```

Like illustrated in the example above variables should be declared at the beginning of the block or file and follow this format: `--name-of-variable: value`. To be applied to a selector, it the var should be called like this: `var(--name-of-variable)`. It's also possible to pass a default value for the variable: `var(--name-of-variable, defaultValue)`

## Cobalt Helper Functions and Mixins
This section will talk breafely of the multiple helper functions we have in order to make it more clear when to use it (and inforce people to use them)

TODO:
Como aplicar uma cor ?
List of special functions we use like:
spacing(),
pallete(),
font-weight(),
mediaQueries()
