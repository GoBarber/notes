```sh
sudo yarn add react-router-dom
sudo yarn add @types/react-router-dom -D
```

## src/pages/Dashboard/index.tsx

```tsx
import React from "react";

const Dashboard: React.FC = () => <h1>Dashboard</h1>;

export default Dashboard;
```

## src/pages/Repository/index.tsx

```tsx
import React from "react";

const Repository: React.FC = () => <h1>Repository</h1>;

export default Repository;
```

## src/routes/index.tsx

```tsx
import React from "react";

import { Switch, Route } from "react-router-dom";

import Dashboard from "../pages/Dashboard";
import Repository from "../pages/Repository";

const Routes: React.FC = () => (
  <Switch>
    <Route path='/' exact component={Dashboard} />
    <Route path='/repository' component={Repository} />
  </Switch>
);

export default Routes;
```

## src/App.tsx

```tsx
import React from "react";
import { BrowserRouter } from "react-router-dom";

import Routes from "./routes";

const App: React.FC = () => (
  <BrowserRouter>
    <Routes />
  </BrowserRouter>
);

export default App;
```
