# Módulo `config`

> Documentação baseada nos arquivos presentes no ZIP em 17/11/2026. "Módulo" designa pacote lógico Java; não é módulo Maven independente.

## Objetivo
Configuração transversal.

## Responsabilidade principal
Configura autenticação, autorização HTTP e tratamento padronizado de exceções.

## Funcionalidades existentes
SecurityConfig, ErrorHandler.

## Arquivos críticos
`ErrorHandler.java`, `SecurityConfig.java`.

## Dependências internas e externas
controller, service; Spring Security, Spring Web.

**Dependências observáveis por imports/pacotes:** service.

## Módulos relacionados
Consulte `docs/ARQUITETURA_DO_SISTEMA.md` para o grafo completo. Relação arquitetural: HTTP → controller → service → repository → model/PostgreSQL; Redis é acessado diretamente por AuthService e CartService.

## Pontos de entrada
SecurityConfig.filterChain, ErrorHandler handlers.

## Fluxos importantes
Segurança stateless com JWT HS256, BCrypt e tratamento de ApiException/validação/integridade.

## Observações técnicas e débitos
O matcher /actuator/health é liberado, mas dependência Actuator não aparece no pom.xml; refresh JWT não é restringido por type.

## Diretriz SDD
Antes de alterar este pacote, escrever especificação com comportamento atual, contrato desejado, regras verificáveis, casos de erro, critérios de aceite e testes de regressão. Não assumir comportamento não demonstrado no código.
