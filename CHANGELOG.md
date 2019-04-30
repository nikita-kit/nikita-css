# dev

* removed distinction between page-blocks and blocks to reduce confusion
* updated guidance about mixins vs. extends
* updated guidance about comments
* updated guidance about element naming convention
* updated all dependencies
* added some new rules:
  * `value-no-vendor-prefix": true`
  * `property-no-vendor-prefix": true`
  * `at-rule-no-vendor-prefix": true`
  * `declaration-no-important": true`
  * `color-named": never`
  * `font-weight-notation": numeric`
  * `max-nesting-depth": 3`
  * `no-duplicate-at-import-rules": true`
  * `no-duplicate-selectors": true`
  * `number-max-precision": 3`
  * `selector-no-qualifying-type": true`
  * `no-empty-first-line": true`
  * `declaration-block-semicolon-newline-before": never-multi-line`
  * `linebreaks": unix`
  * `max-line-length": 160`

# 2.0.0 (2019/02/06)

* removed mixins and extends, these are now part of nikita-generator
* added rem advice to styleguide
* changed some stylelint rules
* updated `stylelint` to 9.9.0
* updated `stylelint-order` to 2.0.0
* replaced `stylelint-find-rules` with `stylelint-find-new-rules`

# 1.0.0 (2017/10/03)

* added stylelint-config-nikita package
* added .gitignore file and removed bower.json
* linted scss files with stylelint
* updated a11y extend to a get rid of the performace intensive positioning if you use it a lot
* updated respond-to mixin to make fallback optional
* added .travis.yml

# 0.11.0

* [BC] centering-mixin: substitute "vert" and "horiz"

# 0.10.1

* added _ib.scss description

# 0.10.0

* added _ib.scss

# 0.9.0

* added bower.json
* fixed typo in footer example
