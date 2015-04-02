# LU CSS/SASS Styleguide

## Table of contents

- [Kiki](#kiki)
- [Syntax & Formatting](#syntax--formatting)
- [Selectors and Names](#selectors-and-names)
- [Rule Set Innards](#rule-set-innards)
- [Colors](#colors)
- [Numbers](#numbers)
- [Sassy Things](#sassy-things)
- [Comments](#comments)
- [Project structure](#project-structure)
- [SCSS-Lint](#scss-lint)

## Kiki

Kiki is an internal library for sass. It cointains useful mixins, helpers, grid system and media queries manager. It's recommended to use it in every project.

https://jira:1337/svn/PLAWEB/22_src/tools/CSS/kiki/trunk/

## Syntax & Formatting
##### Indentation
* 2 spaces, no tabs.

##### Final newline
*   Files should always have a final newline. This results in better diffs when adding lines to the file, since SCM systems such as git won't think that you touched the last line.

##### Empty line between blocks
*   Separate rule, function, and mixin declarations with empty lines.

```scss
/* avoid */
p {
  margin: 0;
  em {
    ...
  }
}
a {
  ...
}
```
```scss
/* recommended */
p {
  margin: 0;

  em {
    ...
  }
}

a {
  ...
}
```
##### Spaces after commas and colons
* Properties should be formatted with a single space separating the colon from the property's value.

```scss
/* avoid */
margin:0; //no space after colon
margin:  0; //more than one space after colon
```
```scss
/* recommended */
margin: 0;
```
* Properties should be formatted with no space between the name and the colon.

```scss
/* avoid */
margin : 0; //space before colon
```
```css
/* recommended */
margin: 0;
```
* Opening braces should be preceded by a single space.

```scss
/* avoid */
p{ 
  ... //no space before brace
}

p  { 
  ... //more than one space before brace
}
```
```scss
/* recommended */
p {
  ...
}
```

##### SpaceBetweenParens
* Parentheses should not be padded with spaces.

```scss
/* avoid */
@include box-shadow( 0 2px 2px rgba( 0, 0, 0, .2 ) );
color: rgba( 0, 0, 0, .1 );
```
```scss
/* recommended */
@include box-shadow(0 2px 2px rgba(0, 0, 0, .2));
color: rgba(0, 0, 0, .1);
```

##### Use single quotation marks
* String literals should be written with single quotes unless using double quotes would save on escape characters.

##### End every declaration with a semicolon
* Property values; @extend, @include, and @import directives; and variable declarations should always end with a semicolon.

```scss
/* avoid */
p {
  color: #fff
}
```
```scss
/* recommended */
p {
  color: #fff;
}
```

## Selectors and Names

##### Id selector
* Avoid using ID selectors.

##### Descendant selector
* Avoid the descendant selector. It is the most expensive selector in CSS. It is dreadfully expensive—especially if the selector is in the Tag or Universal Category.

```scss
/* avoid */
treehead treerow treecell { ... } //bad
treehead > treerow > treecell { ... } //BETTER, BUT STILL BAD
```
```scss
/* recommended */
.treecell-header { ... }
```

##### Important rule
* Avoid using `!important` in properties. It is usually indicative of a misunderstanding of CSS
[specificity] and can lead to brittle code.

```scss
/* avoid */
p {
  color: #f00 !important;
}
```
```scss
/* recommended */
p {
  color: #f00;
}
```

##### Single declarations
* Declare single declarations on one line

```scss
/* avoid */
.span {
  width: 60px;
}
```
```scss
/* recommended */
.span { color: #f00; }
``` 


##### Overqualifying selectors
* Avoid overqualifying selectors

```scss
/* avoid */
html body .wrapper #content a {}
```
```scss
/* recommended */
#content a {}
```

##### Do not qualify type selectors
* Avoid qualifying elements in selectors (also known as "tag-qualifying").

```scss
/* avoid */
div#thing {
  ...
}

ul.list {
  ...
}

ul li.item {
  ...
}

a[href="place"] {
  ...
}
```
```scss
/* recommended */
#thing {
  ...
}

.list {
  ...
}

ul .item {
  ...
}

[href="place"] {
  ...
}
```
##### Provide lowercase, hyphen-delimited names for classes, Sass variables and mixins
* Functions, mixins, and variables should be declared with all lowercase letters and hyphens instead of underscores.

```scss
/* avoid */
$myVar: 10px;

.headerButton {}

@mixin myMixin() {}
```
```scss
/* recommended */
$my-var: 10px;

.header-button {}

@mixin my-mixin() {}
```
##### The Inception Rule: don’t go more than three levels deep
* Avoid nesting selectors too deeply. In media queries you can nest 4 levels.

```scss
// best
.horse {
  font-size: 10em;

  // handy
  &:hover {
    font-size: 20em;

    // eyebrow-raising
    & > .donkey {
    color: #eee;

      // disaster-inviting(-inducing?)
      & + .mule {
        margin: 100em;

        // disaster-in-progress
        ul li a span {
          color: $death;
        }
      }
    }
  }
```
##### Single line per selector
* Split selectors onto separate lines after each comma.

```scss
/* avoid */
.error p, p.explanation {
  ...
}
```
```scss
/* recommended */
.error p,
p.explanation {
  ...
}
```

##### Meaningful or generic class names
* Instead of presentational or cryptic names, always use class names that reflect the purpose of the element in question, or that are otherwise generic.

```scss
/* avoid */
.yee-1901 {} //meaningless 
.red {} //presentational 
```
```scss
/* recommended */
.video {} //specific 
.important {} //generic 
```

##### Keep classes as short and succinct as possible
* Use ID and class names that are as short as possible but as long as necessary.

```scss
/* avoid */
.navigation {}
.atr {}
```
```scss
/* recommended */
.nav {}
.author {}
```

## Rule Set Innards
##### UrlQuotes
* URLs should always be enclosed within quotes.

```scss
/* avoid */
background: url(example.png);
```
```scss
/* recommended */
background: url('example.png');
```
##### Shorthand properties
* Prefer the shortest shorthand form possible for properties that support it.

```scss
/* avoid */
margin: 1px 1px 1px 1px;
```
```scss
/* recommended */
margin: 1px;
```
##### Zero unit
* Omit length units on zero values.

```scss
/* avoid */
margin: 0px;
```
```scss
/* recommended */
margin: 0;
```
##### Border zero
* Prefer the terser `border: 0` over `border: none`.


##### Vendor prefixes
*Avoid vendor prefixes. Instead, you can use Autoprefixer.

```scss
/* avoid */
@-webkit-keyframes anim {
  0% { opacity: 0; }
}

::-moz-placeholder {
  color: red;
}

.foo {
  -webkit-transition: none;
}

.bar {
  position: -moz-sticky;
}
```
```scss
/* recommended */
@keyframes anim {
  0% { opacity: 0; }
}

::placeholder {
  color: red;
}

.foo {
  transition: none;
}

.bar {
  position: sticky;
}
``` 
##### Declaration order
* Rule sets should be ordered as follows: `@extend` declarations, `@include` declarations without inner `@content`, properties, `@include` declarations *with* inner `@content`, then nested rule sets.

```scss
/* avoid */
.fatal-error {
  p {
    ...
  }

  color: #f00;
  @extend %error;
  @include message-box();
}
```
```scss
/* recommended */
.fatal-error {
  @extend %error;
  @include message-box();
  color: #f00;

  p {
    ...
  }
}
```

##### Property sort order
* Sort properties in a strict order. More info - http://smacss.com/book/formatting


**Box**<br>
display<br>
position<br>
top<br>
right<br>
bottom<br>
left<br>
<br>
width<br>
min-width<br>
max-width<br>
<br>
height<br>
min-height<br>
max-height<br>
<br>
margin<br>
margin-top<br>
margin-right<br>
margin-bottom<br>
margin-left<br>
<br>
padding<br>
padding-top<br>
padding-right<br>
padding-bottom<br>
padding-left<br>
<br>
float<br>
clear<br>
<br>
columns<br>
column-gap<br>
column-fill<br>
column-rule<br>
column-span<br>
column-count<br>
column-width<br>
<br>
transform<br>
transition<br>
<br>
**Border**<br>
border<br>
border-top<br>
border-right<br>
border-bottom<br>
border-left<br>
border-width<br>
border-top-width<br>
border-right-width<br>
border-bottom-width<br>
border-left-width<br>
<br>
border-style<br>
border-top-style<br>
border-right-style<br>
border-bottom-style<br>
border-left-style<br>
<br>
border-radius<br>
border-top-left-radius<br>
border-top-right-radius<br>
border-bottom-left-radius<br>
border-bottom-right-radius<br>
<br>
border-color<br>
border-top-color<br>
border-right-color<br>
border-bottom-color<br>
border-left-color<br>
<br>
outline<br>
outline-color<br>
outline-offset<br>
outline-style<br>
outline-width<br>
<br>
**Background**<br>
background<br>
background-color<br>
background-image<br>
background-repeat<br>
background-position<br>
background-size<br>
cursor<br>
<br>
**Text**<br>
color<br>
<br>
font<br>
font-family<br>
font-size<br>
font-smoothing<br>
font-style<br>
font-variant<br>
font-weight<br>
<br>
letter-spacing<br>
line-height<br>
list-style<br>
<br>
text-align<br>
text-decoration<br>
text-indent<br>
text-overflow<br>
text-rendering<br>
text-shadow<br>
text-transform<br>
text-wrap<br>
<br>
white-space<br>
word-spacing<br>
<br>
**Other**<br>
border-collapse<br>
border-spacing<br>
box-shadow<br>
caption-side<br>
content<br>
cursor<br>
empty-cells<br>
opacity<br>
overflow<br>
quotes<br>
speak<br>
table-layout<br>
vertical-align<br>
visibility<br>
z-index<br>

## Colors
##### ColorKeyword
* Prefer hexadecimal color codes over color keywords.

```scss
/* avoid */
color: green;
```
```scss
/* recommended */
color: #0f0;
```
##### Hex length
* Use the shortest possible hex value. `#fff` instead of `#ffffff`.

##### Hex notation
* Use lowercase letters inside hex values. `#fff` instead of `#FFF`. 

##Numbers
##### TrailingZero
* Don't write trailing zeros for numeric values with a decimal point.

```scss
/* avoid */
margin: .500em;
```
```scss
/* recommended */
margin: .5em;
```
##### Unnecessary mantissa
* Numeric values should not contain unnecessary fractional portions.

```scss
/* avoid */
margin: 1.0em;
```
```scss
/* recommended */
margin: 1em;
```

##### Leading zero
* Don't write leading zeros for numeric values with a decimal point.

```scss
/* avoid */
margin: 0.5em;
```
```scss
/* recommended */
margin: .5em;
```

## Sassy Things
##### Placeholder in extend
* Always use placeholder selectors in `@extend`.

```scss
/* avoid */
.fatal {
  @extend .error;
}
```
```scss
/* recommended */
.fatal {
  @extend %error;
}
```
##### Bang format
* Include one space before any ! and no spaces after it, with !important and !default declarations.

```scss
/* avoid */
color: #000!important;
```
```scss
/* recommended */
color: #000 !important;
```

##### Import path
* The basenames of `@import`ed SCSS partials should not begin with an underscore and should not include the filename extension.

```scss
/* avoid */
@import "foo/_bar.scss";
@import "_bar.scss";
@import "_bar";
@import "bar.scss";
```
```scss
/* recommended */
@import "foo/bar";
@import "bar";
```

##### Mixins without arguments
* Mixins without arguments should be written without Parentheses

```scss
/* avoid */
@mixin clearfix() {}
@include clearfix();
```
```scss
/* recommended */
@mixin clearfix {}
@include clearfix;
```

## Comments
##### Title section
 * Begin every new major section of a CSS project with a title. If you are working on a project where each section is its own file, this title should appear at the top of each one.
 
```scss
/*------------------------------------*\
    #SECTION-TITLE
\*------------------------------------*/

.selector {}
```

##### Comment style
* Use C-style comments

```scss
/**
 * Helper class to truncate and add ellipsis to a string too long for it to fit
 * on a single line.
 * 1. Prevent content from wrapping, forcing it on a single line.
 * 2. Add ellipsis at the end of the line.
 */
.ellipsis {
  white-space: nowrap; /* 1 */
  text-overflow: ellipsis; /* 2 */
  overflow: hidden;
}
```

## Project structure
* Use The 7-1 Pattern - 7 folders, 1 file. Folders structure:

base/<br>
components/<br>
layout/<br>
pages/<br>
themes/<br>
utils/<br>
vendors/<br>
main.scss<br>

##### Base folder
The base/ folder holds boilerplate code for the project. In there, you might find the reset file, some typographic rules, and probably a stylesheet (that I'm used to calling _base.scss), defining some standard styles for commonly used HTML elements.

* _base.scss
* _reset.scss
* _typography.scss

##### Layout folder
The layout/ folder contains everything that takes part in laying out the site or application. This folder could have stylesheets for the main parts of the site (header, footer, navigation, sidebar...), the grid system or even CSS styles for all the forms.

* _grid.scss
* _header.scss
* _footer.scss
* _sidebar.scss
* _forms.scss
* _navigation.scss

##### Components folder
For smaller components, there is the components/ folder. While layout/ is macro (defining the global wireframe), components/ is more focused on widgets. It contains all kind of specific modules like a slider, a loader, a widget, and basically anything along those lines. There are usually a lot of files in components/ since the whole site/application should be mostly composed of tiny modules.

* _media.scss
* _carousel.scss
* _thumbnails.scss


##### Pages folder
If you have page-specific styles, it is better to put them in a pages/ folder, in a file named after the page. For instance, it’s not uncommon to have very specific styles for the home page hence the need for a _home.scss file in pages/.

* _home.scss
* _contact.scss

##### Themes folder
On large sites and applications, it is not unusual to have different themes. There are certainly different ways of dealing with themes but I personally like having them all in a themes/ folder. Use only if application need different themes.

##### Utils folder
The utils/ folder gathers all Sass tools and helpers used across the project. Every global variable, function, mixin and placeholder should be put in here.

The rule of thumb for this folder is that it should not output a single line of CSS when compiled on its own. These are nothing but Sass helpers.

* _variables.scss
* _mixins.scss
* _functions.scss
* _helpers.scss

##### Vendor folder
And last but not least, most projects will have a vendors/ folder containing all the CSS files from external libraries and frameworks – Normalize, Bootstrap, jQueryUI, FancyCarouselSliderjQueryPowered, and so on. Putting those aside in the same folder is a good way to say “Hey, this is not from me, not my code, not my responsibility”.

* _normalize.scss
* _bootstrap.scss
* _jquery-ui.scss
* _select2.scss

If you have to override a section of any vendor create folder `vendors-extensions/`

##### Main file
The main file (usually labelled main.scss) should be the only Sass file from the whole code base not to begin with an underscore. This file should not contain anything but @import and comments.

Files should be imported according to the folder they live in, one after the other in the following order:
 1. vendors/ 
 2. utils/ 
 3. base/ 
 4. layout/ 
 5. components/ 
 6. pages/ 
 7. themes/

##### Shame file
All the CSS declarations, hacks, that need to be resolved later should be in shame file.
File named _shame.scss should be imported after any other file, at the very end of the stylesheet.
More info - http://csswizardry.com/2013/04/shame-css/

## SCSS-Lint
* Use SCSS-Lint to linter your code. https://github.com/causes/scss-lint. Recommended setup:

```xml
scss_files: "**/*.scss"

linters:
  BangFormat:
    enabled: true
    space_before_bang: true
    space_after_bang: false

  BorderZero:
    enabled: true
    convention: zero

  ColorKeyword:
    enabled: true

  ColorVariable:
    enabled: false

  Comment:
    enabled: false

  DebugStatement:
    enabled: true

  DeclarationOrder:
    enabled: true

  DuplicateProperty:
    enabled: true

  ElsePlacement:
    enabled: true
    style: same_line

  EmptyLineBetweenBlocks:
    enabled: true
    ignore_single_line_blocks: true

  EmptyRule:
    enabled: true

  FinalNewline:
    enabled: true
    present: true

  HexLength:
    enabled: true
    style: short

  HexNotation:
    enabled: true
    style: lowercase

  HexValidation:
    enabled: true

  IdSelector:
    enabled: true

  ImportantRule:
    enabled: true

  ImportPath:
    enabled: true
    leading_underscore: false
    filename_extension: false

  Indentation:
    enabled: true
    allow_non_nested_indentation: false
    character: space
    width: 2

  LeadingZero:
    enabled: true
    style: exclude_zero

  MergeableSelector:
    enabled: false
    force_nesting: true

  NameFormat:
    enabled: false
    allow_leading_underscore: true
    convention: hyphenated_lowercase 

  NestingDepth:
    enabled: true
    max_depth: 3

  PlaceholderInExtend:
    enabled: true

  PropertyCount:
    enabled: false
    include_nested: false
    max_properties: 10

  PropertySortOrder:
    enabled: true
    ignore_unspecified: false
    separate_groups: false
    order: smacss

  PropertySpelling:
    enabled: true
    extra_properties: []

  QualifyingElement:
    enabled: true
    allow_element_with_attribute: false
    allow_element_with_class: false
    allow_element_with_id: false

  SelectorDepth:
    enabled: true
    max_depth: 3

  SelectorFormat:
    enabled: true
    convention: hyphenated_lowercase

  Shorthand:
    enabled: true

  SingleLinePerProperty:
    enabled: true
    allow_single_line_rule_sets: true

  SingleLinePerSelector:
    enabled: true

  SpaceAfterComma:
    enabled: true

  SpaceAfterPropertyColon:
    enabled: true
    style: one_space

  SpaceAfterPropertyName:
    enabled: true

  SpaceBeforeBrace:
    enabled: true
    style: space
    allow_single_line_padding: false

  SpaceBetweenParens:
    enabled: true
    spaces: 0

  StringQuotes:
    enabled: true
    style: single_quotes

  TrailingSemicolon:
    enabled: true

  TrailingZero:
    enabled: false

  UnnecessaryMantissa:
    enabled: true

  UnnecessaryParentReference:
    enabled: true

  UrlFormat:
    enabled: true

  UrlQuotes:
    enabled: true

  VariableForProperty:
    enabled: false
    properties: []

  VendorPrefixes:
    enabled: true
    identifier_list: base
    include: []
    exclude: []

  ZeroUnit:
    enabled: true

  Compass::*:
    enabled: false


```


 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 