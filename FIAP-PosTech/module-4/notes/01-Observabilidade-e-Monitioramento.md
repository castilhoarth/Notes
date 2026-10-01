# Introdução à Observabilidade e Monitoramento

- Monitoramento: acompanhamento contínuo da saúde dos sistemas e visa tornar transparente o que está acontecendo no sistema
    - Baseado em métricas que tornam o sistema uma _caixa branca_
        - Logs e alertas

- Observabilidade: capacidade de entender o porquê de uma falha a partir de saídas do sistema
    - Instrumentação profunda e correlação de dados
    - Compreensão do comportamento interno

- Importância em sistemas modernos
    - Microserviços trazem mais pontos de falha
    - Cloud é um ambiente dinâmico que traz mais desafios de gestão

- Impacto no time:
    - Suporte: detecta problemas de maneira mais rápida
    - DevOps: consegue identificar gargalos
    - Negócios: consegue promover melhora na experiência do cliente final

- Métricas: dados agregados em uma janela de tempo
    - Alertas baseados em limiares
    - Usadas para detecção rápida
    - Ex: uso de CPU, memória e latência

- Logs: registro histórico de eventos de forma detalhada
    - Alto detalhamento
    - Ótimo para auditoria
    - Ex: erros de autenticação

- Trace: linha do tempo de uma requisição ponta a ponta
    - Útil para identificação de gargalos
    - Rastrear a jornada
    - Podem gerar alertas a partir de falhas em parte do fluxo

- Alertas
    - Permitem respostas em tempo real
    - Dimensionamento: conceito que expressa os critérios para definição de gatilhos
        - Limiar estáticos vs dinâmicos
        - Bom dimensionamento evita fadiga de alertas
        - SLI e SLO como base

- Alertas técnicos vs. de negócio
    - Técnicos: infraestrutura, sistemas e aplicações
    - De negócio: vendas, pedidos e taxa de churn

- Conceitos em métricas
    - Percentis: trazem análise mais precisa ao considerar porcentagem de usuários que não tiveram problemas
        - Ex: p95 de transações com menos de 5ms
            - 5% sofreram com maiores lentidões

    - Cardinalidade: quantidade de valores distintos que uma métrica ou label pode assumir
        - Quanto maior a cardinalidade, maior o custo, o processamento e o esforço para consulta

    - Rollups: agregação de dados ao longo do tempo para diminuir consumo de armazenamento e viabilizar análise histórica do sistema

- Conceitos em logs
    - Stack trace: pilha de chamadas de função no momento em que um evento ocorre
    - Categorização e ID: é importante categorizar logs em níveis e identificá-los com IDs para serem cruzados com outros dados

- Conceitos em traces
    - OpenTelemetry é o padrão utilizado para unificar geração e exportação de traces (métricas e logs também)

## Fundamentos dos pilares de monitoramento e observabilidade

- É necessário ser criterioso com o que logar para não consumir muito armazenamento
- Métricas ajudam a detectar problemas, logs dão contexto e traces mostram o caminho da execução

### Backends e stacks de monitoramento e observabilidade

- Prometheus + Grafana
    - Prometheus: ferramenta open source que coleta e armazena métricas (backend de métricas)
    - Grafana: complementa Prometheus com visualizações e dashboards

- ELK
    - Processa logs e métricas, os armazena e também fornece visualização

- Jaeger ou Zipkin
    - Ferramentas para entender jornadas de requisição via traces

- OpenTelemetry
    - Padrão open source para coleta de dados e viabilizar comunicação entre as ferramentas de observabilidade e monitoramento
    - Essa coleta de dados e do comportamento de um sistema se chama instrumentação

### Ferramentas SaaS para observabilidade
 - Datadog, New Relic e Splunk oferecem soluções completas e gerenciadas para observabilidade
 - Substituem ELK, Prometheus + Grafana e Jaeger em troca de pagamentos e vendor lock-in

### OS vs SaaS
 - OS é ideal para equipes com conhecimento especializado e necessidade de customização
 - SaaS é para equipes que priorizam agilidade e foco no core business

## APM e instrumentação
 - Monitoramento ponta a ponta: seguir uma requisição por todo o seu ciclo de vida
     - Permite compreender cada etapa do fluxo
     - Ajuda a identificar gargalos
         - Consultas lentas ou perda de pacotes
 - Métricas de desempenho: viabilizam avaliação de eficiência e saúde de um sistema
     - Ex:
         - throughput — volume de requisições por unidade de tempo
         - Latência — tempo para responder uma requisição
         - Taxa de erro - percentual de falhas

- Traces distribuídos
    - Utilizados para rastrear uma requisição pelo sistema
    - Importante no contexto de microserviços e sistmas distribuídos
    - TraceID: identifica a requisição distribuída (linha do tempo)
    - SpanID: identifica operações individuais do fluxo
        - Contém informações como tempo de execução, status, atributos etc
- É importante correlacionar dados técnicos e métricas de negócio para realizar priorizações
    - Pensar em uma visão integrada
- Instrumentação
    - Processo de adicionar mecanismos de coleta de dados dentro da aplicação
    - Pode ser feita manualmente: maior controle e maior necessidade de manutenção
    - Instrumentação automática: envolve interceptar chamadas sem necessidade de mudar o código

## Fundamentos de coleta de métricas

### Camadas de coleta de métricas
- Métricas de Infra (Camada 1)
    - CPU, memória, disco e rede
    - Medição de disponibilidade e desempenho dos recursos

- Métricas de SO (Camada 2)
    - Processos e consumo de recursos
    - Logs do Sistema Operacional

- Máquinas virtuais e hosts de nuvem (Camada 3)
    - Cloudwatch e Azure Monitor
    - Escalabilidade automática e infra elástica

- Orquestração (k8s e Docker Swarm) (Camada 4)
    - Status dos nós e saúde dos pods/containers

- Contêineres (Camada 5)
    - Uso de CPU e memória
    - Restart count e latência de rede entre os contêineres

- Aplicação (Camada 6)
    - Métricas de negócio
    - Coletadas por agentes e exporters

### Modelos de coleta
- Pull: agente busca métricas periodicamente
- Push: aplicações enviam métricas para coletor
    - Especialmente útil para jobs de curta duração

- Trade-off: frequência de coleta x custo
    - Quanto mais dados, maior o entendimento do sistema, porém o custo de análisar, armazenar e gerenciar esse volume de dados se torna maior

### Padrões abertos de coleta
- OpenMetrics
    - Padrão para exposição e formatação de métricas
    - Baseado no ecossistema _Prometheus_
- OpenTelemetry
    - Framework unificado para métricas, logs e traces
    - Facilita correlação e instrumentalização
- Uso de labels e tags
    - Ambientes dinâmicos exigem contexto nas métricas
    - Facilita análise cruzada entre múltiplas camadas
- Correlação entre métricas
    - Fornecem um entendimento multifator de um problema e evitam diagnósticos precipitados
    - Ex: Latência alta e High CPU Usage

### Boas práticas e estratégias
- Não monitorar apenas infra
    - Monitorar app, experiência de usuário e negócio
- Unificar camadas de observabilidade
    - Permite buscas eficientes e correlações automáticas
- Automatização é necessária em contextos dinâmicos
    - Por exemplo, novas instâncias são monitoradas automaticamente
- Definição de KPIs e SLIs
    - Medir o que realmente importa
        - Disponibilidade, latência, taxa de erro e throughput
- Pipeline unificado de métricas
    - Coleta -> Armazenamento -> Análise -> Correlação

### KPIs (Key Performance Indicators): Técnicos vs Negócio

- Técnicos: Uso de CPU e memória, latência média e número de erros do tipo 5xx
- Negócio: taxa de conversão, tempo médio de processamento e disponibilidade para o cliente final

### Considerações de segurança e governança
- Endpoints não autorizados podem revelar topologia interna
- Versões de software inadequadas e métricas sensíveis podem facilitar ataques
- Práticas como redes privadas, anonimização e rotação de credenciais são necessárias
- Tags auxiliam na governanança e geração de relatórios

## Alertas
- Com o enorme volume de dados gerado por sistemas, o desafio é transformá-los em conhecimento
- Contexto é dado pela coleta e análise de dados
- _Detecção_ de padrões anormais geram um _Alerta_ que, por sua vez, aciona a equipe responsável que inicia uma _Ação_
### Dados em métricas acionáveis
- CPU a 90% sozinho pode não ser um problema, mas combinado com latência pode indicar degradação
### Tipos de alertas
- Técnico: relacionam-se com a infraestrutura, tempo de CPU, uso de memória etc
- Negócio: impacta diretamente o cliente como erros de login e queda no checkout
### Boas práticas com alertas
- Evitar ruído
- Definir prioridade
- Automatizar correlações
- Usar contexto
- Documentar ações para cada problema
- Usar como base conceitual para definição de thresholds SLIs e SLOs
### Alertas inteligentes
- Utilizam análise contextual para filtrar o que é importante do que que é ruído
- Machine Learning pode ser empregada nesse contexto
- Notificações também devem ser eficientes ao mandar para o time certo pelo canal de comunicação adequado
- Diferenciação de alertas críticos e informativos
- Playbooks e RunBooks que descrevem passo a passo como responder incidentes
- Automação de respostas basedados nesses documentos