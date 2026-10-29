# Detalhes Técnicos

## Stack Tecnológica

### Backend
*Java 21 LTS, Spring Boot 3.3 e Spring Security*

Long-Term Support mais recente. Traz Virtual Threads que é interessante para criar DTOs imutáveis. Algo importante para a segurança, pois imutabilidade no trânsito de dados evita mutações indesejadas de payloads.

### Frontend
*Angular 18+*

Estrutura MVC rígida, roteamento seguro integrado e interceptores HTTP nativos, otimos para usar com JWT token.
### Banco de Dados
*PostgreSQL 16+*

Um padrão para sistemas relacionais. Suporte robusto a transações ACID (Atomicidade, )Consistência, Isolamento e Durabilidade) e suporta nativamente a criptografia de dados sensíveis.
### Infraestrutura e Deploy Local
*Docker + Docker Compose*

Muito requisitado atualmente e essencial para orquestrar o PostgreSQL, o backend Java, o frontend Angular e um possível update futuro para integração de ferramentas de de segurança como Keycloak e Wazuh.

### Segurança (Security by Design)
A segurança da plataforma não atua como um recurso adicional, mas como base arquitetural, emulando os rigorosos padrões de proteção de uma aplicação corporativa B2C multilocatário.

- **Defesa Ativa contra BOLA/IDOR:** Validação estrita de *ownership* (propriedade do recurso) na Camada de Serviço. Toda requisição cruza a identidade extraída do JWT com o dono do recurso no banco de dados, neutralizando manipulações de IDs.
- **Criptografia em Repouso:** Implementação de *Envelope Encryption* (AES-GCM protegido por uma Master Key) para blindar PII (Informações Pessoalmente Identificáveis) e tokens de integrações no banco de dados.
- **Pipeline de Segurança Contínua (DevSecOps):** Integração de ferramentas automatizadas no fluxo de CI/CD:
  - **SAST (Static Application Security Testing):** Varreduras nos Pull Requests utilizando **Semgrep** para bloquear falhas estruturais e más práticas antes do *merge*.
  - **SCA & Secrets:** Escaneamento de dependências e imagens Docker em busca de CVEs utilizando **Trivy**, aliado ao **Gitleaks** para prevenir o vazamento de credenciais no código.
  - **DAST (Dynamic Application Security Testing):** Execução de ataques automatizados contra os *endpoints* da API utilizando o **OWASP ZAP** no ambiente conteinerizado.

## Arquitetura do Sistema
De momento a arquitetura escolhida é o **Monolito Modular API-First**. Essa abordagem garante velocidade de desenvolvimento e facilidade de deploy, mantendo o código  estritamente desacoplado para um futura divisão, caso a plataforma escale.

Optou-se por não seguir a rota de Microsserviços logo de início, pois introduz uma complexidade de rede e orquestração desnecessária nesse momento. Futuramente o aplicativo passará por reformulações e será implementado tal arquitetura.

*API First é uma metodologia de desenvolvimento onde o design, a definição e a documentação da API são realizados antes de qualquer linha de código.*
### Monolito Modular
Para sustentar o Monolito Modular e garantir que a segurança seja enraizada no design do código (Security by Design), o projeto adota os seguintes padrões:

1. Layered Architecture (Arquitetura em Camadas Estritas)
A separação de responsabilidades é vital para testabilidade e segurança. O tráfego de dados obedece a um fluxo unidirecional restrito:

    -  Controllers (Presentation): Focados exclusivamente em receber requisições HTTP, tratar parâmetros, delegar a lógica e retornar respostas com os Status Codes adequados. Nenhuma regra de negócio vive aqui.

    - Services (Business & AppSec Layer): O núcleo da aplicação. É nesta camada que reside a proteção contra BOLA/IDOR (Broken Object Level Authorization). Antes de qualquer operação de leitura, alteração ou exclusão, o Service extrai a identidade do usuário autenticado no contexto (via JWT) e cruza com a propriedade (ownership) do recurso no banco de dados.

    - Repositories (Persistence Layer): Interfaces baseadas em Spring Data JPA dedicadas exclusivamente à abstração e comunicação com o PostgreSQL, isolando o banco de dados do resto da aplicação.

2. Autenticação Stateless (Sem Estado)
O sistema foi desenhado para não armazenar estado de sessão na memória do servidor (como as antigas sessões baseadas em Cookies/JSESSIONID).

    - Escalabilidade Horizontal: Toda requisição do client-side contém as credenciais necessárias para validação através de um JWT (JSON Web Token) assinado. Isso significa que a API RESTful pode ser replicada em múltiplos containers sem a necessidade de sincronizar sessões entre eles.

    - Isolamento de Segurança: O uso do JWT permite a separação clara de papéis (RBAC - Role-Based Access Control), como ROLE_FREE, ROLE_PREMIUM e ROLE_ADMIN, embutidos de forma segura no payload do token assinado criptograficamente, garantindo que adulterações pelo lado do cliente sejam imediatamente rejeitadas pelo Spring Security.