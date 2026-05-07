# Introdução a Modelos
## Objetivos
1. Construir os modelos da aplicação exemplo(adocato)
2. Conceituar o segundo elemento conforme o estilo arquitetural adotado pelo Django, as classes de modelo (models)

## Configurando o Banco de Dados
### ORM
- Object Relational Mapper
- Realiza o mapeamento de objetos para banco de dados relacionais
- Praticamente todo o manuseio do banco de dados se dá pelo use de métodos e objetos oferecidos pela biblioteca de mapeamento
- O Django possui seu próprio ORM, porém a linguagem Python tem diversos outras bibliotecas, como SQLAlchemy, por exemplo.
- Sem o ORM, é necessário a montagem de consultas SQL para serem então enviadas ao banco de dados, por intermédio de uma biblioteca específica
