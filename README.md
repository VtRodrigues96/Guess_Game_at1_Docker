# Guess Game - Docker Compose

## Descrição

Este projeto consiste na implementação do jogo de adivinhação disponibilizado em:

https://github.com/fams/guess_game

A aplicação foi containerizada utilizando Docker Compose, atendendo aos requisitos da atividade propostos para:

- Backend Flask (Python)
- Banco de dados PostgreSQL
- Frontend React
- NGINX atuando como servidor web, proxy reverso e balanceador de carga

O objetivo do trabalho foi estruturar a aplicação utilizando containers sem alterar a lógica original do sistema.

Foi utilizada uma arquitetura baseada em Docker Compose porque ela permite padronizar o ambiente de execução, simplificar a implantação da aplicação, garantir reprodutibilidade entre diferentes máquinas e facilitar manutenção, atualização e escalabilidade. Dessa forma, o usuário precisa apenas executar docker compose up -d --build, sem instalar manualmente Python, Node.js, PostgreSQL ou suas dependências.

---

# Arquitetura da Solução

A arquitetura é composta pelos seguintes serviços:

## PostgreSQL

Responsável pelo armazenamento dos dados do jogo.

Características:

- Executado em container dedicado.
- Persistência através de volume Docker.
- Dados mantidos mesmo após reinicialização dos containers.

---

## Backend Flask

Responsável pelas regras de negócio do jogo.

Características:

- Executado em containers independentes.
- Conectado ao PostgreSQL.
- Escalável através da criação de múltiplas instâncias.

Nesta implementação são executadas duas instâncias:

- backend-1
- backend-2

---

## Frontend React

Responsável pela interface do usuário.

Características:

- Aplicação React compilada para produção.
- Arquivos estáticos servidos pelo NGINX.

---

## NGINX

Responsável por:

- Servir os arquivos do frontend React.
- Atuar como proxy reverso.
- Realizar balanceamento de carga entre múltiplas instâncias do backend.

Fluxo da aplicação:

```text
Usuário
   ↓
 NGINX
   ↓
Backend Flask (1)
Backend Flask (2)
   ↓
 PostgreSQL
```

---

# Decisões de Design

## Utilização do PostgreSQL

Foi utilizado PostgreSQL por ser um banco relacional robusto e amplamente utilizado em ambientes corporativos.

---

## Persistência dos Dados

Foi criado um volume Docker dedicado para o banco de dados.

Benefícios:

- Persistência dos dados após reinicialização.
- Independência entre dados e containers.
- Facilidade de manutenção.

---

## Balanceamento de Carga

O NGINX foi configurado como proxy reverso para distribuir as requisições entre múltiplas instâncias do backend.

Benefícios:

- Melhor distribuição das requisições.
- Maior disponibilidade da aplicação.
- Facilidade para escalar novos containers.

---

## Facilidade de Atualização

Cada componente é independente.

Isso permite atualizar:

- Backend
- Frontend
- Banco de Dados

apenas alterando a versão da imagem correspondente e reconstruindo os containers.

---

# Estrutura do Projeto

```text
.
├── backend/
├── frontend/
├── nginx/
├── docker-compose.yml
├── README.md
```

---

# Pré-requisitos

É necessário possuir instalado:

- Docker
- Docker Compose

Verificação:

```bash
docker --version
docker compose version
```

---

# Instalação

## 1. Clonar o repositório

```bash
git clone https://github.com/VtRodrigues96/Guess_Game_at1_Docker.git
cd Guess_Game_at1_Docker
```

---

## 2. Construir e iniciar os containers

```bash
docker compose up -d --build
```

Serão criados os seguintes containers:

- postgres
- backend-1
- backend-2
- nginx

---

## 3. Verificar os containers

```bash
docker ps
```

Todos os containers devem estar com status:

```text
Up
```

---

# Acesso à Aplicação

Após a execução do comando:

```bash
docker compose up -d --build
```

A aplicação estará disponível em:

```text
http://localhost
```

Esta é a URL que deve ser utilizada para acessar o sistema.

---

# Utilização pela Interface Web

1. Acesse:

```text
http://localhost
```

2. Clique em:

```text
Create a Game
```

3. Informe uma senha.

4. O sistema retornará um Game ID.

5. Utilize o Game ID para realizar tentativas de adivinhação.

---

# Utilização pela API

## Criar um jogo

```bash
curl -X POST http://localhost/api/create \
-H "Content-Type: application/json" \
-d '{"password":"teste123"}'
```

Exemplo de retorno:

```json
{
  "game_id": "abc123"
}
```

---

## Realizar uma tentativa

```bash
curl -X POST http://localhost/api/guess/<GAME_ID> \
-H "Content-Type: application/json" \
-d '{"guess":"teste123"}'
```

Substitua:

```text
<GAME_ID>
```

pelo identificador retornado na criação do jogo.

---

# Consulta ao Banco de Dados

## Acessar o PostgreSQL

```bash
docker exec -it postgres psql -U postgres -d postgres
```

---

## Listar tabelas

```sql
\dt
```

---

## Consultar os jogos armazenados

```sql
SELECT * FROM game;
```

---

## Consultar um jogo específico

```sql
SELECT *
FROM game
WHERE game_id = '<GAME_ID>';
```

---

## Sair do PostgreSQL

```sql
\q
```

---

# Resiliência

A solução foi configurada para atender aos requisitos de resiliência da atividade.

## Reinício Automático

Todos os containers utilizam:

```yaml
restart: always
```

Dessa forma, caso ocorra falha em algum container, ele será reiniciado automaticamente.

---

## Persistência

O PostgreSQL utiliza volume dedicado para armazenamento permanente dos dados.

---

## Balanceamento de Carga

O NGINX distribui as requisições entre múltiplas instâncias do backend.

---

# Atualização dos Componentes

A arquitetura permite atualização independente dos serviços.

## Atualizar Backend

Atualizar a imagem do backend e reconstruir:

```bash
docker compose up -d --build
```

---

## Atualizar Frontend

Gerar um novo build React e reconstruir:

```bash
docker compose up -d --build
```

---

# Atendimento aos Requisitos da Atividade

| Requisito | Status |
|-----------|---------|
| Backend Flask em container | ✔ |
| PostgreSQL em container | ✔ |
| NGINX em container | ✔ |
| Frontend servido pelo NGINX | ✔ |
| Proxy reverso | ✔ |
| Balanceamento de carga | ✔ |
| Persistência via volume | ✔ |
| Reinício automático | ✔ |
| Facilidade de atualização | ✔ |
| Docker Compose | ✔ |
| README de instalação e utilização | ✔ |

---

# Autor

Vitor Rodrigues

Projeto desenvolvido para a atividade 1 de Docker utilizando o projeto Guess Game.
