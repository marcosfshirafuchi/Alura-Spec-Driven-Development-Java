# Módulo `model`

> Documentação baseada nos arquivos presentes no ZIP em 17/11/2026. "Módulo" designa pacote lógico Java; não é módulo Maven independente.

## Objetivo
Modelo persistente e enums.

## Responsabilidade principal
Representa usuários, catálogo, pedidos, itens e pagamentos com entidades JPA.

## Funcionalidades existentes
User, Category, Product, PurchaseOrder, OrderItem, Payment, Role, ProductStatus, OrderStatus.

## Arquivos críticos
`Category.java`, `OrderItem.java`, `OrderStatus.java`, `Payment.java`, `Product.java`, `ProductStatus.java`, `PurchaseOrder.java`, `Role.java`, `User.java`.

## Dependências internas e externas
repository, service; Jakarta Persistence e Jackson.

**Dependências observáveis por imports/pacotes:** nenhuma importação explícita entre pacotes internos nesta camada; referências são feitas por outros pacotes.

## Módulos relacionados
Consulte `docs/ARQUITETURA_DO_SISTEMA.md` para o grafo completo. Relação arquitetural: HTTP → controller → service → repository → model/PostgreSQL; Redis é acessado diretamente por AuthService e CartService.

## Pontos de entrada
Entidades @Entity, associações @ManyToOne, @OneToMany e @OneToOne.

## Fluxos importantes
User → PurchaseOrder → OrderItem → Product → Category; Payment → PurchaseOrder.

## Observações técnicas e débitos
Campos públicos mutáveis; tenantId default sem isolamento aplicado; serialização de grafos JPA requer verificação.

## Diretriz SDD
Antes de alterar este pacote, escrever especificação com comportamento atual, contrato desejado, regras verificáveis, casos de erro, critérios de aceite e testes de regressão. Não assumir comportamento não demonstrado no código.
