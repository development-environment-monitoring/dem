# DEM - Development Environment Monitoring

Este repositório implementa o DEM, uma plataforma para monitorar conformidade técnica de ambientes de desenvolvimento.

## Documentação por assunto

- O que é o DEM e para quem serve: [docs/o-que-e-o-dem.md](docs/o-que-e-o-dem.md)
- Arquitetura técnica detalhada: [docs/arquitetura.md](docs/arquitetura.md)
- Comandos Docker Compose: [docs/docker-compose.md](docs/docker-compose.md)

## Requisitos

- Node.js 18+
- npm 9+
- Python 3.10+ (para o cliente)

## Como executar localmente

Abra dois terminais na raiz do projeto.

### 1) Backend

```bash
npm install --prefix ./backend
npm run start:dev --prefix ./backend
```

API disponível em http://localhost:3026

### 2) Frontend

```bash
npm install --prefix ./frontend
npm run dev --prefix ./frontend
```

Frontend disponível em http://localhost:3027

## Como executar com Docker Compose

Na raiz do projeto:

```bash
docker-compose up -d
```

Serviços disponíveis:

- Frontend: http://localhost:3027
- Backend: http://localhost:3026

Para derrubar:

```bash
docker-compose down
```

Para subir ambiente zerado (limpa banco do volume):

```bash
docker-compose down -v
docker-compose up -d --build
```

## Primeiro acesso

- Abra a tela de login no frontend.
- Cadastre o primeiro usuário.
- O primeiro cadastro vira ADMIN automaticamente.
- Usuários seguintes entram como NORMAL.

## Executando o cliente de verificação

Com backend ativo e regras criadas:

```bash
python3 ./client/run_client.py --api-base-url http://localhost:3026
```

Também é possível usar variável de ambiente:

```bash
export DEM_API_BASE_URL=http://localhost:3026
python3 ./client/run_client.py
```

## Endpoints principais

- Auth:
	- POST /auth/login
	- POST /auth/register
	- GET /auth/me
	- PUT /auth/change-password
	- GET /auth/users (admin)
	- PUT /auth/users/:id/role (admin)

- Regras:
	- GET /verifications (admin)
	- GET /verifications/active (público para cliente)
	- POST /verifications (admin)
	- PUT /verifications/:id (admin)
	- DELETE /verifications/:id (admin)

- Resultados:
	- POST /verification-results (cliente envia)
	- GET /verification-results (admin)
	- GET /verification-results/devices (admin)
	- PUT /verification-results/devices/:machineId/alias (admin)

## Estrutura resumida do repositório

- backend: API e persistência.
- frontend: painel web.
- client: executor local das verificações.
