# Recursos de configuração

## Objetivo e responsabilidade
Centralizar parâmetros de execução da aplicação.

## Arquivos críticos e funcionalidades
`application.yml`: porta `${PORT:8080}`, datasource PostgreSQL, credenciais via variáveis, `spring.jpa.hibernate.ddl-auto=update`, `open-in-view=false`, Redis e segredo JWT.

## Dependências e módulos relacionados
Consumido pelo Spring Boot e pelos beans em `config`; os serviços dependem de PostgreSQL e Redis.

## Ponto de entrada e fluxo
O Spring Boot lê `application.yml` na inicialização e injeta valores no contexto.

## Débitos e observações
Defaults de credenciais e JWT destinam-se a desenvolvimento; `ddl-auto=update` não substitui migrações versionadas. Não foram encontrados perfis separados ou migrações Flyway/Liquibase.
