## Conteúdos

- Criação da aplicação
- Deletando arquivos desnecessários
- Configurando o editorconfig
- Configurando o eslint com o prettier
- Editando o public/index.html

```sh
  sudo create-react-app app --template=typescript
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

```sh
sudo yarn add eslint -D
```

## Remover do package.json o eslintConfig

```sh
sudo yarn eslint --init
```

```sh
sudo yarn add eslint-import-resolver-typescript prettier eslint-config-prettier eslint-plugin-prettier -D
```

## .editorconfig

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
  allowParens: "avoid"
};
```

## .eslintrc.json

```json
{
  "env": {
    "browser": true,
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

## public/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#FF9000" />
    <title>GoBarber</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```
