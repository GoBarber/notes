```sh
sudo yarn add styled-components
sudo yarn add @types/styled-components -D

```

## src/pages/Dashboard/index.tsx

```ts
import React from "react";
import { Title } from "./styles";

const Dashboard: React.FC = () => <Title>Explore repositórios no github</Title>;

export default Dashboard;
```

## src/pages/Dashboard/styles.ts

```ts
import styled from "styled-components";

export const Title = styled.h1`
  color: #3a3a3a;
  font-size: 48px;
`;
```

## public/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#3A3A3A" />
    <title>Github Explorer</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,700;1,400&display=swap"
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

import githubBackground from '../assets/image/github-background.svg';

export default createGlobalStyle`

* {
  margin: 0;
  padding: 0;
  outline: 0;
  box-sizing: border-box;
}

body {
  background: #F0F0F5 url(${githubBackground}) no-repeat 70% top;
  -webkit-font-smoothing: antialiased;
}

body, input, button {
  font: 16px Roboto, sans-serif;
}

#root {
  max-width: 960px;
  margin: 0 auto;
  padding: 40px 20px;
}

button {
  cursor: pointer;
}
`;

```

## src/App.tsx

```ts
import React from "react";
import { BrowserRouter } from "react-router-dom";

import GlobalStyle from "./styles/global";

import Routes from "./routes";

const App: React.FC = () => (
  <>
    <BrowserRouter>
      <Routes />
    </BrowserRouter>
    <GlobalStyle />
  </>
);

export default App;
```
