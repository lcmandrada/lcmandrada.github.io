# CSS

## Format
```css
selector {
    property: value;
}
```


## Location

### Inline Styles (avoid)
```html
<button style="background-color: blue">...</button>
```

### Internal Stylesheet
```html
<style> element
<head>
    <style>
        h2 {
            color: blue;
        }
    </style>
</head>
```

### External Stylesheet
```html
<head>
    <link rel="stylesheet" href="main.css">
</head>
```



## Selectors
```css
/* universal */
* { }

/* element */
h2 { }

/* id */
#id { }

/* class */
.class { }

/* descendant */
ul li { }

/* adjacent */
/* paragraph that comes immediately after an image */
img + p { }

/* child */
div > span { }

/* attribute */
/* a with a title attribute */
a[title] { }

/* links exactly matching with https://example.org */
a[href="https://example.org"] { }

/* links ending in .org */
a[href$=".org"] { }
```

## Pseudo-Classes
```css
/* element:class */
a:hover { }
```

## Pseudo-Elements
```css
/* element::sub-element */
span::before { }
```

## Specificity
1. `!important` (avoid)
2. Inline Styles (avoid)
3. ID
4. Class
5. Element

## Relative Units
1. `%` - relative to some value
2. `em` - relative to parent or contained font size; can grow or srhink if nested
3. `rem` - relative to root; solves `em` nested issue



## Properties

### Box Model
```css
/* dimension */
width: width;
height: height;

/* border */
border: width style color;

/* padding */
padding: top right bottom left;

/* margin */
margin: top right bottom left;
```

### Display
```css
img {
    display: display;
}
```

1. `inline`
2. `block`
3. `inline-block` - respects vertical, horizontal, width, height unlike inline
4. `flex` - automatically arranges elements depending on the screen size

### Transparency
```css
/* alpha */
h2 {
    color: rgba(255, 255, 255, 0.7);
}

/* opacity */
img {
    opacity: 0.7;
}
```

### Position
```css
img {
    position: position;
}
```

1. `static` - default
2. `relative` - offset from current position
3. `absolute` - no space alloted; relative to parent or container
4. `fixed` - absolute but fixed in the view
5. `sticky` - not initially fixed but gets fixed when moved

### Transition
```css
transition: property duration function delay, other-properties;
```

### Transform
Includes child in the transformation
```css
transform: function other-functions;
```

1. `rotate`
2. `scale`
3. `translate`
4. `skew`

### Background
```css
background: repeat position/size url, other-backgrounds;
background: no-repeat center/80% url("../img/image.png");
```



## Flex Model
1. **Main Axis** - x-axis by default
2. **Cross Axis** - y-axis by default

### Properties
1. `flow-direction` - main axis direction
2. `justify-content` - alignment on main axis
3. `flex-wrap` - wrapping on cross axis
4. `align-items` - alignment on cross axis
5. `align-content` - space between elements on cross axis; must have flex-wrap
6. `align-self` - alignment of the element itself on cross axis; applied on the element, not the container

### Size
Applied on elements
```css
flex: grow shrink basis;
```

1. `flex-basis` - size on main axis
2. `flex-grow` - takes available space; proportion
3. `flex-shrink` - how much element shrinks with limited space; proportion

### Demo
[Minimal Visual Cheat Sheet](https://flexbox.malven.co/)  
[Interactive Cheat Sheet](https://yoksel.github.io/flex-cheatsheet/)



## Media Query
```css
@media (min-width: 300px) and (max-width: 700px) {
    styles
}

/* example */
h1 {
    color: black;
}
@media (min-width: 300px) {
    h1 {
        color: blue;
    }
}
@media (min-width: 700px) {
    h1 {
        color: cyan;
    }
}
```



## Notes

### Use MDN CSS Cookbook
[Cookbook](https://developer.mozilla.org/en-US/docs/Web/CSS/Layout_cookbook)

### Use a CSS reset template
[Mini Reset](https://github.com/jgthms/minireset.css)

### Use Bootstrap for easy CSS framework

### Use Bootstrap guidelines for breakpoints

### Footer at the bottom of the page
1. Set container to flex column with `100vh`
2. Set footer margin to auto



## Cookbook

### Divider before every element
```css
.post h2:before {
    content: '';
    background: no-repeat left/contain url(/public/divider.svg);
    display: block;
    margin: 2rem auto;
    height: 1rem;
    filter: invert(63%) sepia(50%) saturate(6257%) hue-rotate(181deg) brightness(89%) contrast(83%);
}
```