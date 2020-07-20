## Conteúdos

- Editar Focus (recebou foco) e Blur (perdeu) no input
- Criar estado isFocused.
- Estilizar o container baseado no focus
- Use Callback para evitar que uma função dentro da outra seja recriada toda vida
- Deixar o icone laranja caso tenha algo escrito

## src/components/Input/index.tsx

```tsx
import React, {
  InputHTMLAttributes,
  useEffect,
  useRef,
  useState,
  useCallback
} from "react";
import { IconBaseProps } from "react-icons";

import { useField } from "@unform/core";

import { Container } from "./styles";

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
    if (inputRef.current) {
      setIsFilled(!!inputRef.current.value);
    } else {
      setIsFilled(false);
    }
  }, []);

  useEffect(() => {
    registerField({
      name: fieldName,
      ref: inputRef.current,
      path: "value"
    });
  }, [registerField, fieldName]);

  return (
    <Container isFilled={isFilled} isFocused={isFocused}>
      {Icon && <Icon />}
      <input
        onFocus={handleInputFocus}
        onBlur={handleInputBlur}
        defaultValue={defaultValue}
        ref={inputRef}
        {...rest}
      />
    </Container>
  );
};

export default Input;
```

## src/components/Input/styles.ts

```css
import styled, { css } from 'styled-components';

interface ContainerProps {
  isFocused: boolean;
  isFilled: boolean;
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

```
