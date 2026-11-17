# Módulo `repository`

> Documentação baseada nos arquivos presentes no ZIP em 17/11/2026. "Módulo" designa pacote lógico Java; não é módulo Maven independente.

## Objetivo
Acesso a dados relacionais.

## Responsabilidade principal
Expõe operações de persistência JPA e consultas derivadas para entidades de negócio.

## Funcionalidades existentes
UserRepository, CategoryRepository, ProductRepository, PurchaseOrderRepository, PaymentRepository.

## Arquivos críticos
`CategoryRepository.java`, `PaymentRepository.java`, `ProductRepository.java`, `PurchaseOrderRepository.java`, `UserRepository.java`.

## Dependências internas e externas
model; Spring Data JPA, PostgreSQL.

**Dependências observáveis por imports/pacotes:** model.

## Módulos relacionados
Consulte `docs/ARQUITETURA_DO_SISTEMA.md` para o grafo completo. Relação arquitetural: HTTP → controller → service → repository → model/PostgreSQL; Redis é acessado diretamente por AuthService e CartService.

## Pontos de entrada
Interfaces JpaRepository e métodos findByEmail, findByUserId, findByOrderId, findByNameContainingIgnoreCase.

## Fluxos importantes
Service → JpaRepository → PostgreSQL

## Observações técnicas e débitos
ProductRepository tem método paginado não usado em CatalogService.products; importações excedentes em interfaces.

## Diretriz SDD
Antes de alterar este pacote, escrever especificação com comportamento atual, contrato desejado, regras verificáveis, casos de erro, critérios de aceite e testes de regressão. Não assumir comportamento não demonstrado no código.
