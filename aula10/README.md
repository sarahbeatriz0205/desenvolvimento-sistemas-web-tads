# Controle de Acesso
## Parte 1 - Autenticação de Usuários
### Autenticação de Usuários no Django
- **Django provê um sistema de autenticação de usuários**
- **Controla contas de usuários, grupos, permissões e sessões de usuários baseadas em cookies**

### Modelos Django do Sistema de Autenticação
- **Usuários**
- **Grupos: forma genérica de aplicar etiquetas e permissões em mais de um usuário**
- **Permissões: valores booleanos definem se um usuário pode executar determinada tarefa**

### Elementos do Sistema de Autenticação
- **Um sistema hashing configurável de senhas (criptografia)**
- **Formulários e ferramentas de visualização para autenticação de usuários, ou restrição de conteúdo**
- **Um sistema de backend facilmente incorporado**

### Classe de Modelo do Django: User
- **Núcleo do sistema de autenticação**
- **Representam as pessoas interagindo com o sistema**
- **Usados para habilitar coisas como restrições de acesso, registro de perfis de usuários, associar conteúdo com criadores etc.**

### Relacionamento com Outros Modelos
- **Pode ser configurado o relacionamento de User com algum modelo representando usuários do sistema**
~~~python
class Participante(models.Model):
    telefone = models.CharField(max_length=20,verbose_name="Telefone")
    user=models.OneToOneField(User,on_delete=models.CASCADE,blank=True, null=True,related_name='participante')
    nome=models.CharField(max_length=155,verbose_name="Nome")
    foto=models.ImageField(upload_to='participantes/',blank=True,null=True,verbose_name="Foto")

    def __str__(self):
        return self.nome
~~~

### Trabalhando com Herança
- **Pode ser usado como Herança, o que implica, no banco de dados, na criação de uma chave estrangeira no modelo de Usuário**
- **Herança concreta**
~~~python
class Usuario(User):
    nome=models.CharField(max_length=100)
    cpf=models.CharField(max_length=11, unique=True)
    tipo_usuario=models.CharField(max_length=20, choices=[('ADOTANTE','ADOTANTE'),('ABRIGO','ABRIGO')], default='ADOTANTE')
~~~
