## Conteúdos

- Iniciando o app com npm
- Colocando o local do sdk
- colocar o index dentro do src
- remover o eslint e prettier
-

```
sudo npx react-native init appgobarber --template react-native-template-typescript
```

## android/local.properties

```
sdk.dir = /home/glauberc/Android/Sdk
```

## src/index.tsx

```tsx
import React from "react";
import { View, Text } from "react-native";

const App: React.FC = () => (
  <View>
    <Text>Hello darkness my old friend</Text>
  </View>
);

export default App;
```

## index.js

```js
/**
 * @format
 */

import { AppRegistry } from "react-native";
import App from "./src";
import { name as appName } from "./app.json";

AppRegistry.registerComponent(appName, () => App);
```
