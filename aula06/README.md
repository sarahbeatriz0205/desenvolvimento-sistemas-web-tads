# Herança de Templates
## Objetivo 
- Compreender a proposta e como utilizar o mecanismo de herança de templates
- Como usar a estrutura de includes para porcionar o código dos templates

## Herança com Django
- DTL provê um mecanismo de herança de templates
- A proposta é que a construção aconteça sobre algumas definições base (que são comuns)
- São definidos blocos – blocks – que podem ser sobrescritos em extensões específicas
~~~html
<!DOCTYPE html>
  <html>
    <head>
    <title>{% block title %}Título Padrão{% endblock %}</title>
    </head>
    <body>
    {% block content %}
    {% endblock %}
  </body>
</html>
~~~
~~~html
{% extends 'base.html' %}
{% block title %}Minha Página{% endblock %}
{% block content %}
  <h1>Meu conteúdo</h1>
{% endblock %}
~~~

## Extends
- Conceito de herança de templates
- Permite mudança dentro do template onde está herdando a base
- Se não houver mudança, o que prevalece sempre é o conteúdo da "classe" (template) mãe

## Include
- A inclusão permite adicionar outros templates de um template, aumentando o reuso e evitando a duplicação de código comum, seja HTML ou não
- O que prevalece sempre é o conteúdo da "classe" (template) mãe, não se muda nada
~~~html
<!-- Template Principal -->
<body>
  {% include 'components/header.html' %}
  <main>
    Conteúdo principal
  </main>
  {% include 'components/footer.html' %}
</body>
~~~
