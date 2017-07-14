# USX CSS / SASS Style Guide

*The USX approach to CSS and SASS*

Other Style Guides

  - [USX JavaScript](https://github.com/cpapdotcom/usx-javascript-style-guide)

## Table of Contents

  1. [Terminology](#terminology)
  1. [File Structure](#file-structure)
  1. [Documentation](#documentation)
  1. [Rules](#rules)
  1. [Indentation](#indentation)
  1. [Formatting](#formatting)
  1. [Nesting](#nesting)
  1. [Selectors](#selectors)
  1. [Properties](#properties)
  1. [SASS Variables](#sass-variables)
  1. [SASS Components](#sass-components)
  1. [Reserved Classes](#reserved-classes)

## Terminology

  <a name="terminology--block"></a><a name="1.1"></a>
  - [1.1](#terminology--block) **Block:** A *Block* is the name given to a selector (or a group of selectors) with an accompanying group of properties.

      ```sass
      .listing {
          font-size: 1.1rem;
          line-height: 1.2;
      }
      ```

  <a name="terminology--selectors"></a><a name="1.2"></a>
  - [1.2](#terminology--selectors) **Selectors:** In a Block, *Selectors* are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes.

      ```sass
      .my-element-class {
          /* ... */
      }

      [aria-hidden] {
          /* ... */
      }
      ```

  <a name="terminology--properties"></a><a name="1.3"></a>
  - [1.3](#terminology--properties) **Properties:** Finally, *Properties* are what give the selected elements of a Block their style. Properties are key-value pairs, and a Block can contain one or more property declarations.

      ```sass
      /* some selector */ {
          background: #F1F1F1;
          color: $gray;
      }
      ```

**[⬆ back to top](#table-of-contents)**

## File Structure

  <a name="file-structure--file-extensions"></a><a name="2.1"></a>
  - [2.1](#file-structure--file-extensions) Use the .scss syntax, never the deprecated .sass syntax.

      **Bad**

      ```bash
      touch foo.sass
      ```

      **Good**

      ```bash
      touch foo.scss
      ```

  <a name="file-structure--code-location"></a><a name="2.2"></a>
  - [2.2](#file-structure--code-location) All SCSS/CSS should go into our `.scss` files found within the modules `Resources/assets/sass/*` directory. It should *never* be put into Vue single-file component.

      > Why? Even though component SCSS/CSS can be conveniently modularized, and scoped, using Vue single-file components it can be hard to track down within the codebase. It also doesn't get run through the *stylelint* linting on webpack build or PR *Bamboo* build. Having it within our SASS files keeps our SCSS centralized and is easier to find and debug.

  <a name="file-structure--directories"></a><a name="2.3"></a>
  - [2.3](#file-structure--directories) Below is a breakdown and description of a modules `Resources/assets/sass/` directory structure:

    * `Resources/assets/sass/`
        * The primary module import files. These files should contain no actual styling code, just order-specific `@imports` for building the modules CSS output.
    * `Resources/assets/sass/breakpoints`
        * This contains general rules for the primary global framework separated by the applications pre-determined breakpoints. This should remain lean. Most styles are broken down into more specific SASS files described below.
    * `Resources/assets/sass/components`
        * USX Vue Component styles by file. Example: `modal.scss` contains all the style code for the USX Vue Modal Component.
    * `Resources/assets/sass/globals`
        * This is the primary directory for global, reusable styles pertaining to specific functionality or types (but not partials or any page specific styles).
    * `Resources/assets/sass/modules`
        * Modules refer to SASS specific module code. These include:
          - [variables.scss](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#Variables_____variables_)
          - [mixins.scss](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#Mixin_Directives__mixins)
          - [functions.scss](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#Function_Directives__function_directives)
    * `Resources/assets/sass/pages`
        * Specific styles used by a single page represented by a single file. Example: `view-order.scss` would contain non-reusable styles that pertain solely to the order page.
    * `Resources/assets/sass/partials`
        * Contains code grouped by global entities or components. Example: `sidebar-navigation.scss` contains all the style code for the ModuleCow sidebar.
        * The `partials` directory is also a good place to abstract out reusable code that can be used by multiple pages or the framework in general. Example: Create a reusable `.employee-card` class (and nested blocks) that may be used on the employee profile page, but also used again within other pages, such as the employee "About Us" admin page. If you ever build a styled class for a page and think it may have benefits as a reusable entity, you should abstract it out into the `partials` directory.
    * `Resources/assets/sass/vendor-overrides`
        * This is used to override, *and extend*, classes and styles that are vendor specific. Meaning that the vendor files themselves are loaded through Yarn and the primary vendor SASS/CSS file resides within the `node_modules` directory.

**[⬆ back to top](#table-of-contents)**

## Documentation

  <a name="documentation--file"></a><a name="3.1"></a>
  - [3.1](#documentation--file) **File Documentation:** All new SCSS files should contain a DocBlock style multi-line comment providing a general explanation of the category or grouping the styles in that file represent.

      ```sass
      /**
       * ModuleCow Bulma Extensions and Overrides
       *
       * These are exact class specific overrides to Bulma classes. Use this modular file
       * when overriding or extending a rule or rule set on an already existing Bulma class.
       */

      /* initial block */ {
          ...
      }
      ```

  <a name="documentation--two-slash"></a><a name="3.2"></a>
  - [3.2](#documentation--two-slash) Prefer `//` inline comments to standard single line CSS block comments (SCSS files only).

      ```sass
      /* bad */
      .foo {
          ...
      }

      // good
      .foo {
          ...
      }
      ```

  <a name="documentation--singleline"></a><a name="3.3"></a>
  - [3.3](#documentation--singleline) Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

      ```sass
      .foo { // bad
          ...
      }

      .foo {

          // bad
          .bar {
              ...
          }
      }

      // good
      .foo {
          ...
      }

      .foo {
          // good
          .bar {
              ...
          }
      }
      ```

**[⬆ back to top](#table-of-contents)**

## Rules

  <a name="rules--linting"></a><a name="4.1"></a>
  - [4.1](#rules--linting) Our primary style and structure is dictated by our configured [styelint](https://stylelint.io) rule set.

      **Please see the [.stylelintrc](http://bitbucket.usx.core/users/scott.carlson/repos/usx-modulecow-starter-template/browse/.stylelintrc?at=proposedStyleLintConfiguration) file for all enforced CSS/SASS style rules.**

      Detailed rule explanations can be found [here](https://stylelint.io/user-guide/rules).

  <a name="rules--border"></a><a name="4.2"></a>
  - [4.2](#rules--border) Use `0` instead of `none` to specify that a style has no border..

      **Bad**

      ```sass
      .foo {
          border: none;
      }
      ```

      **Good**

      ```sass
      .foo {
          border: 0;
      }
      ```

**[⬆ back to top](#table-of-contents)**

## Indentation

  <a name="indentation"></a><a name="5.1"></a>
  - [5.1](#indentation) Use soft tabs (space character) set to 4 spaces.

      **Bad**

      ```sass
      .foo {
      ∙font-size: 1.2rem;
      }

      .foo {
      ∙∙font-size: 1.2rem;
      }
      ```

      **Good**

      ```sass
      .foo {
      ∙∙∙∙font-size: 1.2rem;
      }
      ```

**[⬆ back to top](#table-of-contents)**

## Formatting

  <a name="formatting"></a><a name="6.1"></a>
  - [6.1](#formatting) Below are our general formatting guidelines. For a complete overview of enforced CSSS/SCSS formatting see our [.stylelintrc](http://bitbucket.usx.core/users/scott.carlson/repos/usx-modulecow-starter-template/browse/.stylelintrc?at=proposedStyleLintConfiguration) configuration file.

    * When using multiple selectors in a rule block, give each selector its own line.

        ```sass
        /* bad */
        .foo {
            background: $white; color: $black;
        }

        /* good */
        .foo {
            background: $white;
            color: $black;
        }
        ```

    * Put a space before the opening brace `{` in rule blocks

        ```sass
        /* bad */
        .foo{
            background: $white;
        }

        /* good */
        .foo {
            background: $white;
        }
        ```

    * In properties, put a space after, but not before, the `:` character.

        ```sass
        /* bad */
        .foo {
            background:$white;
        }

        /* good */
        .foo {
            background: $white;
        }
        ```

    * Put closing braces `}` of rule blocks on a new line

        ```sass
        /* bad */
        .foo {
            background: $white;}

        /* good */
        .foo {
            background: $white;
        }
        ```

    * Put blank lines between rule blocks

        ```sass
        /* bad */
        .foo {
            background: $white;
        }
        .bar {
            color: $black;
        }

        .foo {
            background: $white;
            .bar {
                color: $black;
            }
        }

        /* good */
        .foo {
            background: $white;
        }

        .bar {
            color: $black;
        }

        .foo {
            background: $white;

            .bar {
                color: $black;
            }
        }
        ```

**[⬆ back to top](#table-of-contents)**

## Nesting

  <a name="nesting--required"></a><a name="7.1"></a>
  - [7.1](#nesting--required) Nesting blocks is required, even if no properties exist on the selector itself.

      > Why? Updating and extending selectors is *much* easier when the nesting structure is already in place.

      **Bad**

      ```sass
      .foo {
          font-size: 1.2rem;
      }

      .foo .bar .faz {
          color: $grey;
      }
      ```

      **Good**

      ```sass
      .foo {
          font-size: 1.2rem;

          .bar {
              .faz {
                  color: $grey;
              }
          }
      }
      ```

  <a name="nesting--depth"></a><a name="7.2"></a>
  - [7.2](#nesting--depth) Attempt to keep primary selector nesting to no more than 4 levels deep, not including pseudo-elements or pseudo-classes. We won't include enforced linting for this rule but it is best practice.

      > Why? When selectors become this long, you're likely writing CSS that is:
      > * Strongly coupled to the HTML (fragile) —OR—
      > * Overly specific (powerful) —OR—
      > * Not reusable

      **Bad**

      ```sass
      .page-container {
          .profile {
              .employee {
                  .employee-history {
                      .icon {
                          /* STOP! */
                      }
                  }
              }
          }
      }
      ```

      **Good**

      ```sass
      /* create reusable employee card styling */
      .employee-card {
          .history {
              .icon {
                  ...
              }
          }
      }
      ```

**[⬆ back to top](#table-of-contents)**

## Selectors

  <a name="selectors--naming"></a><a name="8.1"></a>
  - [8.1](#selectors--naming) Always use slug naming (dashed compounds) for class and ID selector names.

      **Bad**

      ```sass
      .fooBar {
          ...
      }

      .foo_bar {
          ...
      }
      ```

      **Good**

      ```sass
      .foo-bar {
          ...
      }
      ```

  <a name="selectors--id"></a><a name="8.2"></a>
  - [8.2](#selectors--id) Prefer classes over IDs. IDs should rarely be used.

      > Why? While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.
      > For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

      **Bad**

      ```sass
      #foo {
          ...
      }
      ```

      **Good**

      ```sass
      .foo {
          ...
      }
      ```

  <a name="selectors--javascript"></a><a name="8.3"></a>
  - [8.3](#selectors--javascript) **JavaScript Hook Classes:** Avoid binding to the same class in both your CSS and JavaScript. Due to the fact that we utilize the Vue framework throughout our application, a specific JavaScript hook class should rarely, if ever, be required (currently none exist at the time of this writing).

      However, if it is required that you use JavaScript to "reach" into the DOM to manipulate a specific element you should use a unique, JavaScript specific, class prefixed with `.js-`.

      > Why? Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

      Create a JavaScript-specific class to bind to, prefixed with `.js-`:

      ```html
      <button class="btn btn-primary js-request-prescription">Request Prescription</button>
      ```

**[⬆ back to top](#table-of-contents)**

## Properties

  <a name="properties--ordering"></a><a name="9.1"></a>
  - [9.1](#properties--ordering) Properties should be ordered alphabetically. We have a Gulp command to implement this automatically. It should be run before submitting a PR request.

      > Why? It adds consistency and makes it easier to locate properties within a block.

      **Bad**

      ```sass
      .foo {
          z-index: $base-z-index + 5;
          font-family: Arial, sans-serif;
          background: $white;
          width: 100vw;
          font-size: 1.1rem;
          display: block;
          font-weight: bold;
      }
      ```

      **Good**

      ```sass
      .foo {
          background: $white;
          display: block;
          font-family: Arial, sans-serif;
          font-size: 1.1rem;
          font-weight: bold;
          width: 100vw;
          z-index: $base-z-index + 5;
      }
      ```

**[⬆ back to top](#table-of-contents)**

## SASS Variables

  <a name="sass-variables--naming"></a><a name="10.1"></a>
  - [10.1](#sass-variables--naming) Always use slug naming (dashed compounds) for SASS variables.

      **Bad**

      ```sass
      $fooBar: #FFF;

      $foo_bar: #FFF;
      ```

      **Good**

      ```sass
      $foo-bar: #FFF
      ```

  <a name="sass-variables--z-index"></a><a name="10.2"></a>
  - [10.2](#sass-variables--z-index) `z-index` properties should *always* extend from one of the base `z-index` [SASS variables](http://bitbucket.usx.core/users/scott.carlson/repos/usx-modulecow-starter-template/browse/modules/Framework/Resources/assets/sass/modules/variables.scss).

      **Bad**

      ```sass
      .foo {
          z-index: 999;
      }
      ```

      **Good**

      ```sass
      .foo {
          z-index: $base-z-index + 5;
      }
      ```

  <a name="sass-variables--colors"></a><a name="10.3"></a>
  - [10.3](#sass-variables--colors) SASS colors should *always* be variables. Most framework colors should already be defined and you should attempt to use the preset colors defined within the SASS variables to keep the UI consistent. If you must utilize a new color that isn't already defined then you should add it to the `variables.scss` file with a semantic name.

      > Why? This allows for consistent, and reusable, color palettes for the UI. It also allows us to change color options site wide with a single variable.

      **Bad**

      ```sass
      .foo {
          color: #F1F1F1;
          background: #333;
      }
      ```

      **Good**

      ```sass
      .foo {
          color: $off-white;
          background: $gray;
      }
      ```

**[⬆ back to top](#table-of-contents)**

## SASS Components

  <a name="sass-components--mixins"></a><a name="11.1"></a>
  - [11.1](#sass-components--mixins) **Mixins:** [Mixins](http://sass-lang.com/guide#topic-6) should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions.

  <a name="sass-components--extend"></a><a name="11.2"></a>
  - [11.2](#sass-components--extend) **Extend Directive:** `@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

**[⬆ back to top](#table-of-contents)**

## Reserved Classes

  <a name="reserved-classes"></a><a name="12.1"></a>
  - [12.1](#reserved-classes) The following are reserved classes used by the USX framework, vendor libraries, or components.

      * `.dropzone`
      * `.animated`

**[⬆ back to top](#table-of-contents)**
