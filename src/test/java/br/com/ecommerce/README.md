# Testes

## Objetivo e responsabilidade
Testes automatizados do projeto.

## Funcionalidade existente e arquivo crítico
`EcommerceApplicationTests.java`: único método `projectSkeleton()`, contendo `assertTrue(true)`; não exercita aplicação, serviços, persistência ou endpoints.

## Dependências, módulos relacionados e ponto de entrada
JUnit Jupiter via `spring-boot-starter-test`; execução pela fase de testes Maven. Relaciona-se potencialmente com todas as camadas, mas nenhuma delas é testada pelo método atual.

## Fluxos e débitos
Não há testes de integração, autenticação, concorrência, carrinho, pagamento ou controle de acesso no código inspecionado.

## Diretriz SDD
Cada nova especificação deve gerar testes de aceite e regressão antes da implementação.
