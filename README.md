# nikita.css

If you want to start a new project from scratch, try [nikita.kickstarter](https://github.com/nikita-kit/nikita-kickstarter).

Latest Release: [![GitHub version](https://badge.fury.io/gh/nikita-kit%2Fnikita-css.png)](https://github.com/nikita-kit/nikita-css/releases)


## HTML Rules

__HTML5__ is preferred for all HTML documents, so I'm using:

__HTML5 Elements__

sectioning: `<header>, <footer>, <nav>, <aside>, <article>, <section>` …  
grouping: `<main>, <figure>, <figcaption>` …  
text-level semantic: `<mark>, <time>` …  
multimedia: `<video>, <audio>` …

__HTML5 Forms__

types: `date, email, number, range, search, tel, url` …  
elements: `datalist, progress, output` …  
attributes: `pattern, placeholder, required` …  

To achieve a constant form-behaviour across all A-graded browsers, I can recommend [Webshims Lib](https://github.com/aFarkas/webshim), a modular capability-based polyfill-loading library.


## HTML Coding Style

(This list is not intended to be exhaustive.)

- __Document Type:__ Use the HTML5 doctype `<!DOCTYPE html>`.
- __Encoding:__: Use UTF-8 `<meta charset="utf-8">`
- __Validity:__ The HTML-code should be valid where possible. You can test it with the [W3C HTML validator](http://validator.w3.org/nu/).
- __Semantics:__ Use HTML according to its purpose. For example, use heading elements `h1–h6` for headings, `p` elements for paragraphs, `a` elements for anchors etc. Tables shouldn't be used for page layout. Also try to avoid DIVitis.
- __Separation of Concerns:__ Separate structure from presentation from behavior.
- __Type Attributes:__ Omit type attributes for style sheets and scripts.
- __General Formatting:__ Use a new line for every block, list, or table element and indent every such child element. I'm using tabs instead of spaces.
- __Quoting:__ When quoting attributes values, use double quotation marks `" "`.
- __Label/Input:__ Every form input should utilize a `label` with a `for`-attribute..
- __Tables:__ Make use of `<thead>, <tfoot>, <tbody>, <th>`.
- __Human readable:__ Code is written and maintained by people. Ensure your code is descriptive, well commented, and approachable by others!


## CSS Rules

I'm using some variation of BEM+SMACSS+optionatedexperienceofcssdevelopmentyears:

My site exists of basic __blocks__. Blocks are independent parts of the site (e.g. menu, metanav, login form, sidebar, user detail page). Like explained at [yandex's BEM](http://img-fotki.yandex.ru/get/5008/221798411.0/0_babce_7deef28f_XXL.png).
The blocks may contain other blocks.

The smallest entities of a block are called __elements__. For instance the block 'menu' contains multiple items, a login block may contain a username element, password element and a submit button element. Like explained at [yandex's BEM](http://img-fotki.yandex.ru/get/6726/221798411.0/0_babd1_f14000fa_XL.png).

Blocks and elements may be modified with __modifiers__. For instance the selected menu item is a modified version of the menu item.

- Blocks
  - are prefixed with _b-_
  - __good:__ b-menu, b-sidebar, b-sitemap, b-user
  - __bad:__ menu, sidebar, sitemap, user
- Elements
  - have _no prefix_ and can only be defined in block scope
  - are not prefixed with their block (choose a longer name if it's not expressive enough)
  - __good:__ item, title, user-avatar (instead of user or avatar)
  - __bad:__ user-user-avatar, menu-item-a
- Modifier
  - are prefixed with _is-_, and have to be defined in block or element scope
  - __good:__ is-selected, is-active, is-approved
  - __bad:__ selected, active, approved


### Example

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


### Class-Naming

Because you want to know if the block is for page layout or for a single component, I separate page layout blocks from component blocks.

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


## SASS structure

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


## CSS Coding Style

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


## CSS Coding Guidelines

(This list is not intended to be exhaustive.)

- Avoid element selectors.
  - __bad:__ .foo div, .foo span, .foo ul
  - ___good:___ .foo .section, .foo .title, .foo .linklist
- Avoid IDs where possible (exeption: e.g. in forms -> for-attribute).
  - __bad:__ #sidebar
  - __good:__ .sidebar
- Avoid qualifying class names with type selectors.
  - __bad:__ ul.linklist, div.example, a.back
  - __good:__ .linklist, .example, .back
- Avoid the descendant selector. Target directly if possible.
  - __bad:__ .foo .bar .baz
  - __good:__ .baz-header
- Use shorthand properties where possible.
  - __bad:__ padding-top: 0; padding-right: 1em; padding-bottom: 2em; padding-left: 1em;
  - __good:__ padding: 0 1em 2em;
- Omit unit specification after “0” values.
  - __bad:__ margin: 0px;
  - __good:__ margin: 0;
- Use hexadecimal color codes #000 unless using rgba.
  - __bad:__ color: orange;
  - __good:__ color: #ffa500;
- Use 3 character hexadecimal notation where possible.
  - __bad:__ color: #ff0099;
  - __good:__ color: #f09;
- Use number keywords (100–900) for font-weight.
  - __bad:__ font-weight: normal;
  - __good:__ font-weight: 400;
- Separate words in class names by a hyphen.
  - __bad:__ .user_avatar, .userAvatar, .useravatar
  - __good:__ .user-avatar
- Dont't use !important, it's ok to use it on helper classes though.
- Dont't use conditional stylesheets, use the html-class (e.g. .lt-ie9) instead to style directly in your block.


## SASS Coding Guidelines

(This list is not intended to be exhaustive.)

I'm using SCSS-syntax because it's valid CSS and more expressive in my eyes.

__List @extend first__

```
.b-foo {
	@extend %module;
	…
}
```

__List regular styles next__

```
.b-foo {
	@extend %module;
	margin: 0 auto;
	…
}
```

__List @include depending on the property you are referencing__

```
.b-foo {
	@extend %module;
	margin: 0 auto;
	@include rem(padding, 10px);
	color: #000;
}
```

__List nested selectors last__

```
.b-foo {
	@extend %module;
	margin: 0 auto;
	@include rem(padding, 10px);
	color: #000;

	> .bar {
		padding: 0 10px 5px;
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


## Questions?

If you're asking yourself _»Why not …?«_ have a look at my [WHY-NOT.md](https://github.com/nikita-kit/nikita-css/blob/master/WHY-NOT.md) file. There I might answer some common questions. :)


## License

nikita.css is licensed under [CC0](http://creativecommons.org/publicdomain/zero/1.0/): Public Domain Dedication.