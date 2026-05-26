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
- **objects: realiza o gerenciamento das entidades. Permite acesso a partir das instâncias e não diretamente das classes**
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

## Herança de Modelo
- **Toda classe de modelo é subclasse de** ```django.db.models.Model```

### Alternativas para herança em classes de modelo:
**1. Herdar de uma superclasse de modelo abstrata.**
**2. Herdar a partir de uma outra classe de modelo.**
**3. Utilizar “proxy” de modelos, para alterar o comportamento de um modelo sem afetar seus dados.**
**4. Os exemplos a seguir não usam em sua totalidade o projeto do adocato, mas poderia ser possível**

### Herdar de uma classe abstrata
- Úteis para definir atributos base para uma série de outras classes de modelo
~~~python
class CommonInfo(models.Model):
  name = models.CharField(max_length=100)
  age = models.PositiveIntegerField()

  class Meta: # define que a classe é abstrata e não deve ser criada no banco de dados
    abstract = True

class Student(CommonInfo):
  home_group = models.CharField(max_length=5)
~~~

### Herdar de uma outra Classe de Modelo
- **Cada classes da hierarquia será mapeada em uma tabela específica no banco de dados (multi-table)**
- **A herança gera um link entre a classe “filha” e seus “pais”**
~~~python
class Place(models.Model):
  name = models.CharField(max_length=50)
  address = models.CharField(max_length=50)

class Restaurant(Place):
  server_hot_dogs = models.BooleanField(defaut=False)
  server_pizza = models.BooleanField(defaut=False)
~~~
- **Se um dado “Place” também for um “Restaurant”, a partir um objeto “Place” é possível acessar a instância**

#### Relacionamento 1 para 1:
~~~python
place_pointer = models.OneToOneField(Place, on_delete=models.CASCADE, parent_link=True, primary_key=True,)
~~~

## Modelos de usuário
~~~python
from django.db import models
from django.contrib.auth.models import User

class Usuario(User):
    nome = models.CharField(max_length=100)
    cpf = models.CharField(max_length=11)
    tipo = models.CharField(max_length=20, choices=[('Abrigo', 'Abrigo'), ('Adotante', 'Adotante')]) # choices: quando tiver enum no diagrama de domínio, é bom usar

    def __str__(self):
        return self.nome

class Perfil(models.Model):
    usuario = models.OneToOneField(Usuario, on_delete=models.CASCADE)
    avatar = models.ImageField(upload_to='usuarios/avatares/', null=True, blank=True)
    bio = models.TextField()
    comprovanteEndereco = models.FileField(upload_to='usuarios/comprovantes/', null=True, blank=True)

    def __str__(self):
        return f"Perfil de {self.usuario.nome}"
~~~

> [!TIP]
> **abstract = True** faz com que o modelo não seja criado no banco de dados, mas seus atributos podem refletir em modelos que herdam de uma classe que o contenha. Não permite criar uma instância dessa classe

> [!TIP]
> **verbose_name_plural = "nome_plural"** serve para formatar o nome dos atributos no admin do Django

> [!TIP]
> **proxy = True** faz com que o modelo não seja criado no banco de dados, mas permite a criação de um objeto dela

## Relacionamentos
### Um para muitos
~~~python
class DocumentoAdocao(models.Model):
  processo_adocao = models.ForeignKey(ProcessoAdocao, on_delete=models.CASCADE, related_name='documentos')
  descricao = models.CharField(max_length=100)
  enviadoem = models.DateTimeField(auto_now_add=True)
  arquivo = models.FileField(upload_to='adocao/documentos/')

  def __str__(self):
    return f"Documento: {self.descricao} para
    {self.processo_adocao}
~~~

### Muitos para muitos
**ManyToManyField é feita nos objetos que serão editados em formulário**

<img width="811" height="642" alt="image" src="https://github.com/user-attachments/assets/97d17efb-7712-43a6-b3ee-4e1e47ce394e" />

~~~python
class ProcessoAdocao(models.Model):
    STATUS_CHOICES = [
    ('EM_ANALISE', 'Em Análise'),
    ('APROVADA', 'Aprovada'),
    ('REPROVADA', 'Reprovada'),
    ]
    adotante = models.ForeignKey(Usuario, on_delete=models.CASCADE, related_name='processos_adocao_adotante')
    avaliador = models.ForeignKey(Usuario, on_delete=models.SET_NULL,
    null=True, blank=True, related_name='processos_adocao_avaliador')
    animal = models.ForeignKey(Animal, on_delete=models.CASCADE, related_name='processos_adocao')
    criadaem= models.DateTimeField(auto_now_add=True)
    atualizadaem = models.DateTimeField(auto_now=True)
    status = models.CharField(max_length=20, choices=STATUS_CHOICES, default='EM_ANALISE')
    comentarios=models.ManyToManyField(Usuario, blank=True, through='Comentario',related_name='comentarios_usuario') # through: indica por qual classe ele deve passar para chegar no objetivo

    def __str__(self):
      return f"Processo de Adoção: {self.adotante.nome} - {self.animal.nome} ({self.status})"

# === CLASSE ASSOCIATIVA === #
class Comentario(models.Model):
    processo_adocao = models.ForeignKey(ProcessoAdocao,
    on_delete=models.CASCADE, related_name='comentarios_processo')
    autor = models.ForeignKey(Usuario, on_delete=models.CASCADE,
    related_name='comentarios_autor')
    texto = models.TextField()
    realizadoem = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"Comentário de {self.autor.nome} em {self.realizadoem.strftime('%Y-%m-%d %H:%M:%S')}"
~~~

- **No diagrama acima, existe un relacionamento muitos para muitos entre ProcessoAdocao e Usuario. Nesse relacionamento, existe a classe associativa Comentário, que possui atributos que dependem das classes as quais existe a ligação citada anteriormente. Por isso, é necessário criar a classe associativa do diagrama**
