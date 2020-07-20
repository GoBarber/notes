```sh
sudo yarn add eslint -D
```

## package.json
- REMOVER ESSA LINHA 
```json
{
    "eslintConfig": {
      "extends": "react-app"
    },
}
```

```sh
sudo yarn eslint --init
sudo yarn add eslint-import-resolver-typescript -D
```

## .eslintignore
```
**/*.js
node_modules
build

```


## .eslintrc.js
```js
module.exports = {
  env: {
    browser: true,
    es6: true
  },
  extends: [
    "plugin:react/recommended",
    "airbnb",
    "plugin:@typescript-eslint/recommended"
  ],
  globals: {
    Atomics: "readonly",
    SharedArrayBuffer: "readonly"
  },
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 2018,
    sourceType: "module"
  },
  plugins: ["react", "react-hooks", "@typescript-eslint"],
  rules: {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "react/jsx-filename-extension": [1, { extensions: [".tsx"] }],
    "import/prefer-default-export": "off",
    "import/extensions": [
      "error",
      "ignorePackages",
      {
        ts: "never",
        tsx: "never"
      }
    ]
  },
  settings: {
    "import/resolver": {
      typescript: ""
    }
  }
};

```
