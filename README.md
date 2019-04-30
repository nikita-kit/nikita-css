# nikita.css

This is our methodology how to write efficient and scalable (S)CSS for big websites.

Latest Release: [![GitHub version](https://badge.fury.io/gh/nikita-kit%2Fnikita-css.png)](https://github.com/nikita-kit/nikita-css/releases)

If you want to start a new project from scratch, try [nikita.generator](https://github.com/nikita-kit/generator-nikita).  
If you're interested in HTML patterns, code snippets and best practices, try [nikita.html](https://github.com/nikita-kit/nikita-html).  
If you're interested in our Javascript conding style guide, try [nikita.js](https://github.com/nikita-kit/nikita-js).  


## Contents

### Methodology

We're using some variation of BEM+SMACSS+opinionatedexperienceofcssdevelopmentyears:

Our site exists of basic __blocks__. Blocks are independent parts of the site (e.g. menu, metanav, login form, sidebar, user detail page). Like explained at [yandex's BEM](http://img-fotki.yandex.ru/get/5008/221798411.0/0_babce_7deef28f_XXL.png).
The blocks may contain other blocks.

The smallest entities of a block are called __elements__. For instance the block 'menu' contains multiple items, a login block may contain a username element, password element and a submit button element. Like explained at [yandex's BEM](http://img-fotki.yandex.ru/get/6726/221798411.0/0_babd1_f14000fa_XL.png).

Blocks and elements may be modified with __modifiers__. For instance the selected menu item is a modified version of the menu item.

- Blocks
	- are prefixed with `b-`
	- __good:__ b-menu, b-sidebar, b-sitemap, b-user
	- __bad:__ menu, sidebar, sitemap, user
- Elements
	- have _no prefix_ and can only be defined in block scope
	- to avoid conflicts with nested blocks elements should be named unique to their corresponding block. 
	- __good:__ menu-item, sidebar-title, user-text
	- __bad:__ item, title, text (instead of user or avatar)
- Modifier
	- are prefixed with `is-` or `has-`, and have to be defined in block or element scope
	- __good:__ is-selected, is-active, has-items
	- __bad:__ x-selected, active, m-items


#### Example

File `_menu.scss` in `source/sass/blocks` directory.

```
.b-menu { /* block: 'b-menu' */
	&.is-static { /* modifier: 'is-static' for b-menu  */
		…
	}

	.menu-item { /* element: 'item' in b-menu */
		…
	}
}
```


#### Class-Naming

Component Blocks:

- b-eventlist
- b-linklist
- b-sitemap
- b-teaser-text
- b-teaser-video
- …


#### Commenting

Add comments at the line where they belong to or above the ruleset that you want to comment.

```
/* Rule specific comment */
.box {
	position: fixed;
	transform: translate3d(0, 0, 0); /* Enable hardware acceleration */
	
	/* Rule specific comment */
	.csspositionsticky & {
		position: sticky; /* On anything except top aligned elements this is buggy in Chrome <37 and iOS 7+ */
	}
}
```


### CSS Coding Style

(This list is not intended to be exhaustive.)

- Use lowercase for class names.
- Be consistant with indentation – we use spaces instead of tabs.
- Be consistent in declaration order, cluster related properties (Positioning, Box-Model, Text & Color). I'm no fan of an alphabetical order.
- Be consistant with quotes – I'm using double quotes `""`.
- Quote attribute values in selectors, e.g. `input[type="checkbox"]`.
- One selector per line, one rule per line.
- Put spaces after `:` in property declarations.
- Put spaces before `{` in rule declarations.
- Put a `;` at the end of the last declaration in a declaration block.
- Include a space after each comma in comma-separated property or function values, e.g. `rgba(0, 0, 0, 0)`.
- Separate each ruleset by a blank line.


### CSS Coding Guidelines

(This list is not intended to be exhaustive.)


#### Avoid dangerous selectors

If a selector is too generic, it's dangerous. In 99% of cases you have to overwrite this rule somewhere. Be more specific. Try using a class instead. (Exception: CSS-Resetstyles)

__bad__

```
header { … }
h2 { … }
ul { … }
```

__good__

```
.header { … }
.subtitle { … }
.linklist { … }
```


#### Avoid element selectors

Element selectors are expensive. Like the rule above, be more specific. Try using a class instead. Furthermore elements like `<div />` and `<span />` should always have a class-attribute in your markup.

__bad__

```
.foo div { … }
.foo span { … }
.foo ul { … }
```

__good__

```
.foo .section { … }
.foo .title { … }
.foo .linklist { … }
```


#### Avoid IDs where possible

IDs should never be used in CSS. Use IDs in HTML for fragment identifiers and maybee JS hooks but never in CSS because of their heightened specificity and because they can never be used more than once in a page.

Though you should use IDs in forms to connect `<input />` and `<label />` with the `for`-attribute.

__bad__

```
#sidebar
```

__good__

```
.sidebar
```


#### Avoid qualifying class names with element selectors

It's counterproductive because you unnecessary heighten the specifity.

__bad__

```
ul.linklist { … }
div.example { … }
a.back { … }
```

__good__

```
.linklist { … }
.example { … }
.back { … }
```


#### Avoid the descendant selector

The descendant selector is the most expensive selector in CSS. You should target directly if possible.

__bad__

```
html body .linklist li a { … }
```

__good__

```
.linklist-link { … }
```


#### Avoid deep nesting

Following to the rule above you should also try to nest your selectors maximum 3 levels deep.

__bad__

```
.navlist li a span:before { … }
```

__good__

```
.navlist .info:before { … }
```


#### Avoid using the same selctor for styling and JS

Separation of concerns

__bad__

```
.dialog-opener { … }
```

```
$('.dialog-opener')…
```

__good__

```
.dialog-opener { … }
```

prefixed with `js-`

```
$('.js-dialog-opener')…
```

or use data-attributes:

```
$('[data-dialog-opener]')…
```


#### Avoid using native language

The English language has proven itself among coders as the standard.

__bad__

```
.share-buttons .teilen a {
	background: url("../img/icons/facebook-teilen.png") no-repeat 0 0;
}
```

__good__

```
.share-buttons .facebook-share a {
	background: url("../img/icons/facebook-share.png") no-repeat 0 0;
}
```


#### Use shorthand properties where possible

It's shorter and easier to read.

__bad__

```
.box {
	padding-top: 0;
	padding-right: 10px;
	padding-bottom: 20px;
	padding-left: 10px;
}
```

__good__

```
.box {
	padding: 0 10px 20px;
}
```


#### prefer rem units whenever possible

Then it's very easy to modify the font-size of the hole page by just editing the font-size of the `html` element.
It's also recommended to set the default `font-size` of the `html` element to `1px`.
This ensures that 1rem = 1px.

__bad__

```
.box {
	font-size: 12px;
	line-height: 16px;
}
```

__good__

```
.box {
	font-size: 12rem;
	line-height: 16rem;
}
```


#### Omit unit specification after “0” values

Zero is zero. :)

__bad__

```
.box {
	margin: 0px;
}
```

__good__

```
.box {
	margin: 0;
}
```

__exception, where you don't omit__

```
.box {
	transform: rotate(0deg);
}
```


#### Use hexadecimal color codes unless using rgba or hsl

In most cases the hex code is shorter than the color names, so you could save some bits.

__bad__

```
.box {
	color: orange;
}
```

__good__

```
.box {
	color: #ffa500;
}
```


#### Use 3 character hexadecimal notation where possible

Like above, it's shorter and saves some bits.

__bad__

```
.box {
	color: #ff0099;
}
```

__good__

```
.box {
	color: #f09;
}
```


#### Use number keywords 100–900 for font-weight

It's the typographic standard to use number keywords. Like above it's also shorter and saves some bits.

__bad__

```
.box {
	font-weight: normal;
}
```

__good__

```
.box {
	font-weight: 400;
}
```


#### Separate words in class names by a hyphen

It's easier to read and to select the fragments by using `shift + alt + left/right-arrow`.

__bad__

```
.user_avatar { … }
.userAvatar { … }
.useravatar { … }
```

__good__

```
.user-avatar { … }
```


#### Don't use !important

Self-explanatory I hope. :)
It may be ok to use it on helper classes though.


#### Avoid using conditional stylesheets

Better wrap your html-element in conditional comments and then use the html-class, e.g. `.lt-ie9` to style directly in your component-block.

__bad__

```
<!--[if IE 9]><link href="ie9.css" rel="stylesheet" /><![endif]-->
```

__good__

```
<!--[if IE 9]> <html class="no-js lt-ie10 ie9" lang="de"> <![endif]-->
<!--[if gt IE 9]><!--> <html class="no-js" lang="de"> <!--<![endif]-->
```


### SASS structure

The main SCSS-file is `styles.scss`. It imports all partials. `variables`, `mixins`, `extends`, `icons` and `blocks` will be imported with a globbing-pattern.
It's important that _every block-component_ gets its own partial and is put into the `blocks`-folder! Use subfolders if your site uses lots of partials.

This is how the `sass`-folder looks like:

```
$ tree
.
├── _basics.scss
├── _reset.scss
├── _webfonts.scss
├── blocks
│   ├── _aside.scss
│   ├── _footer.scss
│   ├── _header.scss
│   ├── _nav.scss
│   └── …
├── extends
│   ├── _buttons.scss
│   ├── …
├── mixins
│   ├── _triangle.scss
│   └── …
├── styles.scss
└── variables
    ├── _breakpoints.scss
    ├── _color.scss
    ├── _timing.scss
    ├── _typography.scss
    └── …
```

Some explanation:

- __basics.scss__ – basic styles, some normalizing
- __reset.scss__ – global browser reset by [Eric Meyer](http://meyerweb.com/eric/tools/css/reset/)
- __webfonts.scss__ – use it for `@font-face`-declarations
- __blocks/__ – all block-component-partials go in here
- __extends/__ – put your placeholder-extends in here
- __mixins/__ – put your mixins in here
- __styles.scss__ – main stylesheet, imports all partials
- __variables/__ – put your variables in here, e.g. `color`, `typography` etc.


### SASS Coding Guidelines

Someone said: »Preprocessors do not output bad code. Bad developers do.« That's why it's important to have a common ruleset.
If you work in a team with other frontend developers you get the following benefits: maintainability, scalability, efficiency, you avoid conflicts from the beginning and last but not least you save time for the finer things. :)


#### Syntax

We're using SCSS-syntax because it's valid CSS and more expressive in our eyes.


#### Order of definition

If you have a consistent order of definition, everybody can scan the code on first sight.

__List media queries first__

```
.b-foo {
	
	// Media Queries
	@include respond-to(desktop) {
		padding: 10px;
	}
	
}
```

__List global styles beginning with @extend second (separated by a blank line)__

```
.b-foo {
	
	// Media Queries
	@include respond-to(desktop) {
		padding: 10px;
	}
	
	// Global Styles
	@extend %module;
	
}
```

__List @include third__

```
.b-foo {
	
	// Media Queries
	@include respond-to(desktop) {
		padding: 10px;
	}
	
	// Global Styles
	@extend %module;
	@include centering(horiz);
	
}
```

__List regular styles next__

```
.b-foo {
	
	// Media Queries
	@include respond-to(desktop) {
		padding: 10px;
	}
	
	// Global Styles
	@extend %module;
	@include centering(horiz);
	color: #000;
	
}
```

__List pseudo-class/elements nesting with & (separated by a blank line)__

```
.b-foo {
	
	// Media Queries
	@include respond-to(desktop) {
		padding: 10px;
	}
	
	// Global Styles
	@extend %module;
	@include centering(horiz);
	color: #000;
	
	&:hover {
		color: #fff;
	}
	
	&::after {
		content: "";
	}
	
}
```

__List nested selectors last (separated by a blank line)__

```
.b-foo {
	
	// Media Queries
	@include respond-to(desktop) {
		padding: 10px;
	}
	
	// Global Styles
	@extend %module;
	@include centering(horiz);
	color: #000;
	
	&:hover {
		color: #fff;
	}
	
	&::after {
		content: "";
	}
	
	> .bar {
		background-color: #f90;
	}
	
}
```


#### Nesting

Maximum Nesting: three levels deep!

```
.b-foo {
	.bar {
		.baz {
			// no more!
		}
	}
}
```


#### Blocks in blocks

Where to define the styles for blocks in blocks? Answer: always in your block which gets the styling. Otherwise you have to maintain more than one file which is error-prone.

Example: Assumed that you have a different styling for the user-avatar-block, based on whether it's in your header-block or in your footer-block.

```
<div class="b-header">
	<div class="b-user-avatar">
		…
	</div>
</div>

<div class="b-footer">
	<div class="b-user-avatar">
		…
	</div>
</div>
```

__bad__

```
// _header.scss
.b-header {
	.b-user-avatar {
		float: right;
		width: 100px;
		height: 100px;
	}
}

// _footer.scss
.b-footer {
	.b-user-avatar {
		float: left;
		width: 50px;
		height: 50px;
	}
}

// _user-avatar.scss
.b-user-avatar {
	border-radius: 50%;
}
```

__good__

```
// _user-avatar.scss
.b-user-avatar {
	border-radius: 50%;
	
	.b-header & {
		float: right;
		width: 100px;
		height: 100px;
	}
	
	.b-footer & {
		float: left;
		width: 50px;
		height: 50px;
	}
}
```


#### Order of elements

Selectors mirror the order of the markup.

```
<div class="b-foo">
	<ul class="bar">
		<li class="baz">
			<a class="qux" href="#">Link</a>
		</li>
	</ul>
</div>
```

__bad__

```
.b-foo {
	.qux {
		…
	}
	
	.bar {
		…
	}
}
```

__good__

```
.b-foo {
	.bar {
		…
	}
	
	.qux {
		…
	}
}
```


#### Bundling

All child selectors are bundled below the parent selector.

```
<div class="b-foo">
	<ul class="bar">
		<li class="baz">
			<a class="qux" href="#">Link</a>
		</li>
	</ul>
</div>
```

__bad__

```
.b-foo {
	.bar {
		…
	}
}

.b-foo {
	.qux {
		…
	}
}
```

__good__

```
.b-foo {
	.bar {
		…
	}
	
	.qux {
		…
	}
}
```


#### Child selectors

Each child selector will be indented and set on a new line. Important: you don't have to mirror the complete DOM!  
Rule of thumb: The selector is as short as possible. Indention only if the selector is needed.

```
<div class="b-foo">
	<ul class="bar">
		<li class="baz">
			<a class="qux" href="#">Link</a>
		</li>
	</ul>
</div>
```

__bad__

```
.b-foo {
	.baz .qux {
		…
	}
}
```

__bad too__

```
.b-foo {
	.baz {
		.qux {
			…
		}
	}
}
```

__good__

```
.b-foo {
	.qux {
		…
	}
}
```


#### Extends vs. Mixins

Sass/SCSS provide two practical ways to reuse code snippets in your CSS: Mixins and Extends. Both have their pros and cons and it needs careful consideration what to choose for each use case. The biggest caveat is that **extends can't be used inside media queries** but mixins can. Therefore extends should be used only for self contained rulesets and not snippets. 


#### Placeholder extends vs. class extends

You have two options to extend code blocks that are reused several times: standard classes and placeholders. The advantage of placeholder extends over classes: they won't be added to the CSS output and remain silent.

__Class extend__

```
// Usage
.foo {
	padding: 10px;
}

.bar {
	@extend .foo;
	color: #fff;
}

// Output
.foo,
.bar {
	padding: 10px;
}

.bar {
	color: #fff;
}
```

__Placeholder extend__

```
// Usage
%foo {
	padding: 10px;
}

.bar {
	@extend %foo;
	color: #fff;
}

// Output
.bar {
	padding: 10px;
	color: #fff;
}
```

#### Keep it simple – Part 1

Just because you can solve problems with functions, mixins etc. in SASS, you must not necessarily do it. :)  
Always remember that others should easily read and understand your code too.

__elaborate__

```
// Mixin
@mixin context($old-context, $new-context) {
	@at-root #{selector-replace(&, $old-context, $new-context)} {
		@content;
	}
}

// Usage
li {
	float: left;
	
	ul {
		display: none;
		
		@include context('li', 'li:hover') {
			display: block;
		}
	}
}
```

__simple__

```
li {
	float: left;
	
	ul {
		display: none;
	}
	
	&:hover ul {
		display: block;
	}
}
```


#### Keep it simple – Part 2

For better readability, you should write the properties as in CSS.

__elaborate__

```
.foo {
	font: {
		family: arial, sans-serif;
		size: 5em;
		weight: 700;
	}
}
```

__simple__

```
.box {
	font-family: arial, sans-serif;
	font-size: 5em;
	font-weight: 700;
}
```


## Questions?

If you're asking yourself _»Why not …?«_ have a look at my [WHY-NOT.md](https://github.com/nikita-kit/nikita-css/blob/master/WHY-NOT.md) file. There I might answer some common questions. :)


## License

nikita.css is licensed under [CC0](http://creativecommons.org/publicdomain/zero/1.0/): Public Domain Dedication.
