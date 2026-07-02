# Docker Compose - Comandos de Operacao

Este guia centraliza os comandos para subir, parar, diagnosticar e resetar o ambiente Docker do DEM.

## Requisito

- Docker Engine em execucao.
- Docker Compose disponivel no host.

## Subir ambiente

Na raiz do projeto:

```bash
docker-compose up -d
```

Com rebuild das imagens:

```bash
docker-compose up -d --build
```

## Parar ambiente

Parar e remover containers/rede:

```bash
docker-compose down
```

Parar sem remover:

```bash
docker-compose stop
```

Retomar apos stop:

```bash
docker-compose start
```

## Ambiente zerado

Remove containers, rede e volume de dados (SQLite):

```bash
docker-compose down -v
```

Sobe novamente do zero:

```bash
docker-compose up -d --build
```

## Logs e status

Ver status dos servicos:

```bash
docker-compose ps
```

Ver logs de todos os servicos:

```bash
docker-compose logs -f
```

Ver logs apenas do backend:

```bash
docker-compose logs -f backend
```

Ver logs apenas do frontend:

```bash
docker-compose logs -f frontend
```

## Acesso

- Frontend: http://localhost:3027
- Backend: http://localhost:3026

## Observacao sobre comando alternativo

Em alguns ambientes, o comando pode ser:

```bash
docker compose up -d
```

Se o seu host nao suportar essa forma, use o comando com hifen (docker-compose), que e o padrao documentado neste projeto.