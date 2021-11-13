# SCSS template

Repository with usefull scss template, and some usefull mixins

## Table of contents

- [Directory Layout](#directory-layout)
- [Libraries](#libraries)
- [Mixins](#mixins)

## Directory Layout

    ├─ libs             # Folder with libraries
    ├─ main             # Folder with scaffolding styles and variable initialization
    ├─ mixins           # Folder with mixins
    ├─ variables        # Folder with variables
    ├──── colors        # Folder were colors/themes situated
    ├─ index.scss       # Main scss file
    ├─ Readme.md        # Documentation

## Libraries

- Normalize.css: For normalizing css styles in Browsers

## Mixins

Here you can find description for mixins (grupped by usage)

- [Animations](#animations)
- [Responsive](#responsive)
- [Fonts](#fonts)
- [Container](#container)
- [Themes](#themes)
- [To css var](#to-css-var)
- [Else](#else)

## Animations

### Hover

    @mixin hover {
      @media (hover: hover) {
        &:hover {
          @content;
        }
      }
    }

### Animation
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin animation($name, $duration: 300, $delay: 0) {
      @if map-has-key($animations, $name ) {
        animation-name: $name;
        animation-fill-mode: both;
        animation-duration: #{$duration}ms;
        animation-delay: #{$delay}ms;
        animation-timing-function: ease-in-out;
      }
    }

## Responsive

### Media from
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin media-from($breakpoint) {
      @if map-has-key($breakpoints, $breakpoint) {
        @media (min-width: map-get($breakpoints, $breakpoint)) {
          @content;
        }
      }

      @else {
        @media (min-width: #{$breakpoint}px) {
          @content;
        }
      }
    }

### Media to
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin media-to($breakpoint) {
      @if map-has-key($breakpoints_down, $breakpoint) {
        @media (max-width: map-get($breakpoints_down, $breakpoint)) {
          @content;
        }
      }

      @else {
        @media (max-width: #{$breakpoint}px) {
          @content;
        }
      }
    }

### Media From to
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin media-from-to($min, $max) {
      @if map-has-key($min, $breakpoint) and map-has-key($max, $breakpoint) {
        @media (min-width: map-get($min, $breakpoint)) and (max-width: map-get($min, $breakpoint)) {
          @content;
        }
      }

      @if map-has-key($min, $breakpoint) and not (map-has-key($max, $breakpoint)) {
        @media (min-width: map-get($min, $breakpoint)) and (max-width: #{$max}px) {
          @content;
        }
      }

      @if not (map-has-key($min, $breakpoint)) and map-has-key($max, $breakpoint) {
        @media (min-width: #{$max}px) and (max-width: map-get($max, $breakpoint)) {
          @content;
        }
      }
    }

## Fonts
### Font
Add font style to element

    @mixin font-... {
      ...
    }

### Add Font
Register font
#### Arguments
- $font
- $weight
- $location
- $ext

body:

    @mixin add-font($font: $base-font, $weight, $location, $ext) {
      @font-face {
        font-family: $font;
        font-style: normal;
        font-display: swap;
        font-weight: $weight;

        @if $ext == woff2 {
          src: url('../assets/font/#{f$location}.woff2') format('woff2');
        }

        @if $ext == woff {
          src: url('../assets/font/#{$location}.woff') format('woff');
        }

        @if $ext == ttf {
          src: url('../assets/font/#{$location}.ttf') format('truetype');
        }
      }
    }

## Container
descr

    @mixin container {
      margin: 0 auto;
      padding: 0 var(--grid-spacing-mobile);
      width: 100%;
      max-width: 100%;
      @include media-from('sm') {
        max-width: 540px;
      }
      @include media-from('md') {
        max-width: 720px;
      }
      @include media-from('lg') {
        padding: var(--grid-spacing-desktop);
        max-width: 900px;
      }
      @include media-from('xxl') {
        max-width: 1300px;
      }
      @include media-from('fhd') {
        max-width: 1620px;
      }
    }

## Themes
### Map get strict
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @function map-get-strict($map, $key, $themeKey) {
      @if map-has-key($map, $key) {
        @return map-get($map, $key);
      }

      @else {
        @error "ERROR: Specified color key '#{$key}' does not exist in theme '#{$themeKey}'";
      }
    }

### Load theme
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin load-theme($themeMap, $themeKey) {
      @at-root {
        html#{if($themeKey == "default", "", "[#{$themeKey}]")} {
          @each $key in $theme-keys {
            --color-#{$key}: #{map-get-strict($themeMap, #{$key}, $themeKey)};
          }
        }
      }
    }

## To css var
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin to-css-var($array, $prefix: null) {
      @at-root {
        html#{''} {
          @each $name, $value in $array {
            @if ($prefix) {
              --#{$prefix}-#{$name}: #{$value};
            } @else {
              --#{$name}: #{$value};
            }
          }
        }
      }
    }

## Else

### No user select
descr

    @mixin no-user-select {
      user-select: none;
    }

    @mixin disable-scrollbar {
      -ms-overflow-style: none;  /* IE and Edge */
      scrollbar-width: none;  /* Firefox */

      &::-webkit-scrollbar {
        display: none;
      }
    }

### Border style
descr
#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin border-style($color, $radius: null) {
      @if $color == transparent {
        border: 1px solid $color;
      }

      @else if $color != null {
        border: 1px solid var($color);
      }

      @if $radius != null {
        border-radius: $radius;
      }
    }

### Flex center
descr

    @mixin flex-center {
      display: flex;
      align-items: center;
      justify-content: center;
    }

### Absolute center
descr

    @mixin absolute-center {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }

### Width Height
descr

#### Arguments
- $var1
- $var2
- $var13

body:

    @mixin size($width, $height: $width) {
      width: $width;
      height: $height;
    }

### Disabled styles
descr

    @mixin disabled-styles {
      &:disabled,
      &[disabled],
      &[disabled="disabled"] {
        @content;
      }
    }
