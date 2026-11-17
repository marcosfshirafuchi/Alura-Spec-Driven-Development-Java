# Módulo `controller`

> Documentação baseada nos arquivos presentes no ZIP em 17/11/2026. "Módulo" designa pacote lógico Java; não é módulo Maven independente.

## Objetivo
Interface HTTP REST.

## Responsabilidade principal
Recebe requisições, extrai identidade JWT, aplica validação quando anotada e delega aos serviços.

## Funcionalidades existentes
AuthController, CartController, CatalogController, OrderController, PaymentController, UserController.

## Arquivos críticos
`AuthController.java`, `CartController.java`, `CatalogController.java`, `OrderController.java`, `PaymentController.java`, `UserController.java`.

## Dependências internas e externas
service, config; Spring MVC e Spring Security.

**Dependências observáveis por imports/pacotes:** service.

## Módulos relacionados
Consulte `docs/ARQUITETURA_DO_SISTEMA.md` para o grafo completo. Relação arquitetural: HTTP → controller → service → repository → model/PostgreSQL; Redis é acessado diretamente por AuthService e CartService.

## Pontos de entrada
Endpoints /api/v1/auth, /cart, /categories, /products, /orders, /payments, /users.

## Fluxos importantes
HTTP → Controller → Service; CatalogController usa @PreAuthorize para alterações administrativas.

## Observações técnicas e débitos
DTOs são records definidos em serviços; respostas retornam entidades JPA diretamente, gerando acoplamento ao modelo e possíveis problemas de serialização.

## Diretriz SDD
Antes de alterar este pacote, escrever especificação com comportamento atual, contrato desejado, regras verificáveis, casos de erro, critérios de aceite e testes de regressão. Não assumir comportamento não demonstrado no código.
