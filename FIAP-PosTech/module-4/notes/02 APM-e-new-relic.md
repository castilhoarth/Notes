 # New Relic

 - Oferece 100 GB de ingestão de dados gratuita
 - Usuário ilimitado com acesso a todos os módulos
 - Organização de contas
    - Organição contém _accounts_
    - Accounts: possui dados, usuários associados e configurações.
    - Usuário
- Níveis de contas
    - Basic (read only): acessos a dashboards e coisas
    - Core: acesso a logs, errors, integração com IDE e debugs
    - Full: acesso irrestrito (admins)

## Conceito de transações
- Unidade lógica de trabalho monitorada pelo New Relic
- Desde entrada até saída
- Tipos
    - Web: iniciadas por requisiçõpes http (sync) críticas para UX
    - Background: jobs, fila, cron (async)

## Instrumentação automática e manual

- Automática
    - Feita por agentes
    - Ideal para uso geral e início rápido
- Manual
    - Uso de SDKs ou APIs
    - Requer alteração e é ideal para processos complexos e KPIs

## Modelos de entidades
- Entidade: qualquer objeto monitorado que emite dados
- Principais tipos:
    - Services: aplicações backend, transações de BD etc;
    - Infrastructure: host, containers e nuvem
    - Browser: Aplicações frontend
    - Synthetics: monitores proativos que simulam comportamento

## Entity Explorer e LookOut
- Entity Explorer: inventário global e filtrável de todas as entidades
- Entity Navigator: Visualização de alta densidade. Permite ver status de saúde de milhares de entidades em uma só tela
- New Relic LookOut: visualização de desvios de comportamento
    - Analisa a partir do histórico
    - Sem necessidade configuração prévia
- Fluxo: Navigator -> lookout -> trace detail

## Service Map
- Serve para visualizar dependências
- Construido automaticamento a partir da análise de tráfego

## Tagging e Metadata
- Tags: metadados de alto nível. Mutáveis e usados para filtragem de inventário e agrupamento.
- Custom Attributes: metadados de baixo nível e associados a eventos de forma imutável

## Governança de Tags
- Env, team, service-level e owner são tags obrigatórias em escala

## APDEX Score
- Métrica para medir satisfação do usuário

## Métricas e eventos
- É possível coletar métricas customizadas e registros JSONs completos

## Sampling e retenção de dados
- Mecanismo integrante do new relic que armazena parte dos traces para otimizar recursos
- New Relic equilibra custo e performance com a seguinte estratégia:
    - Métricas e logs por mais tempo
    - Eventos e traces por 8 dias (mais pesados)

## Usuários e papéis
- Sistema de controle no New Relic é o RBAC
- Usuários adicionados à uma org e podem ter uma ou mais contas associadas a uma role dentro delas

## Tipos de chaves
- License: usadas para enviar dados de telemetria
- User: herda permissões do user e permite integrar com API e base de dados
- Browser: coletar dados de perfomance do frontend

## Arquitetura do New Relic (APM) — Resumo

 - New Relic é uma plataforma distribuída para coleta, processamento, análise e visualização de telemetria de aplicações e infraestrutura.
 - A arquitetura é organizada em pilares complementares:
     - Agentes de telemetria: bibliotecas ou serviços que instrumentam aplicações e recursos para coletar métricas, traces, eventos e logs.
         - Em aplicações, os agentes podem fazer instrumentação automática ou manual, integrando-se ao runtime e interceptando chamadas a banco de dados, HTTP, frameworks e outros pontos de medição.
         - Os agentes mantêm buffers locais e enviam dados periodicamente em ciclos de "harvest" (envio/flush).
     - Rede de ingestão: endpoints distribuídos que recebem os dados dos agentes; autenticam (license/API key), validam, enriquecem (metadata), normalizam e encaminham para os sistemas de processamento/armazenamento.
     - Armazenamento e processamento: sistemas otimizados para métricas, eventos, traces e logs, com políticas de retenção e amostragem; permite consultas e análises via NRQL (New Relic Query Language) e APIs.
     - Camada de plataforma / UI: dashboards, visualizações APM, distributed tracing, alertas e ferramentas de investigação que expõem os dados para usuários e equipes.

 ## Pontos importantes

 - Tipos de agente: APM (Java, .NET, Node.js, Python, Ruby, PHP, Go), Infrastructure agent, Browser, Mobile, Logs, Synthetic, entre outros.
 - Harvest cycle: período em que o agente agrupa dados localmente e faz o envio ao serviço de ingestão.
 - Amostragem e retenção: traces e eventos podem ser amostrados; retenção varia por tipo de dado e plano contratado.
 - Segurança: comunicação TLS, autenticação por chaves; cuidado com dados sensíveis na instrumentação (mascaramento/omitência).
 - Integrações: suporte a cloud providers, Kubernetes, plataformas de mensageria e sistemas de logging.

 ## Referências rápidas

 - Consultas: NRQL para análise ad-hoc.
 - Conceitos: instrumentação, harvest, ingestão, enrichment, armazenamento especializado, UI/alerts.

 (Notas: corrigi ortografia, atualizei termos técnicos e acrescentei pontos sobre agentes, ingestão, armazenamento e segurança.)

## Exemplos práticos

- Ciclo de "harvest" (exemplo):
    - Agente coleta métricas/traces localmente por um período (p.ex. 60s), agrupa em buffers e prepara payloads.
    - Ao final do ciclo, o agente envia (flush) os dados para os endpoints de ingestão via HTTPS/TLS.
    - A ingestão valida, enriquece e encaminha para processamento; em caso de falha o agente faz retry com backoff.
    - Observação: durações e frequência variam por agente e configuração (amostragem, batch size, limites de banda).

- NRQL — consultas básicas úteis:
    - Contagem de transações nas últimas 1 hora:
        - `SELECT count(*) FROM Transaction WHERE appName = 'My App' SINCE 1 hour ago`
    - Latência média por rota (últimos 30 minutos):
        - `SELECT average(duration) FROM Transaction WHERE appName = 'My App' FACET name SINCE 30 minutes`
    - Traces por operação (ex.: consultas SQL) nas últimas 24h:
        - `SELECT count(*) FROM Span WHERE name LIKE '%SELECT%' SINCE 1 day ago`

- Exemplo rápido de instrumentação:
    - Node.js (auto-instrumentação): instale e inicialize o agente antes de outros módulos:
        - `npm install newrelic`
        - `require('newrelic')` no topo do arquivo de entrada; configure `newrelic.js` com a `license_key`.
    - Java (exemplo manual): use a API de agent annotations:
        - `import com.newrelic.api.agent.Trace;`
        - `@Trace public void minhaOperacao() { /* ... */ }`

## Observações finais

- Sempre evite enviar dados sensíveis (PII) nos traços; use mascaramento ou omita campos sensíveis na instrumentação.
- Verifique a política de retenção do seu plano e ative amostragem quando necessário para reduzir custos.

--

(Adicione mais exemplos ou query snippets que queira salvar aqui.)