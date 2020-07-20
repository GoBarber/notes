## src/pages/Dashboard/index.tsx

```tsx
import React, { useState, useEffect, FormEvent } from "react";
import { Title, Form, Repositories, Error } from "./styles";
import { Link } from "react-router-dom";
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
  const [inputError, setInputError] = useState("");
  const [repositories, setRepositories] = useState<Repository[]>(() => {
    const storagedRepositories = localStorage.getItem(
      "@GithubExplorer:repositories"
    );

    return storagedRepositories ? JSON.parse(storagedRepositories) : [];
  });

  useEffect(() => {
    localStorage.setItem(
      "@GithubExplorer:repositories",
      JSON.stringify(repositories)
    );
  }, [repositories]);

  async function handleAddRepository(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();

    if (!newRepo) {
      setInputError("Digite o autor/nome do repositório");
      return;
    }

    try {
      const response = await api.get<Repository>(`/repos/${newRepo}`);

      const repository = response.data;

      setRepositories([...repositories, repository]);
      setNewRepo("");
      setInputError("");
    } catch {
      setInputError("Erro na busca por esse repositório");
    }
  }

  return (
    <>
      <img src={logoImg} alt='Github Explorer' />
      <Title>Explore repositórios no github</Title>

      <Form onSubmit={handleAddRepository} hasError={!!inputError}>
        <input
          placeholder='Digite o nome do repositório'
          value={newRepo}
          onChange={e => setNewRepo(e.target.value)}
        />
        <button type='submit'>Pesquisar</button>
      </Form>

      {inputError && <Error>{inputError}</Error>}

      <Repositories>
        {repositories.map(repository => (
          <Link
            to={`/repository/${repository.full_name}`}
            key={repository.full_name}
          >
            <img src={repository.owner.avatar_url} alt={repository.full_name} />

            <div>
              <strong>{repository.full_name}</strong>
              <p>{repository.description}</p>
            </div>

            <FiChevronRight size={20} />
          </Link>
        ))}
      </Repositories>
    </>
  );
};

export default Dashboard;
GlSOLi69;
```

## src/routes/index.tsx

```tsx
import React from "react";

import { Switch, Route } from "react-router-dom";

import Dashboard from "../pages/Dashboard";
import Repository from "../pages/Repository";

const Routes: React.FC = () => (
  <Switch>
    <Route path='/' exact component={Dashboard} />
    <Route path='/repository/:repository+' component={Repository} />
  </Switch>
);

export default Routes;
```

## src/pages/Repository/index.tsx

```tsx
import React from "react";
import { useRouteMatch } from "react-router-dom";

interface RepositoryParams {
  repository: string;
}

const Repository: React.FC = () => {
  const { params } = useRouteMatch<RepositoryParams>();
  return <h1>Repository: {params.repository}</h1>;
};

export default Repository;
```
