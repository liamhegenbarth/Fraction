# Fraction
Introducing Fraction. A simple but smart little sass mixin, built to make grids and columns easy to create and update.

Fraction outputs minimal, DRY CSS - only the bare minimum styles and rules to get a grid up and running.


## Creating a grid
Fraction allows you to set columns in lots of different ways.

```
.child {
	@include fraction(4);
}

.child {
	@include fraction(1/4);
}

.child {
	@include fraction(0.25);
}
```

These would then all output `width: 25%;` on the element that fraction() is called on.

```
.child {
	float: left;
	width: 25%;
}
```

Alternatively, you can set ratios of widths independently.

```
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

Which outputs to **super minimal**, **DRY** CSS.

```
.child-1,
.child-2,
.child-3 {
	float: left;
}

.child-1 {
	width: 14.28571%;
}

.child-2 {
	width: 28.57143%;
}

.child-3 {
	width: 57.14286%;
}
```

Fraction also comes with a `has-fraction()` mixin which should be set on the parent container.

This will output a simple `clear-fix` helper to prevent float issues.

This mixin will also accept gutter values, outlined below.

>	**Note** If you don't need to set any margins, and you have your own `clear-fix` rule setup, you don't need to include `has-fraction()` at this stage.


## Applying gutters
Fraction also accepts 2 further arguments for applying margins, though both are not required by default.

>	**Note** A margin rule will only be output in your css if you actually pass margins in.

To set a gutter between your grid items, pass in the `$gutter` width that you want and Fraction will calculate the rest.

You will also need to pass in the same `$gutter` value to `has-fraction()` on the parent container so Fraction can get the alignment correct.

```
.parent {
	@include has-fraction(5%);

	.child {
		@include fraction(3, 5%);
	}
}

.parent {
	@include has-fraction(5rem);

	.child {
		@include fraction(3, 5rem);
	}
}

.parent {
	@include has-fraction(5px);

	.child {
		@include fraction(3, 5px);
	}
}
```

>	**Note** If you pass in a margin value that isn't a %, Fraction will use calc() to calculate widths


## Controlling vertical gutters
The 3rd argument for Fraction is for `margin-bottom`.

```
.parent {
	@include has-fraction(5%);
	
	.child {
		@include fraction(3, 5%, 5%);
	}
}
```
Which outputs to 

```
.parent {
	margin: 0 -2.5%;
}

.child {
	width: 28.33333%;
	margin: 0 2.5% 5%;
}
```

You can also pass a `margin-bottom` value to `has-fraction()` which is independent of whatever you are setting for the child element.

```
.parent {
	@include has-fraction(5%, 12%);
	
	.child {
		@include fraction(3, 5%, 8px);
	}
}
```

# Options
The options for `fraction()` and `has-fraction()` are -

```
@mixin has-fraction($gutter, $margin-bottom);

@mixin fraction($columns, $gutter, $margin-bottom);
```

# Extending Fraction
The idea for fraction was to create a grid system that is quick to call and setup - either for page layouts, or modular element grids - which is independent from controlling and updating classes in markup.

If you prefer to work in this way, you could easily setup your own base helper classes that call `fraction()` instead.

