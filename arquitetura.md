# Arquitetura do DEM

## Visão geral

O projeto segue uma arquitetura de três componentes:

- Backend API central (NestJS + TypeORM + SQLite)
- Frontend web administrativo (React + Vite)
- Cliente executor local (Python)

## Componentes

### Backend

- Local: backend
- Porta de desenvolvimento: 3026
- Responsabilidades:
  - autenticação e autorização por perfil;
  - CRUD de regras de verificação;
  - recebimento e persistência de resultados;
  - agregação de dispositivos e aliases.

Módulos principais:

- auth
  - login, cadastro, sessão e troca de senha;
  - gestão de usuários e papel ADMIN/NORMAL.

- verifications
  - define regras técnicas com campos:
    - name;
    - description;
    - active;
    - command;
    - expectedOutput.

- verification-results
  - recebe execuções vindas dos clientes;
  - persiste histórico por linha de execução (append-only);
  - expõe resumo de dispositivos e aliases.

### Frontend

- Local: frontend
- Porta de desenvolvimento: 3027
- Responsabilidades:
  - login e sessão de usuário;
  - dashboard de métricas;
  - gestão de regras, usuários e dispositivos;
  - visualização de resultados.

Telas operacionais:

- Dashboard
- Painel de Status
- Regras
- Dispositivos
- Usuários
- Configurações

Mapa visual das telas:

- Login:
  <img src="assets/login.png" alt="Login" width="960" loading="lazy" />
- Criação de conta:
  <img src="assets/create-account.png" alt="Criação de conta" width="960" loading="lazy" />
- Dashboard:
  <img src="assets/dashboard.png" alt="Dashboard" width="960" loading="lazy" />
- Status por regra:
  <img src="assets/status-rule.png" alt="Status por regra" width="960" loading="lazy" />
- Status por dispositivo:
  <img src="assets/status-device.png" alt="Status por dispositivo" width="960" loading="lazy" />
- Regras:
  <img src="assets/rules.png" alt="Regras" width="960" loading="lazy" />
- Criação de regra:
  <img src="assets/create-rule.png" alt="Criação de regra" width="960" loading="lazy" />
- Dispositivos:
  <img src="assets/devices.png" alt="Dispositivos" width="960" loading="lazy" />
- Usuários:
  <img src="assets/users.png" alt="Usuários" width="960" loading="lazy" />

### Cliente

- Local: client/run_client.py
- Responsabilidades:
  - obter regras ativas em /verifications/active;
  - executar o comando da regra na máquina local;
  - validar expectedOutput quando definido;
  - enviar payload para /verification-results.

Informações de contexto enviadas por execução:

- machineId
- machineName
- username
- verificationId
- result
- output
- processedAt

## Fluxos principais

### Fluxo de configuração de regras

1. Admin autentica no frontend.
2. O frontend chama o backend para criar/editar a regra.
3. A regra fica disponível para consulta ativa pelos clientes.

### Fluxo de execução distribuída

1. O cliente busca regras ativas.
2. O cliente executa comandos localmente.
3. O cliente envia o resultado para a API.
4. O backend armazena uma nova linha por execução.

### Fluxo de monitoramento

1. O frontend busca resultados, regras ativas e dispositivos.
2. O frontend consolida métricas de ok, erro e não executado.
3. O dashboard calcula o ranking de pendências e de usuários sem comunicação recente.

## Modelo de dados (alto nível)

- users
  - credenciais e papel de acesso.

- verifications
  - regras técnicas ativas/inativas.

- verification_results
  - histórico de execuções por regra e máquina.

- machine_aliases
  - nome amigável por machineId.

## Segurança e acesso

- Login cria token de sessão em memória no backend.
- Endpoints administrativos exigem Authorization: Bearer token.
- Primeiro usuário registrado recebe papel ADMIN.
- O sistema impede remover o último administrador.

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

## Decisões técnicas atuais

- SQLite para simplicidade local e setup rápido.
- Sessão em memória para o fluxo inicial de autenticação.
- Cliente em Python para facilitar a execução em ambientes Linux.

## Limitações conhecidas

- Sessão em memória não é ideal para escalabilidade horizontal.
- A persistência em SQLite pode limitar cenários de alta concorrência.
- Não há agendador central de execução no estado atual.

## Evoluções recomendadas

1. JWT com expiração e estratégia de revogação.
2. Migração para PostgreSQL em ambiente compartilhado.
3. Agendamento central e políticas de frequência por regra.
4. Notificações proativas para pendências críticas.