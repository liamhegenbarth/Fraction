# Fraction
Introducing Fraction. A simple yet powerful sass mixin, built to make grids and columns easy to create and update.

Fraction allows you to easily set a layout with as much control as you need, accepting Shorthand and Advanced options to achieve this speed.

Fraction relies on the `nth-child` or optional `nth-of-type` selector, with automatic clears and margin calculations.


# Shorthand
The shorthand call accepts 3 params - `columns`, `gutter` and `bottom`. Fraction assumes this method will be called to create an inline column layout (i.e, not a grid, but to perhaps evenly distribute 4 blocks in a panel). Grids should be made using the Advanced options.


## Columns

```sass
.element {
	@include fraction(4);
}

.element {
	@include fraction(1/4);
}

.element {
	@include fraction(0.25);
}
```

These would then all output `width: 25%;` on the element that fraction() is called on.

```sass
.element {
	float: left;
	width: 25%;
}
```

Alternatively, you can set the fractions of widths independently.

```sass
.child-1 {
	@include fraction(1/7);
}

.child-2 {
	@include fraction(2/7);
}

.child-3 {
	@include fraction(4/7);
}
```

Which outputs to the following CSS.

```sass
.child-1 {
	float: left;
	width: 14.28571%;
}

.child-2 {
	float: left;
	width: 28.57143%;
}

.child-3 {
	float: left;
	width: 57.14286%;
}
```
>	**Note** Fraction doesn't use a %placeholder to combine floats to allow it to be called inside @media queries


## Gutters
Fraction also accepts 2 arguments for applying margins, controlling horizontal and vertical spacing.

To set a gutter between your columns, pass in the `$gutter` width that you want and Fraction will calculate the rest.

Similarly, the final `$bottom` argument sets a bottom margin value.

```sass
.element
	@include fraction(3, 5%);
}

.element
	@include fraction(3, 5rem);
}

.element
	@include fraction(3, 5rem, 10px);
}

.element
	@include fraction(3, 5px, 2rem);
}
```
>	**Note** If you pass in a margin value that isn't a %, Fraction will use calc() to calculate widths


# Advanced 
The advanced call accepts a map of params and values, allowing for more control over the output. Fraction assumes this method will be called to create a grid (e.g, a 4x4 grid of boxes), or to change direction (e.g, align right). 

>	**Note** Grids automatically control clears and resets on rows, where as the Shorthand options do not.


## Debug
```sass
type : [boolean]
default : false
options : true | false
```
Helper param to output to console a log of all accepted keys and types (as below).


## Selector
```sass
type : [string]
default : 'child'
options : 'child' | 'type' | 'matches'
```
Tell Fraction which pseudo selector to use. Automatically interpolates with `nth-{$selector}`, which provides future support for CSS4 `nth-matches`.

The `nth-of-type` pseudo is particularly useful if there are extra elements in the grid container that would otherwise break the grid.


## Grid
```sass
type : [boolean]
default : false
options : true | false
```
Setting this param tells Fraction to automatically set clear rules on the first element of each row. 

Fraction will also output a reset in case it has been previously called on this element (e.g, changing the grid from a 2x4 to a 4x2 with an `@media` query).

>	**Note** the `grid` option should be used in every Fraction instance after the first initial call, even if subsequently going to a single row grid. This is the only way to get the automatic resets of clear rules.


## Columns
```sass
type : [integer | decimal | fraction | list]
default : 1
options : 3, 0.3, 1/3, (1/3, 2/3)
```
The only required param, pass in a single value for Fraction to apply this column width to all child elements. Passing in a [list] causes Fraction to set each value on the multiple of that element. For example, `1/3` would be set on `odd` children, `2/3` would be set on `even` children.


## Align
```sass
type : [string]
default : 'left'
options : 'left' | 'right'
```
Set the float value for Fraction to use on the child elements.


## Ratio
```sass
type : [integer | decimal | fraction]
default : none
options : 2, 0.5, 1/2
```
Implements a `padding-bottom` percentage trick to create responsive squares or rectangles. Setting a ratio of `1/1` creates a perfect square; adjusting the ratio will create a porportioned vertical or horizontal rectangle.

>	**Note** use `position : absolute` on the inner content to complete the effect. 


## Ratios
```sass
type : [list]
default : none
options : (1/2, 1/1)
```
List version of `ratio`, allows you to set independent ratios across `columns`. Each ratio in the list will be set on the same index of column that Fraction creates.

>	**Note** Fraction will expect the number of ratios passed in to match the number of columns it makes.


## Gutter
```sass
type : [number]
default : none
options : 1rem, 10px, 10%
```
Set the gutter width between all columns. Fraction will uses `calc()` if the units are not a percentage.


## Gutters
```sass
type : [list]
default : none
options : (1rem, 10px)
```
List version of `gutter`, allows you to set independent gutters across `columns`. Each gutter in the list will be set on the same index of column that Fraction creates.

>	**Note** Fraction will expect the number of gutters passed in to be 1 less than the number of columns it makes.


## Bottom
```sass
type : [number]
default : none
options : 1rem, 10px, 10%
```
Set a bottom margin on all child elements. An optimised shorthand version of `margin` is used depending on the combination of `gutter` and `bottom` that is used.


## IE Fix
```sass
type : [boolean]
default : false
options : true | false
```
Setting this param fixes IE render issues with `calc()` and `100%` calculations. Fraction will use a value of `99.9999%` where needed for any `calc()` operations.



# Options
The options for `fraction()` are -

```sass
// shorthand
@include fraction($columns, $gutter, $bottom);

// advanced
@include fraction((
	debug 	: 'boolean',
	selector: 'string',
	grid 	: 'boolean',
	columns : 'integer | decimal | fraction | list',
	align	: 'left | right',
	ratio 	: 'integer | decimal | fraction',
	ratios 	: 'list',
	gutter	: 'number',
	gutters : 'list',
	bottom	: 'number',
	ie-fix  : 'boolean'
));
```

# Extending Fraction
The idea for fraction was to create a grid system that is quick to call and setup - either for page layouts, or modular element grids - which is independent from controlling and updating classes in markup.

If you prefer to work in this way, you could easily setup your own base helper classes that call `fraction()` instead.

