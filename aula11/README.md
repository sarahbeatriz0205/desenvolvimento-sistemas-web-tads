# Class Based Views
## Classes de View
- **Django provê o conceito de classes de view, ou class-based views, as quais tratam as requisições**
- **As classes de view herdam de ```views.generic.View```**
- **Exemplos:**
  - **TemplateView – retorna um template específico**
  - **RedirectView – proporciona um redirecionamento HTTP**

- **A maneira mais simples de utilizar uma classe de visão genérica é na definição das URLs ```(urls.py)```**
- **O método ```as_view()``` é a forma padrão de acionar uma classe de view (tipo, encare isso como uma função de view).**
- **Uma classe de view provê os seus serviços através da estrutura de uma classe (atributos e métodos prontos)**
- **Possibilita às views a operarem segundo os princípios da orientação à objetos**

~~~python
# DESSE MODO, MOSTRA UMA PÁGINA ESTÁTICA
# NÃO SERVE PARA PASSAR LÓGICA DE NEGÓCIO QUE EXIGA CONSULTA NO BANCO DE DADOS OU QUALQUER OUTRA LÓGICA MAIS PESADA

urlpatterns=[
    path('',TemplateView.as_view(template_name='adocato/index.html'),name='index'),
]
~~~

- **Django disponibiliza algumas classes de view prontas para serem utilizadas**
- **Tais classes visam encurtar o processo de criação para operações “comuns” de views**

| Classe | Descrição |
| ------- | ------ |
| **django.views.generic.View** | Superclasse de todas as ```Views```. Se usa, no mínimo, para as operações de CRUD|
| **django.views.generic.TemplateView** | Permite o retorno de um template
| **django.views.generic.RedirectView** | Permite o redirecionamento
| **django.views.generic.ArchiveIndexView, django.views.generic.YearArchiveView, django.views.generic.MonthArchiveView, django.views.generic.WeekArchiveView, django.views.generic.DayArchiveView, django.views.generic.TodayArchiveView, django.views.generic.DateDetailView** | Permite a visualização de uma coleção de objetos com atributos com datas específicas
| **django.views.generic.CreateView, django.views.generic.DetailView, django.views.generic.UpdateView, django.views.generic.DeleteView, django.views.generic.ListView, django.views.generic.FormView** | Permite a execução de operações de CRUD, sem a necessidade de realizar explicitamente às consultas aos models
