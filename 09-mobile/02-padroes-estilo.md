## Conteúdos

- instalando eslint e prettier
- configure editor config
- configure eslint
- configure esling ignore
- configure prettier

```
sudo yarn add eslint -D
sudo yarn eslint --init
sudo yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
sudo yarn add eslint-import-resolver-typescript -D
```

## .editorconfig

```
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
```

## .eslintrc.json

```json
{
  "env": {
    "es6": true
  },
  "extends": [
    "plugin:react/recommended",
    "airbnb",
    "plugin:@typescript-eslint/recommended",
    "prettier/@typescript-eslint",
    "plugin:prettier/recommended"
  ],
  "globals": {
    "Atomics": "readonly",
    "SharedArrayBuffer": "readonly"
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 11,
    "sourceType": "module"
  },
  "plugins": ["react", "react-hooks", "@typescript-eslint", "prettier"],
  "rules": {
    "prettier/prettier": "error",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/jsx-filename-extension": [1, { "extensions": [".tsx"] }],
    "import/prefer-default-export": "off",
    "react/jsx-props-no-spreading": "off",
    "no-unused-expressions": "off",
    "react/prop-types": "off",
    "import/extensions": [
      "error",
      "ignorePackages",
      {
        "ts": "never",
        "tsx": "never"
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

## .eslintignore

```
**/*.js
node_modules
build

```

## prettier.config.js

```js
module.exports = {
  singleQuote: true,
  trailingComma: "all",
  allowParens: "avoid",
};
```
