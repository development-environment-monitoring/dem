# O que e o DEM

## Definicao

DEM (Development Environment Monitoring) e uma solucao para monitorar conformidade tecnica de ambientes de desenvolvimento.

Em vez de observar apenas disponibilidade de servicos, o DEM verifica se cada maquina esta seguindo regras operacionais definidas pelo time, por exemplo:

- ferramenta instalada em versao minima;
- comando obrigatorio retornando saida esperada;
- padrao tecnico requerido para produtividade e seguranca.

## Problema que o DEM resolve

Times tecnicos costumam enfrentar variacao entre ambientes de desenvolvimento:

- em uma maquina funciona, em outra nao;
- onboarding inconsistente;
- perda de tempo com erros repetitivos de setup;
- baixa visibilidade sobre quem esta fora do padrao.

O DEM reduz esse gap ao transformar regras tecnicas em verificacoes automatizadas e centralizadas.

## Para quem e o DEM

O DEM e pensado para equipes que precisam governanca tecnica sem perder agilidade.

- Lideranca tecnica (Tech Leads, Staff, Arquitetura): define e evolui padroes.
- Equipe de plataforma/DevEx: monitora aderencia e remove friccao de ambiente.
- Times de desenvolvimento: recebem feedback objetivo sobre conformidade local.
- Operacao interna/seguranca: ganha trilha historica para auditoria tecnica.

## Como funciona no dia a dia

1. Um administrador cadastra regras no painel.
2. O cliente DEM executa as regras na maquina local.
3. O cliente envia resultado (success/error) e output para o backend.
4. O dashboard consolida status por usuario, dispositivo e regra.
5. A equipe prioriza correcoes com base em pendencias reais.

## Telas do sistema

### Acesso e onboarding

Tela de login:

<img src="../assets/login.png" alt="Tela de login" width="960" loading="lazy" />

Tela de criacao de conta:

<img src="../assets/create-account.png" alt="Tela de criacao de conta" width="960" loading="lazy" />

### Monitoramento e status

Dashboard:

<img src="../assets/dashboard.png" alt="Dashboard" width="960" loading="lazy" />

Painel de status por usuario/regra:

<img src="../assets/status-rule.png" alt="Painel de status por usuario e regra" width="960" loading="lazy" />

Painel de status por dispositivo:

<img src="../assets/status-device.png" alt="Painel de status por dispositivo" width="960" loading="lazy" />

### Governanca operacional

Lista de regras:

<img src="../assets/rules.png" alt="Lista de regras" width="960" loading="lazy" />

Criacao/edicao de regra:

<img src="../assets/create-rule.png" alt="Criacao de regra" width="960" loading="lazy" />

Lista de dispositivos:

<img src="../assets/devices.png" alt="Lista de dispositivos" width="960" loading="lazy" />

Gestao de usuarios:

<img src="../assets/users.png" alt="Gestao de usuarios" width="960" loading="lazy" />

## Diferenciais do DEM neste projeto

- Foco em conformidade tecnica, nao apenas uptime de infraestrutura.
- Coleta distribuida (execucao local), evitando suposicoes do servidor.
- Historico de resultados append-only para rastreabilidade.
- Visao de acao com ranking de pendencias e usuarios sem comunicacao recente.
- Identificacao amigavel de dispositivos por alias.
- Controle de acesso por perfil (ADMIN e NORMAL).

## O que o DEM nao substitui

O DEM nao substitui ferramentas classicas de observabilidade de producao.
Ele complementa esse ecossistema ao cobrir uma camada diferente: qualidade e padronizacao do ambiente de desenvolvimento.

## Quando adotar

O DEM tende a gerar mais valor quando:

- existe mais de um time/produto compartilhando padroes;
- o onboarding esta lento ou inconsistente;
- ha recorrencia de incidentes por divergencia de ambiente local;
- a equipe quer reduzir troubleshooting repetitivo.