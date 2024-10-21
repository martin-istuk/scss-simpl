# scss-simpl

scss-simpl is a lightweight SCSS library offering a set of reusable SCSS mixins for common styling needs.

1. viewports() - Apply styles for multiple viewports in one step.
2. grid-areas() - Create grid layouts using named areas.
3. bg-cover() - Set a background image with center/cover alignment.
4. clip-text() - Clip overflowing text, with optional support for line clamping.

# How to use

1. To add the library, use [npm](https://www.npmjs.com/):

```npm
npm install scss-simpl
```

2. Use it in your SCSS file:

```scss
@use "scss-simpl" as sl;
```

# Usage examples

## viewports() :: Responsive styles for multiple viewports

The viewports() mixin allows you to define styles across different screen widths simultaneously, simplifying media queries.

It also provides easier tracking of changes to properties across multiple window widths.

The mixin requires an argument as an SCSS map (notice the double parentheses after @include), in this form:

1. Breakpoints map property (optional):
   - As key, use "use-min" for min-width or "use-max" for max-width to specify the type of media query,
   - Default breakpoints are: global, 600px, 1200px, 1800px and 2400px,
   - You can override these values as needed.

2. Style map properties (required):
   - Keys represent CSS properties,
   - Values are SCSS lists of property values corresponding to the breakpoints.

```scss
// Include:
.selector {
  @include sl.viewports((
    background: (red, url("/path"), "linear-gradient(red, blue)"),
    margin-bottom: (12px, _, _, 16px),
    border-width: (1px, 2px),
  ));
}

// Output:
.selector {
  background: red;
  margin-bottom: 12px;
  border-width: 1px;
  @media screen and (min-width: 600px) {
    background: url("/path");
    border-width: 2px;
  }
  @media screen and (min-width: 1200px) {
    background: linear-gradient(red, blue);
  }
  @media screen and (min-width: 1800px) {
    margin-bottom: 16px;
  }
}
```

You can customize breakpoints and use "max-width" instead of "min-width":

```scss
// Include:
.selector {
  @include sl.viewports((
    use-max:       (global, 700px, 1300px)
    margin-bottom: (12px,   14px),
    font-size:     (20px,   _,     22px),
    background:    (white,  grey,  black),
  ));
}

// Output:
.selector {
  margin-bottom: 12px;
  font-size: 16px;
  background: white;
  @media screen and (max-width: 699px) {
    margin-bottom: 22px;
    background: grey;
  }
  @media screen and (max-width: 1299px) {
    font-size: 22px;
    background: black;
  }
}

// Note: The "use-max" option decreases breakpoints by 1px to prevent conflicts between different media query types used on the same page
```

For custom consistent breakpoints across styles, you can create your own reusable mixin:

```scss
// Create:
@mixin my-mixin($prop-map) {
  $options: (use-max: (2000px, 1500px, 1000px, 500px));
  @include sl.viewports(map-merge($options, $prop-map));
};

// Include:
.selector {
  @include my-mixin((
    color: (red, green),
    padding: (16px, _, 12px),
  ));
}

// Output:
.selector {
  @media screen and (max-width: 1999px) {
    color: red;
    padding: 16px;
  }
  @media screen and (max-width: 1499px) {
    color: green;
  }
  @media screen and (max-width: 999px) {
    padding: 12px;
  }
  // Breakpoints with no property values are ignored (500px).
}
```
Example with multiple breakpoints and properties:

```scss
// Include:
.selector {
  @include sl.viewports((
    margin:     (8px,   _,     12px 24px),
    background: (red,   green, "linear-gradient(to left, red, blue)"),
    padding:    (12px,  _,     _,    22px),
    color:      (white, black),
    font-size:  (8px,   10px,  12px, 14px, 16px),
  ));
}

// Output:
.selector {
  margin: 8px;
  background: red;
  padding: 12px;
  color: white;
  font-size: 8px;
  @media screen and (min-width: 600px) {
    background: green;
    color: black;
    font-size: 10px;
  }
  @media screen and (min-width: 1200px) {
    margin: 12px 24px;
    background: linear-gradient(to left, red, blue);
    font-size: 12px;
  }
  @media screen and (min-width: 1800px) {
    padding: 22px;
    font-size: 14px;
  }
  @media screen and (min-width: 2400px) {
    font-size: 16px;
  }
}
```

## grid-areas() :: Create grid layouts with named areas

This mixin simplifies grid layouts by assigning named areas to child elements based on class or ID names.

```scss
// Include:
.selector {
  @include sl.grid-areas(".foo .bar", "#baz #baz");
}

// Output:
.selector {
  display: grid;
  grid-template-areas: "foo bar", "baz baz";
  .foo {
    grid-area: foo;
  }
  .bar {
    grid-area: bar;
  }
  #baz {
    grid-area: baz;
  }
}
```

## bg-cover() :: Set a full-cover background image

Use this mixin to set a background image with center alignment and full coverage.

```scss
// Include:
.selector {
  @include sl.bg-cover("path/to/image.svg");
}

// Output:
.selector {
  background: transparent url("path/to/image.svg") center/cover no-repeat;
}
```

## clip-text() :: Clip overflowing text with optional line clamping

This mixin limits text overflow and optionally clamps the number of lines. First argument is the element max-width and the second is number of lines to clamp. Both arguments are optional and default to "auto" and "1".

```scss
// Include:
.selector {
  @include sl.clip-text(150px, 4);
}

// Output:
.selector {
  max-width: 150px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  display: -webkit-box;
  white-space: initial;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
}
```

# License

This library is licensed under the [MIT](https://choosealicense.com/licenses/mit/) license.
