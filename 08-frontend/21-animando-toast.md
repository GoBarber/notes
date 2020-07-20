## Conteúdos

- Instalar o React Spring para fazer animações
- Usando o React spring no ToastContainer
  - Criar a animação
  - Passar os parametros para o Toast
- Receber propriedades na toast
  - passar o style pro container
  - importar o animated no styles e usar no container

```sh
sudo yarn add react-spring
```

## src/components/ToastContainer/index.tsx

```tsx
import React from "react";
import { useTransition } from "react-spring";

import { ToastMessage } from "../../hooks/toast";

import { Container } from "./styles";

import Toast from "./Toast";

interface ToastContainerProps {
  messages: ToastMessage[];
}

const ToastContainer: React.FC<ToastContainerProps> = ({ messages }) => {
  const messagesWithTransitions = useTransition(
    messages,
    (message) => message.id,
    {
      from: { right: "-120%", opacity: 0 },
      enter: { right: "0%", opacity: 1 },
      leave: { right: "-120%", opacity: 0 },
    }
  );
  return (
    <Container>
      {messagesWithTransitions.map(({ item, key, props }) => (
        <Toast key={key} message={item} style={props} />
      ))}
    </Container>
  );
};

export default ToastContainer;
```

## src/components/ToastContainer/Toast/index.tsx

```tsx
import React, { useCallback, useEffect } from "react";

import {
  FiAlertCircle,
  FiXCircle,
  FiCheckCircle,
  FiInfo,
} from "react-icons/fi";

import { Container } from "./styles";

import { ToastMessage, useToast } from "../../../hooks/toast";

interface ToastProps {
  message: ToastMessage;
  style: object;
}

const icons = {
  info: <FiInfo size={24} />,
  error: <FiAlertCircle size={24} />,
  success: <FiCheckCircle size={24} />,
};

const Toast: React.FC<ToastProps> = ({ message, style }) => {
  const { removeToast } = useToast();

  const handleRemoveToast = useCallback(
    (id: string) => {
      removeToast(id);
    },
    [removeToast]
  );

  useEffect(() => {
    const timer = setTimeout(() => {
      handleRemoveToast(message.id);
    }, 3000);

    return (): void => {
      clearTimeout(timer);
    };
  }, [handleRemoveToast, message.id]);

  return (
    <Container
      type={message.type}
      hasDescription={!!message.description}
      style={style}
    >
      {icons[message.type || "info"]}
      <div>
        <strong>{message.title}</strong>
        {message.description && <p>{message.description}</p>}
      </div>

      <button onClick={(): void => handleRemoveToast(message.id)} type='button'>
        <FiXCircle size={18} />
      </button>
    </Container>
  );
};

export default Toast;
```

## src/components/ToastContainer/Toast/styles.ts

```css
import styled, { css } from 'styled-components';
import { animated } from 'react-spring';

const toastTypeVariations = {
  info: css`
    background: #ebf8ff;
    color: #3172b7;
  `,
  success: css`
    background: #e6fffa;
    color: #2e656a;
  `,
  error: css`
    background: #fddede;
    color: #c53030;
  `,
};

interface ContainerProps {
  type?: 'success' | 'error' | 'info';
  hasDescription: boolean;
}

export const Container = styled(animated.div)<ContainerProps>`
  max-width: 360px;
  width: 100%;
  position: relative;
  padding: 16px 30px 16px 16px;
  border-radius: 10px;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.2);

  display: flex;

  & + div {
    margin-top: 8px;
  }

  ${(props) => toastTypeVariations[props.type || 'info']}

  > svg {
    margin: 4px 12px 0 0;
  }

  div {
    flex: 1;
    p {
      margin-top: 4px;
      font-size: 14px;
      opacity: 0.8;
      line-height: 20px;
    }
  }

  button {
    position: absolute;
    right: 16px;
    top: 19px;
    opacity: 0.6;
    border: 0;
    background: transparent;
    color: inherit;
  }

  ${(props) =>
    !props.hasDescription &&
    css`
      algin-items: center;

      svg {
        margin-top: 0;
      }
    `}
`;

```
