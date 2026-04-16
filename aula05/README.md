# JavaScript e HTMX
~~~html
<!DOCTYPE html>
<html lang="ptbr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript</title>
</head>
<body>
    <ul id="cursos"></ul>
    <script>
        const somar = (a, b) => { // arrow function
            return a + b;
        };
        const cursos = ["Django", "JavaScript", "CSS"]; // lista imutável
        const lista = document.getElementById("cursos"); // pega o id de um elemento HTML (documento); const lista é referência para esse elemento do template; objeto do tipo element
        for (let i = 0; i < cursos.length; i++){ 
            let li = document.createElement("li"); // let: variável que existe apenas dentro de um bloco de código; createElement: cria um elemento HTML do tipo "li" (item de lista)
            li.innerText = cursos[i];
            lista.appendChild(li);
        }
    </script>
</body>
</html>
~~~
## Incluindo JavaScript no template
~~~html
<script>
    console.log("Olá, Mundo!");
</script>
~~~~

## Variáveis e Tipos de Dados
### Declaração
- **var**
- **let**
- **const**

### Tipos primitivos
- **Number**
- **String**
- **Boolean**

### Tipos complexos
- **Object**
- **Array:** Mantém elementos ordenados numericamente

### Condicionais
- **if/else**
> Toma decisões baseadas em condições. Execute diferentes blocos dependendo do resultado.

- **for e while**
> Repete instruções. O for é ideal quando sabemos o número de iterações.

- **switch**
> Avalia múltiplas condições. Alternativa elegante a vários if/else encadeados.

### Funções
#### Função Tradicional
~~~javascript
  function somar(a, b) {
      return a + b;
  }
~~~

#### Função Anônima
~~~javascript
  const somar = function(a, b)
  {
      return a + b;
  };
~~~

#### Arrow Function
- Uma variável recebe parâmetros e retorna o que a função faz 
- Redução de quantidade de código e permite encadeamento de chamadas
~~~javascript
  const somar = (a, b) => {
      return a + b;
  };
~~~

## Orientação À Objetos
~~~javascript
class Pessoa {
  constructor(nome, idade){
      this.nome = nome;
      this.idade = idade;
  }
  apresentar(){
    return 'Olá! Meu nome é ${this.nome} e eu tenho ${this.idade} anos';
  }
}

const pessoa = Pessoa("Sarah", 18);
alert(pessoa.apresentar());
~~~
