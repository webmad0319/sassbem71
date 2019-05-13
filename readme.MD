# 7-1 Pattern

https://sass-guidelin.es/#the-7-1-pattern

```
sass/
|
|– abstracts/
|   |– _variables.scss    # Sass Variables
|   |– _functions.scss    # Sass Functions
|   |– _mixins.scss       # Sass Mixins
|   |– _placeholders.scss # Sass Placeholders
|
|– base/
|   |– _reset.scss        # Reset/normalize
|   |– _typography.scss   # Typography rules
|   …                     # Etc.
|
|– components/
|   |– _buttons.scss      # Buttons
|   |– _carousel.scss     # Carousel
|   |– _cover.scss        # Cover
|   |– _dropdown.scss     # Dropdown
|   …                     # Etc.
|
|– layout/
|   |– _navigation.scss   # Navigation
|   |– _grid.scss         # Grid system
|   |– _header.scss       # Header
|   |– _footer.scss       # Footer
|   |– _sidebar.scss      # Sidebar
|   |– _forms.scss        # Forms
|   …                     # Etc.
|
|– pages/
|   |– _home.scss         # Home specific styles
|   |– _contact.scss      # Contact specific styles
|   …                     # Etc.
|
|– themes/
|   |– _theme.scss        # Default theme
|   |– _admin.scss        # Admin theme
|   …                     # Etc.
|
|– vendors/
|   |– _bootstrap.scss    # Bootstrap
|   |– _jquery-ui.scss    # jQuery UI
|   …                     # Etc.
|
`– main.scss              # Main Sass file
```

# BEM Pattern

http://getbem.com/naming/

The BEM approach ensures that everyone who participates in the development of a website works with a single codebase and speaks the same language. Using proper naming will prepare you for the changes of the design of the website.

##Block

Encapsulates a standalone entity that is meaningful on its own. While blocks can be nested and interact with each other, semantically they remain equal; there is no precedence or hierarchy. Holistic entities without DOM representation (such as controllers or models) can be blocks as well.

Naming: Block names may consist of Latin letters, digits, and dashes. To form a CSS class, add a short prefix for namespacing: `.block`

Example: `<div class="block">...</div>`

##Element

Parts of a block and have no standalone meaning. Any element is semantically tied to its block.

Naming: Element names may consist of Latin letters, digits, dashes and underscores. CSS class is formed as block name plus two underscores plus element name: `.block__elem`

Example: `Example: <div class="block">
	  ...
	  <span class="block__elem"></span>
	</div>`

##Modifier

Flags on blocks or elements. Use them to change appearance, behavior or state.

Naming: Modifier names may consist of Latin letters, digits, dashes and underscores. CSS class is formed as block’s or element’s name plus two dashes: `.block--mod` or `.block__elem--mod` and `.block--color-black` with `.block--color-red`. Spaces in complicated modifiers are replaced by dash.

Example: `<div class="block block--mod">...</div>
	<div class="block block--size-big
		block--shadow-yes">...</div>`

##General Example

###HTML

``` 
<form class="form form--theme-xmas form--simple">
  <input class="form__input" type="text" />
  <input
    class="form__submit form__submit--disabled"
    type="submit" />
</form>
```

###CSS

```
.form { }
.form--theme-xmas { }
.form--simple { }
.form__input { }
.form__submit { }
.form__submit--disabled { }
```

#SASS
##How to install

`Npm i node-sass`

And then rename all files from css to scss, including all the imports in all the files.

#Variables

```
$direction: 'left';

$value: 42; // Yep
$length: $value * 1px;
```

#Calculations

```
.foo {
  width: (100% / 3);
}
```

#Colors

```
$sass-pink: hsl(330, 50%, 60%);

$main-theme-color: $sass-pink;
```

#Mixed examples

```
.foo, .foo-bar,
.baz {
  $length: 42em;

  @include ellipsis;
  @include size($length);
  display: block;
  overflow: hidden;
  margin: 0 auto;

  &:hover {
    color: red;
  }
}
```

###Mixins

Source: https://www.competa.com/blog/sass-mixins-and-functions/

Mixins are written in Sass as @mixin and can be inserted into an element by using the @include directive. Most developers used mixins until proper functions got introduced in Sass 3.1.0. This is because it can pass in arguments, contain selections and parent references. They obtain full CSS rules where properties that are dynamic will be passed into arguments. By using arguments, it allows you a wide variety of styles and makes it easier to re-use.

```
   @mixin border-radius($radius) {
        -webkit-border-radius: $radius;
           -moz-border-radius: $radius;
            -ms-border-radius: $radius;
                border-radius: $radius;
    }

    .block { 
        @include border-radius(2px); 
    }
```

Will generate

```
   .block {
        -webkit-border-radius: 2px;
           -moz-border-radius: 2px;
            -ms-border-radius: 2px;
                border-radius: 2px;
    }
```

###Functions

Functions are written in Sass as @function followed by a function name and arguments. Since functions work with the value of a style attribute, they are great for calculating values that you may need to re-use elsewhere. Sass has in-built functions that you can use, but if you prefer/need other functions, you can also create them yourself. In case you create them, please note that the best practice is to use short names and no camelCase or snake_case. A big pro with functions is that you can port them into other projects!

```
   @function function-name($arg1, $arg2) {
        @return $arg1 + $arg2
    }
```

Use

```
   .element {
        padding: function-name(20px, 15px);
    }
```

Output:

```
   .element  {
        padding: 35px;
    }
```

###Extend

```
%button {
  display: inline-block;
  // ... button styles

  // Relationship: a %button that is a child of a %modal
  %modal > & {
    display: block;
  }
}

.button {
  @extend %button;
}

// Yep
.modal {
  @extend %modal;
}
```

Mixing for different vendors

```
@mixin transform($value) {
  -webkit-transform: $value;
  -moz-transform: $value;
  transform: $value;
}
```

###Conditions

```
@function dummy($condition) {
  @if $condition {
    @return true;
  }

  @return false;
}
```

###For

```
@for $i from 1 through 10 {
  .foo:nth-of-type(#{$i}) {
    border-color: hsl($i * 36, 50%, 50%);
  }
}
```