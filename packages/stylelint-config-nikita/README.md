# stylelint-config-nikita

This package provides nikitas stylelint-config as an extensible shared config.

Our default export contains all of our stylelint rules. It requires `stylelint` and `stylelint-order`.

## Usage


1. Install the correct versions of each package, which are listed by the command:

  ```sh
  npm info "stylelint-config-nikita@latest" peerDependencies
  ```

  Run command like that:

  ```sh
    npm install --save-dev stylelint-config-nikita stylelint@^#.#.# stylelint-order@^#.#.#
  ```

2. Add `"extends": "nikita"` to your stylelint-config or reference index.js as config in your Grunt stylelint task.


## Version

The version of this package equals the nikita-css version.


## more

You can make sure this module lints with itself using `npm run lint`.

See [Nikita (S)CSS Styleguide](https://github.com/nikita-kit/nikita-css) for more information.
