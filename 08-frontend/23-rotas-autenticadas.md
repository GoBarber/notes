## Conteúdos

- Criar página de Dashboard
- Importar a página no routes
- Criar rota privada (substituindo o Route )
- Usar o location para não perder o histórico

## src/pages/Dashboard/index.tsx

```tsx
import React from "react";
import { Switch } from "react-router-dom";

import Route from "./Route";

import SignIn from "../pages/SignIn";
import SignUp from "../pages/SignUp";
import Dashboard from "../pages/Dashboard";

const Routes: React.FC = () => (
  <Switch>
    <Route path='/' exact component={SignIn} />
    <Route path='/signup' component={SignUp} />

    <Route path='/dashboard' component={Dashboard} isPrivate />
  </Switch>
);

export default Routes;
```

## src/routes/Route.tsx

```tsx
import React from "react";
import {
  Route as ReactDOMRoute,
  RouteProps as ReactDOMRouteProps,
  Redirect,
} from "react-router-dom";

import { useAuth } from "../hooks/auth";

interface RouteProps extends ReactDOMRouteProps {
  isPrivate?: boolean;
  component: React.ComponentType;
}

const Route: React.FC<RouteProps> = ({
  isPrivate = false,
  component: Component,
  ...rest
}) => {
  const { user } = useAuth();

  return (
    <ReactDOMRoute
      {...rest}
      render={({ location }) => {
        return isPrivate === !!user ? (
          <Component />
        ) : (
          <Redirect
            to={{
              pathname: isPrivate ? "/" : "/dashboard",
              state: { from: location },
            }}
          />
        );
      }}
    />
  );
};

export default Route;
```
