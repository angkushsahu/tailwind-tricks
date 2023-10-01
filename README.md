# Tailwind Tricks to pace up development

### Contents:
1. [Peer and group](#1-Peer-and-group)
1. [Media queries](#2-Media-queries)
1. [Use tailwind custom classes in CSS directly](#3-Use-tailwind-custom-classes-in-CSS-directly)
1. [Custom shadows](#4-Custom-shadows)
1. [Importing Tailwind CSS colors inside tailwind.config.js file](#5-Importing-Tailwind-CSS-colors-inside-tailwindconfigjs-file)
1. [Packages](#6-Packages)

## 1. Peer and group

Group for parent children interaction and peer for sibling interaction.

We can also choose a name for the group and peer as shown below:

**parent style**: group/name

**parent style**: group-hover/name:bg-red-500

This is a *group* example, the same goes for *peer* as well

## 2. Media queries

For example:
**sm:bg-red-500** -> applies a background color of bg-red-500 above sm breakpoint

**sm:max-md:bg-red-500** -> applies a background color of bg-red-500 between the breakpoints sm and md. (Notice that the range syntax is quite different than what we usually write)

**min-[400px]:bg-red-500** -> The screen size should be a minimum of 400px wide so that it can use the bg-red-500 background color

## 3. Use tailwind custom classes in CSS directly

You can achieve this by using the good old **@apply** directive

There is another way of achieving this, follow the syntax below

```css
.my-custom-class {
    color: theme("colors.purple.400");
    padding: theme("padding.2") theme("padding.4");
    border-radius: theme("borderRadius.lg");
}
```

## 4. Custom shadows

```html
<div className="shadow-[0_0_10px_purple]"></div>
```
**OR**
```html
<div className="shadow-[0_0_10px_theme("colors.purple.700")]"></div>
```

Inside the theme.config.js file

```js
theme: {
    extend: {
        boxShadow: {
            neon: "0 0 5px theme("colors.purple.200"), 0 0 20px theme("colors.purple.700")"
        }
    }
}
```
```html
<div className="shadow-neon"></div>
```

## 5. Importing Tailwind CSS colors inside tailwind.config.js file

```js
const colors = require("tailwindcss/colors");

theme: {
    extend: {
        colors: {
            primary: colors.violet
        }
    }
}
```

Default color
```js
const colors = require("tailwindcss/colors");

theme: {
    extend: {
        colors: {
            primary: { ...colors.violet, DEFAULT: colors.violet[500] }
        }
    }
}
```

## 6. Packages

Let's say we have a neon-shadow class which applies a neon shadow to a div, however, we want the same neon version for a lot of different colors (say purple, red etc.)

tailwind.config.js file:
```js
const plugin = require("tailwindcss/plugin");

plugins: [
    plugin(({ addUtilities, theme }) => {
        const neonUtitlies = {};
        const colors = theme("colors");
        for (const color in colors) {
            if (typeof colors[color] === "object") {
                const colorOne = colors[color]["500"];
                const colorTwo = colors[color]["700"];
                neonUtilities[`.neon-${color}`] = {
                    boxShadow: `0 0 5px ${colorOne}, 0 0 20px ${colorTwo}`
                }
            }
        }
        addUtilities(neonUtilities);
    })
]
```

the html file:
```html
<div className="neon-purple"></div> <!-- applies neon purple shadow -->
<!-- OR -->
<div className="neon-red"></div> <!-- applies neon red shadow -->
```
