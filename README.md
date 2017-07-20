# Frontend Methodology

This repo is the documentation of the way I plan and code frontend architectures. Its main focus is on design systems, but it will be extended in the short term to include more guidelines regarding logic.

---

## Contents

1. [General description](#general-description)
2. [Naming convention](#naming-convention)
3. [Comment convention](#comment-convention)
4. [File organization](#file-organization)
5. [Code organization](#code-organization)


---

## General description

My personal methodology is inspired in object-oriented methodologies and naming conventions applied to CSS, with RSCSS and Atomic Design being the main bluprints.

---

## Naming convention

Loosely based on [RSCSS](https://github.com/rstacruz/rscss) guidelines. If a doubt can't be solved with what's here, then [refer to RSCSS](http://rscss.io/) itself because it also contains a lot of tips to overcome common problems.

### Independent components 

Use at least two words separated by a single dash.

```scss
.card-box { ... }
```

### Dependent components

Only one word to label the HTML element, which is targeted using the ```>``` child selector.

If more than one word is needed, then the words are concatenated without separators.

```scss
.card-news {
    > .title { ... }
    > .importantcontent { ... }
}
```

### Modifiers

Special skins with a purely cosmetic change are applied adding an extra class to the HTML element.

This class is a single word preceded by two dashes.

```scss
.btn {
    &.--big { ... }
}
```

### Specifications

Specifications are modifiers that have a semantic meaning. They follow the BEM convention for modifiers, and the styles of the superclass are inherited using an @extend directive (to avoid having to write the parent class in the HTML selector).

```scss
.btn {
    // sass concatentes this in the root level
    &--danger { 
        @extend .btn;
        ... 
    }
}
```

### Utilities

Versatile styles that are useful to have in a single class. Harry Robert's objects also fall into this category.

The key to decide whether a block of styles should be placed in a utility class, is to ponder how sure you're that the styles don't change regardless of the contex.

They're labeled with a single word and are placed in the last position in the class attribute of the HTML element it affects.

```scss
.wrap { ... }
```
```html
<div class="main-content wrap">
    ...
</div>
```

### Hacks

Special cases when we need to override some styles in a hurry. Bad utilities also go in this category.

Hacks are designated with the namespace ```_``` and must always be refactored in the near future.

```scss
._pull-left { ... }
```

### Animations

Animations use the namespace ```a-``` and their unique identifier is composed of two words separated by a single dash, where the first word describes the elements that use the animation and the second describe the animation itself.

If several words are needed, they are concatenated without separators.

```scss
@keyframes a-btn-wiggle { ... }
```

### Javascript hooks

Triggers and other JS hooks are always labeled with the namespace (```js-```) and the identifiers follow a camelcase style. 

The hooks must be applied regardless of the presence of other selectors. The idea is to keep styles and logic separated.

```html
<div class="menu-nav">
    ...
    <div class="trigger js-menuTrigger"></div>
</div>
```

### Sass variables

- Dash-cased
  ```scss
  $useful-variable: 'value';
  ```
- Abstracted: their name reflect the function of the data, not the content
  ```scss
  $c-main: blue; // instead of $c-blue
  ```
- Namespaced if possible: these are the useful name spaces:
    - ```c-``` for colors
    - ```g-``` for gradients
    - ```f-``` for font stacks


### Sass functions

Their names are also dash-cased and the attributes follow the guidelines for regular variables.

---

## Comment convention

### General guidelines
- Document whenever something isn't obvious --realy, whenever!-- but nothing more (i.e.: don't waste time commenting stuff you're **completely** sure about remembering and don't need to share with other developers) 
- Specifications shoudn't document their parent class, because you can tell
 from their name
- When working with a team, document more exhaustively than how you normally document
- Only use Sass comments for the sake of readabity

### Specific cases

#### Superclasses (selectors that have specifications)
:warning: Automate the specs generation

```scss
// .superclass
// [Description]
// @specs                    // specifications
//     .superclass--spec1
//     .superclass--spec2
//     ...
.superclass { ... }
```

#### Animation keyframes 
:warning: Automate the user generation

```scss
// a-animation-name
// [Description]
// @users
//     .animationuser1
//     .animationuser2
//     ...
@keyframes a-animation-name { ... }
```

#### Mixins
:warning: Automate the user generation

```scss
// mixin
// [Description]
// @arguments
//     $argument1 // what it is
//     $argument2 // what it is
//     ...
// @users                    
//     .user1
//     .user2
//     ...
@mixin mixin($arguments) { ... }
```

#### Utiliies
:warning: Automate the user generation

```scss
// .utility
// [Description]
// @users                    // blocks that contain elems with this utility
//     .user1
//     .user2
//     ...
.utility { ... }
```
#### Many random long comments in a single block

```scss
// .selector 
// [@notes]                  // if .selector has other comments
// [1] Explan what you need about the section where you placed the marker for 
//     this note
// [2] Explan what you need about the section where you placed the marker for 
//     this note
// [3] Explan what you need about the section where you placed the marker for 
//     this note
// ...
.selector {
    color: value; // [1]
    background: value; // [2]
    font-size: value; // [3]
    ...
}
```

---

## File organization

```
tools/
    _variables.scss    # use guarded assignation
    _settings.scss     # redefine variables if needed 
    _functions.scss
    _mixins.scss
base/
    _resetter.scss     # clean browser base and put mine
    _typography.scss   # define a robust modular vertical rythm 
                       # (make use of mixns and functions)
utils/
    _layout.scss       # grid, wrap, etc.
    _forms.scss        # in, textarea, etc.
    _multimedia.scss   # img, figcaption, svg, etc.
    _keyframes.scss    # only those that apply to several components
atoms/
    _buttons.scss
molecules/
organisms/
    _header.scss
    _nav.scss
    _footer.scss
templates/             # use @document
styles.scss            # works as a table of contents too
```

### General criteria to choose where to put partials

- You can also include bare tags outside ```_setter.scss``` as long as they're within the ```base/``` or the ```utils/``` folder
- Think about the semantic meaning of the selector (according to its function within the UI system)


---

## Code organization

### Order of blocks

Use the critetia of ITCSS:
- From low specificity to high specificity
- From broad scope to narrow scope
- From generic to explicit

### Order inside blocks

1. Scoped variables
2. ```@extend``` directives
3. ```@mixin```s
4. Properties
5. Pseudo-classess
6. Pseudo-elements
7. Nested selectors (if any)
8. Keyframes right after the block declaration (if the animation only applies to a specific component)

### Other style guidelines

- Follow the *Inception Rule* (never go deeper than 3 levels)
- Maximum nesting: 50 lines
- Never write vendor prefixes (add them with Autoprefixer)

---

## Inspiration

- [Atomic Design](http://atomicdesign.bradfrost.com/chapter-2/)
- [RSCSS](https://github.com/rstacruz/rscss)
- [CSS Tricks: Sass Style Guide](https://css-tricks.com/sass-style-guide)
- [SitePoint: Architecture of a Sass Project](https://www.sitepoint.com/architecture-sass-project/)
- [CSS Guidelines](https://cssguidelin.es/#table-of-context)
- [Fron-tend Architecture](https://github.com/micahgodbolt/front-end-architecture)
- [Idiomatic Sass](https://github.com/anthonyshort/idiomatic-sass)
- [Idiomatic CSS](https://github.com/necolas/idiomatic-css)