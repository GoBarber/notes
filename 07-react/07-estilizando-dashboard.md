```sh
sudo yarn add polished
sudo yarn add react-icons
```

## src/pages/Dashboard/index.tsx

```tsx
import React from "react";
import { Title, Form, Repositories } from "./styles";

import { FiChevronRight } from "react-icons/fi";

import logoImg from "../../assets/image/logo.svg";

const Dashboard: React.FC = () => {
  return (
    <>
      <img src={logoImg} alt='Github Explorer' />
      <Title>Explore repositórios no github</Title>

      <Form action=''>
        <input placeholder='Digite o nome do repositório' />
        <button type='submit'>Pesquisar</button>
      </Form>

      <Repositories>
        <a href='#'>
          <img
            src='https://avatars2.githubusercontent.com/u/32900021?s=460&u=ddc654a1cd91bfe1f71e9d9a2d8455786826c98e&v=4'
            alt='Glauber Carvalho'
          />

          <div>
            <strong>rocketseat/unformed</strong>
            <p>Descrição do repositório no github</p>
          </div>

          <FiChevronRight size={20} />
        </a>

        <a href='#'>
          <img
            src='https://avatars2.githubusercontent.com/u/32900021?s=460&u=ddc654a1cd91bfe1f71e9d9a2d8455786826c98e&v=4'
            alt='Glauber Carvalho'
          />

          <div>
            <strong>rocketseat/unformed</strong>
            <p>Descrição do repositório no github</p>
          </div>

          <FiChevronRight size={20} />
        </a>

        <a href='#'>
          <img
            src='https://avatars2.githubusercontent.com/u/32900021?s=460&u=ddc654a1cd91bfe1f71e9d9a2d8455786826c98e&v=4'
            alt='Glauber Carvalho'
          />

          <div>
            <strong>rocketseat/unformed</strong>
            <p>Descrição do repositório no github</p>
          </div>

          <FiChevronRight size={20} />
        </a>
      </Repositories>
    </>
  );
};

export default Dashboard;
```

## src/pages/Dashboard/styles.ts

```css
import styled from "styled-components";
import { shade } from "polished";

export const Title = styled.h1`
  color: #3a3a3a;
  font-size: 48px;

  max-width: 450px;
  line-height: 56px;

  margin-top: 80px;
`;

export const Form = styled.form`
  margin-top: 40px;
  max-width: 700px;

  display: flex;

  input {
    flex: 1;
    height: 70px;
    padding: 0 24px;
    border: 0;
    border-radius: 5px 0 0 5px;
    color: #3a3a3a;

    &::placeholder {
      color: #a8a8a3;
    }
  }

  button {
    width: 210px;
    height: 70px;
    background: #04d361;
    border-radius: 0px 5px 5px 0px;
    border: 0;
    color: #fff;
    font-weight: bold;
    transition: background-color 0.2s;

    &:hover {
      background: ${shade(0.2, "#04d361")};
    }
  }
`;

export const Repositories = styled.div`
  margin-top: 80px;
  max-width: 700px;

  a {
    background: #fff;
    border-radius: 5px;
    width: 100%;
    padding: 24px;
    display: block;
    text-decoration: none;

    display: flex;
    align-items: center;
    transition: 0.2s;

    &:hover {
      transform: translateX(10px);
    }

    & + a {
      margin-top: 16px;
    }

    img {
      width: 64px;
      height: 64px;
      border-radius: 50%;
    }
    div {
      margin: 0 16px;
      flex: 1;

      strong {
        font-size: 20px;
        color: #3d3d4d;
      }

      p {
        font-size: 18px;
        color: #a8a8b3;
        margin-top: 4px;
      }
    }

    svg {
      margin-left: auto;
      color: #cbcbd6;
    }
  }
`;
```
