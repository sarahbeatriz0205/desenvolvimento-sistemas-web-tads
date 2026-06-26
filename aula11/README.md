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

## Aplicando decoradores
- **Para decorar as instância de classes de view, as decorações precisam ser aplicadas à classe**
- **O método que deve ser decorado é o ```dispatch()```**

- **Aplicando @method_decorator ao método ```dispatch()```**
<img width="1237" height="537" alt="image" src="https://github.com/user-attachments/assets/1108f75c-77d6-4d36-957c-f9f53b5b5257" />


- **O decorador pode ser aplicado à classe, identificando o nome do método ao qual se aplica**
<img width="1227" height="178" alt="Captura de tela 2026-06-26 092555" src="https://github.com/user-attachments/assets/df35b86b-2f67-4b1a-a5ee-bb5f2cee9cc3" />

## Uso de Mixins para autorização e autenticação
- **Mixins são classes do Django que permitem adicionar/modificar recursos oferecidos por uma class based view**
- **Essa adição/modificação se dá por meio de herança (polimorfismo)**

~~~python
class CoordenadorMixin(UserPassesTestMixin):
    raise_exception = True
    def test_func(self):
        coordenador=CoordenadorService.obter_coordenador_por_id(self.request.user.id)
        return coordenador is not None
~~~
