```sh
sudo yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
```

## .eslintrc.json

```json
{
  "env": {
    "es6": true,
    "node": true
  },
  "extends": [
    "airbnb-base",
    "plugin:@typescript-eslint/recommended",
    "prettier/@typescript-eslint",
    "plugin:prettier/recommended"
  ],
  "globals": {
    "Atomics": "readonly",
    "SharedArrayBuffer": "readonly"
  },
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint", "prettier"],
  "rules": {
    "prettier/prettier": "error",
    "import/extensions": [
      "error",
      "ignorePackages",
      {
        "ts": "never"
      }
    ]
  },
  "settings": {
    "import/resolver": {
      "typescript": {}
    }
  }
}
```

## prettier.config.js

```js
module.exports = {
  singleQuote: true,
  trailingComma: "all",
  arrowParens: "avoid"
};
```

## .eslintignore

```
/*.js
node_modules
dist

```
