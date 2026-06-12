# Validações e QuerySets
## Parte 1: Ajustando e Consultando Classes de Modelo
### Restrições de Nomes de Atributos
**1. Não pode utilizar palavras reservadas no Python**

<img width="1285" height="118" alt="image" src="https://github.com/user-attachments/assets/fa9b8ad6-420f-45a0-a364-4fd4e89ce100" />

**2. Não pode conter mais de um “sublinha”**
  - **Sintaxe utilizada pelo Django nas consultas**

<img width="1252" height="161" alt="image" src="https://github.com/user-attachments/assets/6d914143-891a-4edb-b442-51c509409c68" />

**3. Não pode finalizar com um “sublinha”**

### Métodos de Classes de Modelo
- **As classes de modelo também devem ter a definição dos métodos ligados à lógica do negócio, quando couber.**
- **Se o método envolve várias outras classes, então a implementação deve ficar junto as classes de serviço.**
- **Estratégia → definição em uma única classe:**
  - **(i) das informações relevantes ao domínio e**
  - **(ii) da lógica do negócio associada, desde que esteja restrito a um conjunto pequeno de classes associadas.**

~~~python
class ProcessoAdocao(models.Model):
...
def atrasada(self):
    if self.status == 'EM_ANALISE':
    dias_em_analise = (datetime.date.today() - self.criadaem).days
        return dias_em_analise > 7
    return False
~~~

> [!TIP]
> Todos os métodos herdados podem ser sobrescritos. Podem ser sobrescritos os métodos save() e delete(), se necessário

~~~python
class Raca(models.Model):
...

# É possível até mesmo impedir o salvamento de dados específicos, como no exemplo abaixo
def save(self, *args, **kwargs):
    if len(self.nome) < 3:
        raise ValidationError('O nome da raça deve ter pelo menos 3 caracteres.')
    return super().save(*args, **kwargs)
# Porém, quando se trata de validação, essa não é a técnica mais adequada
~~~

### Validação a nível de modelo
**Passos para validação de um objeto(instância)**
- **Validar os atributos (Model.clean_fields)**
- **Validar o modelo como um todo (Model.clean())**
- **Validar unicidade de campos(Model.validate_unique())**
- **Validar restrições(ex: chave estrangeira) (Model.validate_constraints())**
---
- **Todas essas validações podem ser invocadas com um único método chamado full_clean, normalmente chamado antes do save.**
- **A invocação da chamada deve ser feita de forma explícita, caso contrário, ela é ignorada. A alternativa seria chamar o método dentro do método save**
- **Caso seja detectado algum problema é gerada uma exceção do tipo ValidationError**
- **A customização de validação deve ser feira através da sobrescrita do método clean**

~~~python
from django.core.exceptions import ValidationError

class Animal(models.Model):
    def clean(self):
    erros = {}
    if not self.nome:
        erros['nome'] = 'O nome do animal é obrigatório.'
    elif len(self.nome) < 3:
        erros['nome'] = 'O nome do animal deve ter pelo menos 3 caracteres.'
    if self.data_nascimento is None:
        erros['data_nascimento'] = 'A data de nascimento é obrigatória.'
    elif self.data_nascimento > date.today():
        erros['data_nascimento'] = 'A data de nascimento não pode ser no futuro.'
    if self.sexo not in ['M', 'F']:
        erros['sexo'] = 'O sexo deve ser "M" para masculino ou "F" para feminino.'
    if not self.cor:
        erros['cor'] = 'A cor do animal é obrigatória.'
    elif len(self.cor) < 3:
        erros['cor'] = 'A cor do animal deve ter pelo menos 3 caracteres.'
    if self.raca is None:
        erros['raca'] = 'A raça do animal é obrigatória.'
    if erros:
        raise ValidationError(erros)
~~~

- **Chamada do método de clean dentro de um método da classe de serviço**
~~~python
def cadastrar_animal(nome, data_nascimento, sexo, cor, raca_id, descricao=None, disponivel=True, foto=None):
        animal = Animal(
            nome=nome,
            data_nascimento=data_nascimento,
            sexo=sexo,
            cor=cor,
            raca_id=raca_id,
            descricao=descricao,
            disponivel=disponivel,
            foto=foto
        )
        try:
            animal.full_clean()  # Valida os dados do modelo. Invoca essa validação que vem do método clean acima
        except ValidationError as e:
            raise e 
        animal.save()
        return animal
~~~
