```sh
sudo yarn add axios
```

## src/services/api.ts

```tsx
import axios from "axios";

const api = axios.create({
  baseURL: "https://api.github.com"
});

export default api;
```

## src/pages/Dashboard/index.tsx

```tsx
import React, { useState, FormEvent } from "react";
import { Title, Form, Repositories } from "./styles";
import api from "../../services/api";

import { FiChevronRight } from "react-icons/fi";

import logoImg from "../../assets/image/logo.svg";

interface Repository {
  full_name: string;
  description: string;
  owner: {
    login: string;
    avatar_url: string;
  };
}

const Dashboard: React.FC = () => {
  const [newRepo, setNewRepo] = useState("");
  const [repositories, setRepositories] = useState<Repository[]>([]);

  async function handleAddRepository(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();

    const response = await api.get<Repository>(`/repos/${newRepo}`);

    const repository = response.data;
    setNewRepo("");

    setRepositories([...repositories, repository]);
  }

  return (
    <>
      <img src={logoImg} alt='Github Explorer' />
      <Title>Explore repositórios no github</Title>

      <Form onSubmit={handleAddRepository}>
        <input
          placeholder='Digite o nome do repositório'
          value={newRepo}
          onChange={e => setNewRepo(e.target.value)}
        />
        <button type='submit'>Pesquisar</button>
      </Form>

      <Repositories>
        {repositories.map(repository => (
          <a href='#' key={repository.full_name}>
            <img src={repository.owner.avatar_url} alt={repository.full_name} />

            <div>
              <strong>{repository.full_name}</strong>
              <p>{repository.description}</p>
            </div>

            <FiChevronRight size={20} />
          </a>
        ))}
      </Repositories>
    </>
  );
};

export default Dashboard;
```
