# Introdução ao Django e Arquitetura MVT
## Estilo Model - View - Templates
- **Model:** Organização das entidades do projeto, os modelos, conectando-se ao banco de dados e definindo as regras de negócio associada aos dados
- **View:** Controla o fluxo da aplicação, a lógica de processamento do sistemas
- **Templates:** Organização das páginas da aplicação. é a camada resposnável por renderizar o HTML
- *Template no MVT é o View do MVC*

## Django
- Framework de alto nível do Python
- **Batteries included:** Oferece vários recursos para auxiliar na construção da aplicação. Traz autenticação, ORM, formulários, admin, internacionalização, cache, etc.
- **Segurança embutida:** Proteção contra CSRF, XSS, SQL Injection, Clickjacking
- **Escalabilidade e robustez:** Usado em sites de alto tráfego como Instagram e Pinterest.
- **Comunidade ativa:** Extensa documentação, pacotes e suporte.

### Camadas principais
- **Model:** Define a estrutura dos dados e interage com o banco de dados via ORM
- **Templates:** Camada de apresentação (HTML + tags Django)
- **View:** Recebe a requisição do cliente e retorna a resposta. Geralmente é um método com parâmtro "request" que recebe a requisição

### Ciclo de uma requisição
- **REQUEST:** Chega via URLconf
- **View** processa a requisição e acessa o Model se necessário
- Dados são renderizados em um Template
- **Response** é enviada ao cliente
#### Ferramentas adicionais 
- Admin automático
- Migrations
- Middlewares
- REST com Django Rest Framework.
