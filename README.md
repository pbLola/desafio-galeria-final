## Desafio Vue — Galeria de Imagens com Scroll Infinito

Galeria de imagens responsiva que utiliza a API da Pexels, com funcionalidades de busca com debounce e scroll infinito.

## 📸 Capturas de Tela

![Tela Principal da Galeria](./screenshots/captura1.png)

![Tela de Erro](./screenshots/captura2.png)

## Decisões Técnicas

Para a funcionalidade de **scroll infinito** foi escolhido usar a **`Intersection Observer API`**

## Como Rodar o Projeto

Siga os passos abaixo para executar o projeto na sua máquina.

**Pré-requisitos:**

- Node.js (v16 ou superior)
- npm ou Yarn

**Passos:**

1.  **Clone o repositório:**

    ```bash
    git clone [https://github.com/pbLola/desafio-galeria-final]
    ```

2.  **Navegue até a pasta do projeto:**

    ```bash
    cd [desafio-galeria-final]
    ```

3.  **Instale as dependências:**

    ```bash
    npm install
    ```

4.  **Configure sua chave da API:**

          * Crie um arquivo chamado `.env` na raiz do projeto.
          * Dentro dele, adicione sua chave da API da Pexels da seguinte forma:

    VITE_PEXELS_API_KEY=dihzk12BJ9rt5b29iyHXrX3ujcoeAlTIDPEq3mGPsZSCYMtUowqkVdcf

    ```

    ```

5.  **Execute o projeto:**

    ```bash
    npm run dev
    ```

A aplicação estará disponível no endereço que aparecer no seu terminal (geralmente `http://localhost:5173`).
