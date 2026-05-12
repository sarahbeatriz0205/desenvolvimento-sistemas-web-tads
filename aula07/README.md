# Introdução a Modelos
## Objetivos
1. Construir os modelos da aplicação exemplo (Adote-me)
2. Conceituar o segundo elemento conforme o estilo arquitetural adotado pelo Django, as classes de modelo (models)

## Configurando o Banco de Dados
### ORM
- Object Relational Mapper
- Realiza o mapeamento de objetos para banco de dados relacionais
- Praticamente todo o manuseio do banco de dados se dá pelo use de métodos e objetos oferecidos pela biblioteca de mapeamento
- O Django possui seu próprio ORM, porém a linguagem Python tem diversos outras bibliotecas, como SQLAlchemy, por exemplo.
- Sem o ORM, é necessário a montagem de consultas SQL para serem então enviadas ao banco de dados, por intermédio de uma biblioteca específica

<img width="720" height="366" alt="image" src="https://github.com/user-attachments/assets/2b8fb10d-a570-4b07-9a10-74c4d7518134"/><br>

<img width="957" height="686" alt="image" src="https://github.com/user-attachments/assets/9040660e-32ce-4f21-b6c3-2fa13a434544" />

### Passo a passo
#### Passo 1
- Definir a Engine (biblioteca) que eu usarei no banco de dados (SQLite é padrão)
- Definir o nome do meu banco de dados
~~~python
DATABASES = { 
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}
~~~

#### Passo 2
- Realizar as migrações
~~~bash
$ python manage.py migrate
~~~
> [!TIP]
> O comando acima aplica as alterações de estrutura (migrations) pendentes ao banco de dados, sincronizando seus modelos Python com as tabelas SQL. Gera o SQL automaticamente


<img width="919" height="421" alt="image" src="https://github.com/user-attachments/assets/4ff00862-36de-48d3-b3c5-69c5de12d1c6" />

> Existem scripts prontos no Django ao criar uma aplicação. Essa imagem mostra esses scripts sendo migrados

## Criando os Models
- Um model é responsável por abstrair os conceitos do domínio do problema (classes conceituais)

~~~python
# Classes de modelo para o projeto Animais da aplicação Adoteme
# Observações:
    # todas as associações entre um TipoAnimal e Raça são apagadas ao incluir CASCADE no on_delete
    # ex: se existir um tipo de animal cachorro, a(s) raça(s) associadas a esse tipo são apagadas
# related_name: O related_name no Django é um argumento opcional usado em campos de relacionamento (ForeignKey, ManyToManyField, OneToOneField) para definir o nome do relacionamento reverso.
# Ele permite acessar o modelo pai a partir do modelo filho de forma clara, substituindo o padrão modelo_set por um nome personalizado.


from django.db import models

class TipoAnimal(models.Model):
    nome = models.CharField(max_length=50)

    def __str__(self):
        return self.nome

class Raca(models.Model):
    nome = models.CharField(max_length=50)
    tipo_animal = models.ForeignKey(TipoAnimal, on_delete=models.CASCADE,related_name='racas') # atributo do tipo Chave Estrangeira
    

    def __str__(self):
        return f"{self.tipo_animal.nome} ({self.nome})"

class Animal(models.Model):
    nome = models.CharField(max_length=100)
    data_nascimento = models.DateField()
    sexo=models.CharField(max_length=1,choices=[('M','Masculino'),('F','Feminino')])
    cor = models.CharField(max_length=50)
    raca = models.ForeignKey(Raca, on_delete=models.CASCADE,related_name='animais')
    descricao = models.TextField(blank=True,null=True)
    disponivel = models.BooleanField(default=True,null=True)
    def __str__(self):
        return self.nome
    def idade(self):
        from datetime import date
        hoje = date.today()
        idade = hoje.year - self.data_nascimento.year - ((hoje.month, hoje.day) < (self.data_nascimento.month, self.data_nascimento.day))
        return idade
~~~

## Ativando os Models
- Editar o arquivo adocato/settings.py
~~~python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "animais", #indicar que a aplicação existe
    "principal",
    "usuarios",
    "adocao",
]
~~~

- Preparar a migração
~~~bash
python manage.py makemigrations nome_modelo
~~~~
<img width="1025" height="133" alt="image" src="https://github.com/user-attachments/assets/24857d5c-cd90-41c0-b14b-7dc1f5a93444" /><br>


- Migrar
~~~bash
python manage.py migrate
~~~~
<img width="910" height="97" alt="image" src="https://github.com/user-attachments/assets/ce0a4007-6b14-4b01-8b4f-9b89b8ceef55" /><br>

> [!IMPORTANT]
> Se for necessário fazer alguma alteração na estrutura do modelo, o processo de ativação dos models deve ser refeito, tirando a parte da instalação dos projetos da aplicação

> [!NOTE]
> Para ver o SQL correspondente à migração, execute:
~~~bash
python manage.py sqlmigrate nome_model 0001
~~~

## API Gerada pelo Django

> [!TIP]
> Forma de interação com a API pelo shell
~~~bash
python manage.py shell
~~~

- Quando passamos o manage.py, o ambiente do shell é configurado com as informações do projeto
- Podemos interagir com a API de acesso a banco de dados gerada para dar suporte as classes de modelo

> [!TIP]
> O comando abaixo me oferece mais recursos para desenvolver
~~~bash
pip install django_extensions
~~~

> [!TIP]
> Esse comando me permite manipular meus modelos pelo shell. É oferecido pelo **django_extensions** 
~~~bash
python manage.py shell_plus
~~~

> [!TIP]
> Criando um elemento dentro do banco de dados por meio do shell e usando os recursos da ORM do Django

> [!IMPORTANT]
> Esse processo salva o elemento apenas na memória! É necessário chamar o método **.save()** para salvar
<img width="358" height="216" alt="image" src="https://github.com/user-attachments/assets/7f2f3310-e4f6-4e58-bbbb-514f0d5f2407" /><br>

> [!TIP]
> Pegando um dado do banco pela sua chave primária (pk) e também modificando o dado a que essa pk pertence
<img width="481" height="250" alt="image" src="https://github.com/user-attachments/assets/c258b498-a6ed-44cc-8396-87e3760da8c5" /><br>

> [!TIP]
> Criando um filtro de busca que captura os objetos (dados) de raça que contenham a palavra "lata"
~~~bash
animais_lata = Animal.objects.filter(raca__nome__contains='lata')
~~~
