# Arquitetura do DEM

## Visao geral

O projeto segue arquitetura de tres componentes:

- Backend API central (NestJS + TypeORM + SQLite)
- Frontend web administrativo (React + Vite)
- Cliente executor local (Python)

## Componentes

### Backend

- Local: backend
- Porta de desenvolvimento: 3026
- Responsabilidades:
  - autenticacao e autorizacao por perfil;
  - CRUD de regras de verificacao;
  - recebimento e persistencia de resultados;
  - agregacao de dispositivos e aliases.

Modulos principais:

- auth
  - login, cadastro, sessao e troca de senha;
  - gestao de usuarios e papel ADMIN/NORMAL.

- verifications
  - define regras tecnicas com campos:
    - name;
    - description;
    - active;
    - command;
    - expectedOutput.

- verification-results
  - recebe execucoes vindas dos clientes;
  - persiste historico por linha de execucao (append-only);
  - expoe resumo de dispositivos e alias.

### Frontend

- Local: frontend
- Porta de desenvolvimento: 3027
- Responsabilidades:
  - login e sessao de usuario;
  - dashboard de metricas;
  - gestao de regras, usuarios e dispositivos;
  - visualizacao de resultados.

Telas operacionais:

- Dashboard
- Painel de Status
- Regras
- Dispositivos
- Usuarios
- Configuracoes

Mapa visual das telas:

- Login:
  <img src="../assets/login.png" alt="Login" width="960" loading="lazy" />
- Criacao de conta:
  <img src="../assets/create-account.png" alt="Criacao de conta" width="960" loading="lazy" />
- Dashboard:
  <img src="../assets/dashboard.png" alt="Dashboard" width="960" loading="lazy" />
- Status por regra:
  <img src="../assets/status-rule.png" alt="Status por regra" width="960" loading="lazy" />
- Status por dispositivo:
  <img src="../assets/status-device.png" alt="Status por dispositivo" width="960" loading="lazy" />
- Regras:
  <img src="../assets/rules.png" alt="Regras" width="960" loading="lazy" />
- Criacao de regra:
  <img src="../assets/create-rule.png" alt="Criacao de regra" width="960" loading="lazy" />
- Dispositivos:
  <img src="../assets/devices.png" alt="Dispositivos" width="960" loading="lazy" />
- Usuarios:
  <img src="../assets/users.png" alt="Usuarios" width="960" loading="lazy" />

### Cliente

- Local: client/run_client.py
- Responsabilidades:
  - obter regras ativas em /verifications/active;
  - executar comando da regra na maquina local;
  - validar expectedOutput quando definido;
  - enviar payload para /verification-results.

Informacoes de contexto enviadas por execucao:

- machineId
- machineName
- username
- verificationId
- result
- output
- processedAt

## Fluxos principais

### Fluxo de configuracao de regras

1. Admin autentica no frontend.
2. Frontend chama backend para criar/editar regra.
3. Regra fica disponivel para consulta ativa pelos clientes.

### Fluxo de execucao distribuida

1. Cliente busca regras ativas.
2. Cliente executa comandos localmente.
3. Cliente envia resultado para API.
4. Backend armazena uma nova linha por execucao.

### Fluxo de monitoramento

1. Frontend busca resultados, regras ativas e dispositivos.
2. Frontend consolida metricas de ok, erro e nao executado.
3. Dashboard calcula ranking de pendencias e stale users.

## Modelo de dados (alto nivel)

- users
  - credenciais e papel de acesso.

- verifications
  - regras tecnicas ativas/inativas.

- verification_results
  - historico de execucoes por regra e maquina.

- machine_aliases
  - nome amigavel por machineId.

## Seguranca e acesso

- Login cria token de sessao em memoria no backend.
- Endpoints administrativos exigem Authorization: Bearer token.
- Primeiro usuario registrado recebe papel ADMIN.
- O sistema impede remover o ultimo administrador.

## Endpoints principais

Auth:

- POST /auth/login
- POST /auth/register
- GET /auth/me
- PUT /auth/change-password
- GET /auth/users
- PUT /auth/users/:id/role

Regras:

- GET /verifications
- GET /verifications/active
- POST /verifications
- PUT /verifications/:id
- DELETE /verifications/:id

Resultados:

- POST /verification-results
- GET /verification-results
- GET /verification-results/devices
- PUT /verification-results/devices/:machineId/alias

## Decisoes tecnicas atuais

- SQLite para simplicidade local e setup rapido.
- Sessao em memoria para fluxo inicial de autenticacao.
- Cliente em Python para facilitar execucao em ambientes Linux.

## Limitacoes conhecidas

- Sessao em memoria nao e ideal para horizontal scaling.
- Persistencia SQLite pode limitar cenarios de alta concorrencia.
- Nao ha agendador central de execucao no estado atual.

## Evolucoes recomendadas

1. JWT com expiracao e estrategia de revogacao.
2. Migracao para PostgreSQL em ambiente compartilhado.
3. Agendamento central e politicas de frequencia por regra.
4. Notificacoes proativas para pendencias criticas.