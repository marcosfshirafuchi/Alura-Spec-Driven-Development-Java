# Objetivo do Sistema

**Base:** código de `ecommerce-springboot` 1.0.0 | **Análise:** 17/11/2026

## 1. Propósito principal
Disponibilizar uma API HTTP para cadastro/autenticação de usuários, administração de catálogo, carrinho, criação de pedidos e registro interno de estados de pagamento. A descrição é sustentada pelos controllers e services presentes.

## 2. Problemas que resolve
- Centraliza cadastro de usuários e autenticação JWT.
- Permite consulta pública de categorias/produtos e manutenção administrativa do catálogo.
- Mantém carrinho por usuário em Redis.
- Cria pedidos a partir do carrinho, verificando disponibilidade e reduzindo estoque.
- Registra confirmação e cancelamento internos de pagamentos e pedidos.

## 3. Atores envolvidos
- **Visitante:** pode registrar-se, fazer login e consultar catálogo.
- **Usuário autenticado (USER):** pode gerenciar próprio perfil, carrinho, pedidos e pagamentos associados.
- **Administrador (ADMIN):** pode criar, alterar e excluir categorias/produtos; também pode acessar perfis de outros usuários via `UserService`.
- **Sistemas de infraestrutura:** PostgreSQL e Redis (não atores humanos).

## 4. Principais fluxos de negócio
### Registro e login
`POST /api/v1/auth/register` cria usuário com senha BCrypt e retorna tokens; `POST /api/v1/auth/login` valida credenciais e retorna tokens. Refresh token é armazenado em Redis, mas não há endpoint de renovação implementado.

### Catálogo
`GET /api/v1/categories[/{id}]`, `GET /api/v1/products[/{id}]` públicos; POST/PUT/DELETE de categorias e produtos restritos a ADMIN. Produtos são filtrados por nome/categoria e paginados em memória.

### Carrinho e checkout
`GET /api/v1/cart`, `POST /api/v1/cart/items`, `DELETE /api/v1/cart/items/{itemId}`. `POST /api/v1/orders` cria pedido com itens do carrinho, valida estoque, reduz quantidades, grava pedido e limpa Redis. `GET /api/v1/orders` lista pedidos do usuário.

### Pagamentos
`POST /api/v1/payments` cria ou reutiliza registro por pedido; `POST /api/v1/payments/{id}/confirm` marca pagamento/pedido como pagos; `POST /api/v1/payments/{id}/cancel` cancela e tenta recompor estoque. Não há integração de cobrança externa identificada.

### Perfil
`GET`, `PUT` e `DELETE /api/v1/users/{id}` com controle por identidade ou papel ADMIN.

## 5. Funcionalidades centrais
| Domínio | Implementação | Persistência |
|---|---|---|
| Identidade | AuthService, UserService | PostgreSQL + Redis para refresh |
| Catálogo | CatalogService | PostgreSQL |
| Carrinho | CartService | Redis |
| Pedidos | OrderService | PostgreSQL; consulta/limpeza do Redis |
| Pagamentos | PaymentService | PostgreSQL |

## 6. Visão de produto
**Estado comprovado:** backend de e-commerce com API REST e operações básicas de catálogo, compra e pagamento interno. Não há frontend, checkout em gateway de pagamento, expedição, emissão fiscal ou envio de e-mails no ZIP.

**Hipótese:** a aplicação pode servir como base de estudo/protótipo para um e-commerce mais completo; não há documentação de requisitos comerciais ou métricas de produto que confirme uma finalidade de produção.

## 7. Contexto operacional
Aplicação Java 21 / Spring Boot 3.5.6; API padrão na porta 8080; PostgreSQL na 5432 e Redis na 6379 por padrão; `docker-compose.yml` fornece os dois bancos; Maven ou Dockerfile executam a API. JWT depende de segredo configurado; configuração padrão é de desenvolvimento.

## 8. Restrições e pontos a especificar em SDD
- Critérios de aceite de autenticação, expiração/renovação e permissões.
- Regras de concorrência, reserva/baixa e estorno de estoque.
- Idempotência efetiva, integração financeira e transições de estado de pagamento.
- Contratos de DTO e tratamento de erros.
- Escopo de tenant: atualmente `default` sem isolamento aplicado.
- Cenários de falha entre Redis e PostgreSQL.

**Fonte de verdade:** código-fonte atual. Qualquer comportamento futuro deve ser especificado, implementado e testado antes de ser descrito como existente.
