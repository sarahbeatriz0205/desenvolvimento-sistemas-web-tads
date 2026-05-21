# Estrutura dos Models
## Objetivo
- **Compreender a infraestrutura disponibilizada pelo Django para as classes de modelo**
- **Entender como definir classes de modelo, seus atributos e seus relacionamentos**

## Tópicos
- **Proposta do Django para as classes de modelo**
- **Definição de atributos, seus tipos e configurações**
- **Alternativas de herança de modelo**
- **Relacionamentos entre classes de modelos**
- **Metadados de modelo e “gerente” de entidades**

## Classes de Modelo
- **Representam classes de domínio (conceitos, comportamento, conhecimento)**
- **Possibilitam a simplificação de vários aspectos associados em uma aplicação. Com recursos disponibilizados pelo Django, ele entende que um atributo do modelo vai virar um formulário, por exemplo**

### Armazenamento Persistente
- **O padrão de persistência do Django é o banco de dados relacional**
- **Toda classe de modelo deve herdar de models.Model**
- 
~~~bash
django.bd.models.Model
~~~

> [!TIP]
> O modelo Python abaixo gera, pela ORM do Django, o SQL mais abaixo

~~~python
class TipoAnimal(models.Model):
  nome = models.CharField(max_length=50)

  def __str__(self):
    return self.nome 
~~~
~~~sql
CREATE TABLE "animais_tipoanimal" ("id" integer NOT
NULL PRIMARY KEY AUTOINCREMENT, "nome" varchar(50) NOT NULL);
~~~
- Possível tabela gerada por esse SQL:
  - Nome da tabela: nome da aplicação + nome da classe
  - Campo “id” é acrescentado automaticamente
  - Autoincrementa o "id

## Atributos de Classes de Modelo
- **Definição de atributos é obrigatória**
~~~python
class Animal(models.Model):
  nome = models.CharField(max_length=100)
  data_nascimento = models.DateField()
  sexo = models.CharField(max_length=1,choices=[('M','Masculino'),('F','Feminino')])
  cor = models.CharField(max_length=50)
  raca = models.ForeignKey(Raca, on_delete=models.CASCADE,related_name='animais')
  descricao = models.TextField(blank=True,null=True)
  disponivel = models.BooleanField(default=True,null=True)
  foto = models.ImageField(upload_to='fotos_animais/', blank=True, null=True) # null: indica para o banco que um atributo pode chegar vazio
~~~

## [Tipos de atributo](https://docs.djangoproject.com/pt-br/5.0/ref/models/fields/)
- **Todos os atributos devem estar relacionados à classe Field**

| Tipo de campo | Descrição |
| ------------- | ---------- |
| **AutoField** | Um campo IntegerField que é inserido automaticamente (incrementando o anterior)
| **BinaryField** | Apto a receber uma sequência de bytes
| **CharField** | Apto a receber uma string de médio tamanho |
| **IntegerField** | Apto a receber um inteiro |
| **DateField** | Apto a receber uma data |
| **TimeField** | Apto a receber um horário |

### Image Field
- **É necessário criar uma pasta chamada ```media``` dentro do projeto e configurar o ```settings``` da aplicação para conseguir servir o arquivo**
- **Configuração do ```settings```:**
~~~python
MEDIA_URL = "/media/"
MEDIA_ROOT = BASE_DIR / "media"
~~~

- **É necessário também dizer a rota que serve essas imagens, adicionando as urls do projeto**
~~~python
from django.conf import settings
from django.conf.urls.static import static
...
  if settings.DEBUG:
    urlpatterns+=static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)
~~~

- **Na classe de modelo da aplicação, insira:**
~~~python
foto = models.ImageField(upload_to="animais/media/", blank=True, null=True)
~~~
