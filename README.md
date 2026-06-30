Aqui está um exemplo de um arquivo `README.md` para o seu jogo:

---

# Jogo de Adivinhação com Flask

Este é um simples jogo de adivinhação desenvolvido utilizando o framework Flask. O jogador deve adivinhar uma senha criada aleatoriamente, e o sistema fornecerá feedback sobre o número de letras corretas e suas respectivas posições.

Guess Game - Docker Compose

Este projeto implementa o jogo de adivinhação utilizando uma arquitetura com Docker Compose, contendo:

Backend em Flask (Python)
Frontend em React servido via NGINX
Banco de dados PostgreSQL
NGINX atuando como proxy reverso e balanceador de carga do backend

## Instalação

1. Clone o repositório:

   ```bash
   git clone https://github.com/fams/guess_game.git
   cd guess-game
   ```

1. Subir os containers

Após definir o diretório aplique o seguinte prompt:

  docker compose up -d --build

Nesta etapa 4 containers serão criados:
  * Postgres;
  * Backend-1;
  * Backend-2;
  * Nginx.

2. Com a criação bem-sucedida dos containers. Acesse o local host:

   http://localhost

3. Selecione "Create a game" e crie uma senha e receba um Guess ID

  Aplique o Guess ID e tente aplique a senha recebida ou peça para alguém descobrir a senha criada.

4. Outra opção de jogo:

Se preferir fazer diretamente pelo prompt. Aplique o seguinte prompt:

    curl -X POST http://localhost/api/create \
    -H "Content-Type: application/json" \
    -d '{"password":"teste123"}'

Após a aplição será gerado um Guess ID onde poderá testar através do seguinte prompt:

    curl -X POST http://localhost/api/guess/<GUESS ID> \
    -H "Content-Type: application/json" \
    -d '{"guess":"teste123"}'

5. Consulta a tabela Postgres. Aplique o prompt abaixo para acessar o banco:

   docker exec -it postgres psql -U postgres -d postgres

Aplique \dt e o seguinte código SQL:

    SELECT * FROM game;

