# Lumina

## 📖 Sobre o Projeto
O **Lumina** é uma plataforma SaaS (Software as a Service) projetada para estudantes e concurseiros. A aplicação permite o gerenciamento completo de ciclos de estudo, flashcards, revisões espaçadas e análise de métricas de simulados, operando sob modelos de assinatura (Freemium e Premium).

Mais do que um produto para o usuário final, a plataforma foi concebida sob a filosofia de **Security by Design**. O projeto atua como um ambiente real de aplicação de AppSec, implementando uma arquitetura multilocatário (multi-tenant) blindada contra vulnerabilidades corporativas críticas, como BOLA/IDOR, garantindo o isolamento total dos dados de cada assinante.

## 🎯 Objetivos Principais
* **Para o Usuário:** Entregar clareza e previsibilidade metodológica na preparação para provas, transformando o esforço de estudo em dados acionáveis.
* **Para a Engenharia:** Construir um Monolito Modular (API-First) limpo, escalável e de fácil manutenção.
* **Para a Segurança:** Proteger regras de negócio complexas, fluxos de pagamento e dados sensíveis, emulando os desafios reais de proteção enfrentados por equipes de AppSec no mercado.

## 🚀 Funcionalidades Core
* Gestão de Ciclos de Estudo e controle de horas.
* Sistema de Flashcards e métricas de repetição espaçada.
* Dashboards de desempenho em questões e simulados.
* Gestão de Assinaturas e integração segura com gateways de pagamento.
* Auditoria estruturada para monitoramento de eventos de segurança.

## 🛠️ Tecnologias e Segurança
> A documentação detalhada das decisões arquiteturais, stack tecnológica e controles de mitigação de vulnerabilidades pode ser encontrada no arquivo **TECHNICAL.md**

## ⚙️ Como Executar Localmente
*(Instruções futuras para deploy local via Docker Compose, variáveis de ambiente e inicialização da API e Banco de Dados).*
## Estrutura do repositorio

- Backend (Spring Boot/Maven) na raiz: `src/main/java/com/lumina/team`.
- `frontend/`: Angular (frontend).
- `compose.yaml`: servicos de infraestrutura (PostgreSQL).
- `src/main/resources/db/migration`: migracoes de banco.

