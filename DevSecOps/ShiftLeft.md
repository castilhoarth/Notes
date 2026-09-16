# Shift Left Security

## Secret Sprawl

* Ocorre quando credenciais são armazenadas em locais incorretos, como logs ou arquivos de configuração (tokens da AWS, chaves de API, logins etc.).

* DevSecOps chega para promover segurança de forma proativa sem deixar que essas vulnerabilidades cheguem ao repositório.

### Estratégias de defesa

* Pré-commit hooks: scripts que rodam antes da efetivação do commit e que bloqueiam o commit em caso de erros.
  * Impede que segredos entrem no histórico.

* Varredura contínua: atua como mais uma camada de segurança para interromper pipelines e bloquear pull requests.

* Regras de proteção de branch (branch protection rules): atuam como gatekeepers para proteger branches de missão crítica.
  * Três requisitos técnicos viáveis são:
    * Require status check before merging
    * Require signed commits
    * Require linear history

## Gestão de Identidades

* Devem seguir os seguintes princípios:
  * Minimização de risco com credenciais temporárias
  * MFA
  * Princípio do menor privilégio

### Segredos de build vs Runtime

* Build: usados pelo CI — como tokens para acessar ECR — e devem ser guardados nos cofres nativos do CI.

* Runtime: usados pela aplicação em execução (credenciais de BD) — devem ser guardados em cofres de nuvem especializados que realizam rotação.

* Guardar segredos do CI no pipeline de CD é uma prática inadequada.
  * Separação de responsabilidades: CI e CD têm ciclos de vida e necessidades diferentes.
  * Maior superfície de ataque: pipelines de CD costumam ter acesso a ambientes de produção.
  * Princípio do menor privilégio: não exponha segredos que componentes de deployment não precisam.
  * Rotação e efemeridade: mistura dificulta rotação segura de credenciais de CI.
  * Risco de vazamento: logs, artefatos e variáveis no CD podem expor segredos indevidamente.
