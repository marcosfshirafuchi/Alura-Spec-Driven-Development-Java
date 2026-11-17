# Módulo `service`

> Documentação baseada nos arquivos presentes no ZIP em 17/11/2026. "Módulo" designa pacote lógico Java; não é módulo Maven independente.

## Objetivo
Regras e orquestração de negócio.

## Responsabilidade principal
Executa cadastro/login, catálogo, carrinho, checkout, pagamentos e gestão de usuários.

## Funcionalidades existentes
AuthService, CartService, CatalogService, OrderService, PaymentService, UserService, ApiException.

## Arquivos críticos
`ApiException.java`, `AuthService.java`, `CartService.java`, `CatalogService.java`, `OrderService.java`, `PaymentService.java`, `UserService.java`.

## Dependências internas e externas
repository, model, config beans; Spring Security, Redis, JPA.

**Dependências observáveis por imports/pacotes:** model, repository.

## Módulos relacionados
Consulte `docs/ARQUITETURA_DO_SISTEMA.md` para o grafo completo. Relação arquitetural: HTTP → controller → service → repository → model/PostgreSQL; Redis é acessado diretamente por AuthService e CartService.

## Pontos de entrada
Métodos públicos dos seis serviços; @Transactional no checkout, confirmação e cancelamento.

## Fluxos importantes
Auth → UserRepository/Redis; Cart → ProductRepository/Redis; Order → CartService + repos; Payment → OrderService + repos

## Observações técnicas e débitos
Checkout cruza transação PostgreSQL e operação Redis; PaymentService.cancel altera estoque via entidade relacionada sem salvamento explícito de ProductRepository; idempotência apenas valida presença da chave.

## Diretriz SDD
Antes de alterar este pacote, escrever especificação com comportamento atual, contrato desejado, regras verificáveis, casos de erro, critérios de aceite e testes de regressão. Não assumir comportamento não demonstrado no código.
