# nikita.css

This is our methodology how to write efficient and scalable (S)CSS for big websites.

Latest Release: [![GitHub version](https://badge.fury.io/gh/nikita-kit%2Fnikita-css.png)](https://github.com/nikita-kit/nikita-css/releases)

If you want to start a new project from scratch, try [nikita.kickstarter](https://github.com/nikita-kit/nikita-kickstarter).  
If you're interested in HTML patterns, code snippets and best practices, try [nikita.html](https://github.com/nikita-kit/nikita-html).


## Contents

### CSS Rules

We're using some variation of BEM+SMACSS+optionatedexperienceofcssdevelopmentyears:

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
	- are not prefixed with their block (choose a longer name if it's not expressive enough)
	- __good:__ item, title, user-avatar (instead of user or avatar)
	- __bad:__ user-user-avatar, menu-item-a
- Modifier
	- are prefixed with `is-`, and have to be defined in block or element scope
	- __good:__ is-selected, is-active, is-approved
	- __bad:__ selected, active, approved


#### Example

File `_menu.scss` in `source/sass/blocks` directory.

```
.b-menu { /* block: 'b-menu' */
	&.is-static { /* modifier: 'is-static' for b-menu  */
		…
	}

	.item { /* element: 'item' in b-menu */
		a { /* element: 'item a' in b-menu */
			…
		}
	}
}
```


#### Class-Naming

Because you want to know if the block is for page layout or for a single component, we separate page layout blocks from component blocks.

Page Layout Blocks:

- b-page
- b-page-header
- b-page-nav
- b-page-main
- b-page-aside
- b-page-footer

Component Blocks:

- b-eventlist
- b-linklist
- b-sitemap
- b-teaser-text
- b-teaser-video
- …


#### Commenting

Start with a small description of the rule set, then number tiny details that are worth an explanation. The numbers are matching with the numbered comments at the end of the CSS rules, e.g. `/* 1 */`.

```
/**
 * Purpose of the selector or the rule set
 * 1. Hardware acceleration hack
 * 2. position: sticky; on anything but top aligned elements is buggy in Chrome <37 and iOS 7+
 */
 
.box {
	position: fixed;
	transform: translate3d(0, 0, 0); /* 1 */
	
	.csspositionsticky & {
		position: sticky; /* 2 */
	}
}
```


### CSS Coding Style

(This list is not intended to be exhaustive.)

- Use lowercase for class names.
- Be consistant with indentation – I'm using tabs instead of spaces.
- Be consistent in declaration order, cluster related properties (Positioning, Box-Model, Text & Color). I'm no fan of an alphabetical order.
- Be consistant with quotes – I'm using double quotes `""`.
- Quote attribute values in selectors, e.g. `input[type="checkbox"]`.
- One selector per line, one rule per line.
- Put spaces after `:` in property declarations.
- Put spaces before `{` in rule declarations.
- Put a `;` at the end of the last declaration in a declaration block.
- Include a space after each comma in comma-separated property or function values, e.g. `rgba(0, 0, 0, 0)`.
- Separate each ruleset by a blank line.
- Document styles with [KSS](https://github.com/kneath/kss).


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


#### Use hexadecimal color codes #000 unless using rgba or hsl

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
	color: #ff009;
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
<!--[if IE 8]> <html class="no-js lt-ie10 lt-ie9 ie8" lang="de"> <![endif]-->
<!--[if IE 9]> <html class="no-js lt-ie10 ie9" lang="de"> <![endif]-->
<!--[if gt IE 9]><!--> <html class="no-js" lang="de"> <!--<![endif]-->
```


### SASS structure

There are two main SCSS-files `styles.scss` and `universal.scss`.

The `styles.scss` imports all partials. `mixins`, `icons` and `blocks` will be imported with a globbing-pattern. It's important that _every block-component_ gets its own partial and is put into the `blocks`-folder! Use subfolders if your site uses lots of partials.

The `universal.scss` is a universal fallback stylesheet for older IE browsers mady by [Andy Clarke](http://code.google.com/p/universal-ie6-css/).

This is how the `sass`-folder looks like:

```
.
├── _basics.scss
├── _reset.scss
├── _webfonts.scss
├── blocks
│   ├── _page-aside.scss
│   ├── _page-footer.scss
│   ├── _page-header.scss
│   ├── _page-nav.scss
│   └── …
├── extends
│   ├── _a11y.scss
│   ├── _cf.scss
│   ├── _ellipsis.scss
│   ├── _hide-text.scss
│   ├── _ib.scss
│   ├── …
│   └── ui-components
│       ├── _buttons.scss
│       └── …
├── grunticon
├── icons
│   ├── _icons-data-png.scss
│   ├── _icons-data-svg.scss
│   └── _icons-fallback.scss
├── mixins
│   ├── _grunticon.scss
│   ├── _px-to-rem.scss
│   ├── _respond-to.scss
│   ├── _triangle.scss
│   └── …
├── styles.scss
├── universal.scss
└── variables
    ├── _breakpoints.scss
    ├── _color.scss
    ├── _typography.scss
    └── …
```

Some explanation:

- __basics.scss__ – basic styles, some normalizing
- __reset.scss__ – global browser reset by [Eric Meyer](http://meyerweb.com/eric/tools/css/reset/)
- __webfonts.scss__ – use it for `@font-face`-declarations
- __blocks/__ – all block-component-partials go in here
- __extends/__ – put your placeholder-extends in here, e.g. `a11y`, `cf`, `hide-text` etc.
- __extends/ui-components__ – put your ui-components in here, e.g. `buttons` etc.
- __grunticon/__ – output by the grunticon-task, files will be processed by the string-replace-task afterwards
- __icons/__ – output by the string-replace-task, you can use the grunticon-mixin to include the `%icons`
- __mixins/__ – put your mixins in here, e.g. `px-to-rem`, `respond-to` etc.
- __styles.scss__ – main stylesheet, includes all partials
- __universal.scss__ – stylesheet for old browsers, based on [universal-ie6-css](https://code.google.com/p/universal-ie6-css/)
- __variables/__ – put your variables in here, e.g. `color`, `typography` etc.


### SASS Coding Guidelines

(This list is not intended to be exhaustive.)

I'm using SCSS-syntax because it's valid CSS and more expressive in my eyes.

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


__List pseudo-class/element nesting with & (separated by a blank line)__

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
	
	&:after {
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
	
	&:after {
		content: "";
	}
	
	> .bar {
		background-color: #f30;
	}
	
}
```

__Maximum Nesting: three levels deep__

```
.b-foo {
	.bar {
		.baz {
			// no more!
		}
	}
}
```


### SASS Extends

We provide some useful [Extends](https://github.com/nikita-kit/nikita-css/tree/master/extends) which can easily be copied/pasted into your project.


### SASS Mixins

Same with [Mixins](https://github.com/nikita-kit/nikita-css/tree/master/mixins). Help yourselves!


## Questions?

If you're asking yourself _»Why not …?«_ have a look at my [WHY-NOT.md](https://github.com/nikita-kit/nikita-css/blob/master/WHY-NOT.md) file. There I might answer some common questions. :)


## License

nikita.css is licensed under [CC0](http://creativecommons.org/publicdomain/zero/1.0/): Public Domain Dedication.
