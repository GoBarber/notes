## Conteúdos

- configurando status bar

## src/index.tsx

```tsx
import React from "react";
import { View, Text, StatusBar } from "react-native";

const App: React.FC = () => (
  <>
    <StatusBar barStyle='light-content' backgroundColor='#321e38' />
    <View style={{ flex: 1, backgroundColor: "#321e38" }}>
      <Text>Hello darkness my old friend</Text>
    </View>
  </>
);

export default App;
```
