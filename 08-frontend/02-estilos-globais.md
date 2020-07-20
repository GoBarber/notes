## Conteúdos

- importar fontes no index.html
- Configurando estilos globais
  - Criar o arquivo global.ts
  - importar em app.tsx

```sh
sudo yarn add styled-components
sudo yarn add @types/styled-components -D
```

## public/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="theme-color" content="#FF9000" />
    <title>GoBarber</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@400;500&display=swap"
      rel="stylesheet"
    />
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

## src/styles/global.ts

```css
import { createGlobalStyle } from 'styled-components';

export default createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    outline: 0;
    box-sizing: border-box;
  }

  body {
    background: #312e38;
    color: #fff;
    -webkit-font-smoothing: antialiased;
  }

  body, input, button {
    font-family: "Roboto Slab", serif;
    font-size: 16px;
  }

  h1, h2, h3, h4, h5, h6, strong {
    font-weight: 500;
  }

  button {
    cursor: pointer;
  }
`;

```

## src/App.tsx

```tsx
import React from "react";
import GlobalStyle from "./styles/global";

const App: React.FC = () => {
  return (
    <>
      <h1>Hello World</h1>
      <GlobalStyle />
    </>
  );
};
export default App;
```
