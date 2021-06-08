# Sass&SCSS

<br>

### Variables
>You can store things like colors,font stacks, or any CSS value you think you'll want to reuse.
>Sass uses the $ symbol to make something a variable. 

<br>

```SCSS
$font-stack: Helvetica, sans-serif;
$main-color: #333;

body {
font: 100% $font-stack;
color: $main-color;
}
```

<br>

### Nesting
>Sass will let you next your CSS selectors in a way that follows the same visual hierarchy of your HTML.
>Be aware that overly nexted rules will result in over-qualified CSS that could prove hard to maintain and 
>is generally considered bad practice.

<br>

```SCSS
$bgcolor : #c1c1c1;
#nested {
  bacground-color:$bgcolor;
  h3 {
    color:$main-color;
    }
  }
```

<br>

### Partials
>You can create partial Sass files that contain little snippets of CSS that you can include in otehr Sass files.
>This is a great way to modularize your CSS and help keep things easier to maintain. 
>A partial is a Sass file named with a leading underscore.
>The underscore lets Sass know that the file is only a partial file and that it should not be generated into a CSS file.
>Sass partials are used with the @use rule. 

<br>

```SCSS
// _base.css
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

<br>

```SCSS
// styles.scss
@use 'base';

.inverse {
  background-color: base.$primary-color;
  color: white;
}
```

<br>

### Mixins 
>A mixin lets you make groups of CSS declarations that you want to reuse throughout your site. 
>You can even pass in values to make your mixin more flexible. 

<br>

```SCSS
@mixin theme($theme:DarkGray){
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, .25);
  color:#fff;
}

.info {
  @include theme;
  }
 
.alert {
  @include theme($theme:DarkRed);
}
```

<br>

>By using the variable $property inside the parentheses so we can pass in a transform of whatever we want.
>After you create your mixin, you can then use it as a CSS declaration starting with @include followed by the name of the mixin.

<br>

### Interpolation 
>Interpolation can be used almost anywhere in a Sass stylesheet to embed the result of a SassScript expression into a chunk of CSS. 
>Just wrap an expression in #{} in any of the following places:
> >Selectors in style rules
> >Property names in declarations
> >Custom property values
> >CSS at-rules
> >@extends
> >Plain CSS @imports
> >Quoted or unqouted strings
> >Special functions
> >Plain CSS function names
> >Loud comments 

<br>

```SCSS
@mixin text-style($name, $bold-or-normal, $size, $color){
  #{$name} {
    font-size:#{$size}px;
    font-weight:#{$bold-or-normal};
    color:#{$color};
  }
}

//text-style applys to interpolation class  
@include text-style(".interpolation", bold, 25, blue); 
```

<br>

```SCSS
//function example
@function invert($color, $amount:100%){
    $inverse:change-color($color,$hue:hue($color)+180);
    @return mix($inverse,$color,$amount);
}

$primary-color:red;
#function{
    background-color: invert($primary-color, 80%);
    h3{
        color:white;
    }
}
```

<br>

>Interpolation can be used in SassScript to inject SassScript into unquoted strings. 
>This is particularly useful when dynamically generating names(e.g. animations),
>or when using slash-separated values. 
>Note that interpolation in SassScript always returns an unquoted string.

<br>


```SCSS
//In SassScript
@mixin inline-animation($duration) {
    $name: inline-#{unique-id()};
  
    @keyframes #{$name} {
      @content;
    }
  
    animation-name: $name;
    animation-duration: $duration;
    animation-iteration-count: infinite;
  }
  
  .pulse {
    @include inline-animation(2s) {
      from { background-color: aquamarine }
      to { background-color: plum }
    }
  }
```

<br>

>Interpolation is useful for injecting values into strings, but other than that it's rarely necessary in SassScript expressions.
>You can just write color: $accent instead of writing color:#{$accent}
> >It's almost always a bad idea to use interpolation with numbers. 

<br>

### Extend/Inheritance
>Using @extend lets you share a set of CSS properties from one selector to another.
> >Let's say there's a simple series of messaging for errors and successes using another feature 
> >which goes hand in hand with extend, placeholder classes.
> >A placeholder class is a special type of class that only prints when it is extended, 
> >and can help keep your compiled CSS neat and clean. 

<br>

```SCSS
/* This CSS will print because %message-shared is extended. */
 %message-shared {
    border: 1px solid #ccc;
    padding: 10px;
    color: #282c34;
  }
  
 // This CSS won't print because %equal-heights is never extended.
  %equal-heights {
    display: flex;
    flex-wrap: wrap;
  }
  
  .message {
    @extend %message-shared;
  }
  
  .success {
    @extend %message-shared;
    border-color: green;
  }
  
  .error {
    @extend %message-shared;
    border-color: red;
  }
```

<br>

>What the above code does is tells .message, .success, .error to behave just like @message-shared.
>That means anywhere that %message-shared shows up, .message, .success and .error will to. 






   
