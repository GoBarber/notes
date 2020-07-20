## Conteúdos

- Criar pagina de SignIn e SignUp
- Instalar o react navigation (https://reactnavigation.org/docs/getting-started)
- Instalar o stack navigation
- Configurar routes

```
sudo yarn add styled-components
sudo yarn add @types/styled-components -D
sudo yarn add @react-navigation/native
sudo yarn add react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
sudo yarn add @react-navigation/stack
```

## src/pages/SignIn/index.tsx

```tsx
import React from "react";
import { Text } from "react-native";

import { Container } from "./styles";

const SignIn: React.FC = () => {
  return (
    <Container>
      <Text>SignIn</Text>
    </Container>
  );
};

export default SignIn;
```

## src/pages/SignIn/styles.ts

```css
import styled from 'styled-components/native';

export const Container = styled.View``;

```

## src/pages/SignUp/index.tsx

```tsx
import React from "react";
import { Text } from "react-native";

import { Container } from "./styles";

const SignUp: React.FC = () => {
  return (
    <Container>
      <Text>SignUp</Text>
    </Container>
  );
};

export default SignUp;
```

## src/pages/SignUp/styles.tsx

```css
import styled from 'styled-components/native';

export const Container = styled.View``;

```

## src/routes/index.tsx

```tsx
import React from "react";
import { createStackNavigator } from "@react-navigation/stack";

import SignIn from "../pages/SignIn";
import SignUp from "../pages/SignUp";

const Auth = createStackNavigator();

const AuthRoutes: React.FC = () => (
  <Auth.Navigator
    screenOptions={{
      headerShown: false,
      cardStyle: { backgroundColor: "#321e38" },
    }}
  >
    <Auth.Screen name='SignIn' component={SignIn} />
    <Auth.Screen name='SignUp' component={SignUp} />
  </Auth.Navigator>
);

export default AuthRoutes;
```

## src/index.tsx

```tsx
import "react-native-gesture-handler";
import React from "react";
import { View, StatusBar } from "react-native";
import { NavigationContainer } from "@react-navigation/native";

import AuthRoutes from "./routes";

const App: React.FC = () => (
  <NavigationContainer>
    <StatusBar barStyle='light-content' backgroundColor='#321e38' />
    <View style={{ flex: 1, backgroundColor: "#321e38" }}>
      <AuthRoutes />
    </View>
  </NavigationContainer>
);

export default App;
```
