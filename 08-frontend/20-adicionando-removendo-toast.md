## Conteúdos

- Colocando o estado pra armazenar as mensagens no toast hook
- Função addToast
  - Utilizando id único para os toasts com o uuidv4
  - Passando as messages para o ToastContainer
  - Map no ToastContainer recebendo as props certas
  - Enviando do Toaster as props no SignIn
- Função removeToast
  - Importar o removeToast no toast
- Auto fechamento do toast
  - Separar o Toast do Toast container
  - Usar um useEffect no toast para aplicar uma ação quando ele for criado
  - Criando e deletando o timer
- Importando outros icones

```sh
sudo yarn add uuidv4
```

## src/hooks/toast.tsx

```tsx
import React, { createContext, useCallback, useContext, useState } from "react";
import { uuid } from "uuidv4";

import ToastContainer from "../components/ToastContainer";

export interface ToastMessage {
  id: string;
  type?: "success" | "error" | "info";
  title: string;
  description?: string;
}

interface ToastContextData {
  addToast(message: Omit<ToastMessage, "id">): void;
  removeToast(id: string): void;
}

const ToastContext = createContext<ToastContextData>({} as ToastContextData);

export const ToastProvider: React.FC = ({ children }) => {
  const [messages, setMessages] = useState<ToastMessage[]>([]);

  const addToast = useCallback(
    ({ type, title, description }: Omit<ToastMessage, "id">) => {
      const id = uuid();
      const toast = {
        id,
        type,
        title,
        description,
      };

      setMessages((oldMessages) => [...oldMessages, toast]);
    },
    []
  );
  const removeToast = useCallback((id: string) => {
    setMessages((state) => state.filter((message) => message.id !== id));
  }, []);

  return (
    <ToastContext.Provider value={{ addToast, removeToast }}>
      {children}
      <ToastContainer messages={messages} />
    </ToastContext.Provider>
  );
};

export function useToast(): ToastContextData {
  const context = useContext(ToastContext);

  if (!context) {
    throw new Error("useToast must be used within a ToastProvider");
  }

  return context;
}
```

## src/components/ToastContainer/index.tsx

```tsx
import React from "react";

import { ToastMessage } from "../../hooks/toast";

import { Container } from "./styles";

import Toast from "./Toast";

interface ToastContainerProps {
  messages: ToastMessage[];
}

const ToastContainer: React.FC<ToastContainerProps> = ({ messages }) => {
  return (
    <Container>
      {messages.map((message) => (
        <Toast key={message.id} message={message} />
      ))}
    </Container>
  );
};

export default ToastContainer;
```

## src/components/ToastContainer/styles.ts

```css
import styled from 'styled-components';

export const Container = styled.div`
  position: absolute;
  right: 0;
  top: 0;
  padding: 30px;
  overflow: hidden;

  @media only screen and (max-width: 600px) {
    padding: 30px 10px 10px 0;
  }
`;

```

## src/pages/SignIn/index.tsx

```tsx
import React, { useCallback, useRef, useContext } from "react";
import { FiLogIn, FiMail, FiLock } from "react-icons/fi";

import { FormHandles } from "@unform/core";
import { Form } from "@unform/web";

import * as Yup from "yup";

import getValidationErros from "../../utils/getValidationErros";

import { useAuth } from "../../hooks/auth";
import { useToast } from "../../hooks/toast";

import logoImg from "../../assets/logo.svg";

import Input from "../../components/Input";
import Button from "../../components/Button";

import { Container, Content, Background } from "./styles";

interface SignInFormData {
  email: string;
  password: string;
}

const SignIn: React.FC = () => {
  const formRef = useRef<FormHandles>(null);

  const { signIn } = useAuth();
  const { addToast } = useToast();

  const handleSubmit = useCallback(
    async (data: SignInFormData) => {
      formRef.current?.setErrors({});
      try {
        const schema = Yup.object().shape({
          email: Yup.string()
            .required("Email obrigatório")
            .email("Digite um e-mail válido"),
          password: Yup.string().required("Senha obrigatória"),
        });

        await schema.validate(data, {
          abortEarly: false,
        });

        await signIn({
          email: data.email,
          password: data.password,
        });
      } catch (err) {
        if (err instanceof Yup.ValidationError) {
          const errors = getValidationErros(err);
          formRef.current?.setErrors(errors);
        } else {
          addToast({
            type: "error",
            title: "Erro na autenticação",
            description:
              "Ocorreu um erro ao fazer login, cheque as credenciais",
          });
        }
      }
    },
    [signIn, addToast]
  );

  return (
    <Container>
      <Content>
        <img src={logoImg} alt='GoBarber' />

        <Form ref={formRef} onSubmit={handleSubmit}>
          <h1>Faça seu logon</h1>

          <Input name='email' icon={FiMail} placeholder='E-mail' />
          <Input
            name='password'
            icon={FiLock}
            type='password'
            placeholder='Senha'
          />

          <Button type='submit'>Entrar</Button>

          <a href='forgot'>Esqueci minha senha</a>
        </Form>

        <a href='login'>
          <FiLogIn />
          Criar conta
        </a>
      </Content>
      <Background />
    </Container>
  );
};

export default SignIn;
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
}

const icons = {
  info: <FiInfo size={24} />,
  error: <FiAlertCircle size={24} />,
  success: <FiCheckCircle size={24} />,
};

const Toast: React.FC<ToastProps> = ({ message }) => {
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
    <Container type={message.type} hasDescription={!!message.description}>
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

export const Container = styled.div<ContainerProps>`
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
