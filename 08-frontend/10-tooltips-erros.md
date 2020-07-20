## Conteúdos

- add borda vermelha para o erro
  - passar isErrored para o container do input
  - tratar no styles
- Substituir a mensagem de erro por um icone
- Criar tooltip para exibir a mensagem num balão
- Importar o tooltip no input no styles (importante: Toda vez que fizer herança no styled componentes é importante está atento no classname)
- Configurar arrow do balão e transição com hover
- Extra: Responsividade

## src/components/Input/index.tsx

```tsx
import React, {
  InputHTMLAttributes,
  useEffect,
  useRef,
  useState,
  useCallback,
} from "react";
import { IconBaseProps } from "react-icons";
import { FiAlertCircle } from "react-icons/fi";

import { useField } from "@unform/core";

import { Container, Error } from "./styles";

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  name: string;
  icon: React.ComponentType<IconBaseProps>;
}

const Input: React.FC<InputProps> = ({ name, icon: Icon, ...rest }) => {
  const inputRef = useRef<HTMLInputElement>(null);

  const [isFocused, setIsFocused] = useState(false);
  const [isFilled, setIsFilled] = useState(false);

  const { fieldName, defaultValue, error, registerField } = useField(name);

  const handleInputFocus = useCallback(() => {
    setIsFocused(true);
  }, []);

  const handleInputBlur = useCallback(() => {
    setIsFocused(false);

    setIsFilled(!!inputRef.current?.value);
  }, []);

  useEffect(() => {
    registerField({
      name: fieldName,
      ref: inputRef.current,
      path: "value",
    });
  }, [registerField, fieldName]);

  return (
    <Container isErrored={!!error} isFilled={isFilled} isFocused={isFocused}>
      {Icon && <Icon />}
      <input
        onFocus={handleInputFocus}
        onBlur={handleInputBlur}
        defaultValue={defaultValue}
        ref={inputRef}
        {...rest}
      />
      {error && (
        <Error title={error}>
          <FiAlertCircle color='#c53030' size={20} />
        </Error>
      )}
    </Container>
  );
};

export default Input;
```

## src/components/Input/styles.ts

```css
import styled, { css } from 'styled-components';

import Tooltip from '../Tooltip';

interface ContainerProps {
  isFocused: boolean;
  isFilled: boolean;
  isErrored: boolean;
}

export const Container = styled.div<ContainerProps>`
  background: #232129;
  border-radius: 10px;
  border: 2px solid #232129;
  padding: 16px;
  width: 100%;
  color: #666360;
  display: flex;
  align-items: center;

  & + div {
    margin-top: 8px;
  }
  ${(props) =>
    props.isErrored &&
    css`
      border-color: #c53030;
    `}

  ${(props) =>
    props.isFocused &&
    css`
      color: #ff9000;
      border-color: #ff9000;
    `}

  ${(props) =>
    props.isFilled &&
    css`
      color: #ff9000;
    `}



  input {
    color: #f4ede8;
    flex: 1;
    border: 0;
    background: transparent;
    &::placeholder {
      color: #666360;
    }

    &:-webkit-autofill {
      /* Ajeitar o Autocomplete no chrome  */

      font: inherit;
      -webkit-box-shadow: 0 0 0 30px #232129 inset;
      -webkit-text-fill-color: #f4ede8 !important;
      font: 16px 'Roboto Slab', serif;
    }
  }

  svg {
    margin-right: 16px;
  }
`;

export const Error = styled(Tooltip)`
  height: 20px;
  margin-left: 16px;
  svg {
    margin: 0;
  }

  span {
    background: #c53030;
    color: #fff;

    &::before {
      border-color: #c53030 transparent;
    }
  }
`;

```

## src/components/Tooltip/index.tsx

```tsx
import React from "react";

import { Container } from "./styles";

interface TooltipProps {
  title: string;
  className?: string;
}

const Tooltip: React.FC<TooltipProps> = ({ title, children, className }) => {
  return (
    <Container className={className}>
      {children}
      <span>{title}</span>
    </Container>
  );
};
export default Tooltip;
```

## src/components/Tooltip/styles.ts

```css
import styled from 'styled-components';

export const Container = styled.div`
  position: relative;

  span {
    width: 200px;
    background: #ff9000;
    padding: 8px;
    border-radius: 4px;
    font-size: 12px;
    font-weight: 500;

    position: absolute;
    bottom: calc(100% + 12px);
    left: 50%;
    transform: translateX(-50%);
    opacity: 0;
    transition: opacity 0.4s;
    visibility: hidden;

    color: #312e38;

    @media only screen and (max-width: 600px) {
      transform: translateX(-85%);
    }

    &::before {
      content: '';
      border-style: solid;
      border-color: #ff9000 transparent;
      border-width: 6px 6px 0 6px;
      top: 100%;
      position: absolute;
      left: 50%;
      transform: translateX(-50%);

      @media only screen and (max-width: 600px) {
        left: 87%;
        transform: translateX(-85%);
      }
    }
  }
  &:hover span {
    opacity: 1;
    visibility: visible;
  }
`;

```
