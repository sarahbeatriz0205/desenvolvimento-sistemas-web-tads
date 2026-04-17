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

## Seleção e Manipulação de Elementos
- **getElementById():** Seleciona elemento por ID

- **getElementsByClassName():** Seleciona elementos por classe

- **querySelector():** Usa seletores CSS para encontrar elementos

- **innerHTML:** Modifica o conteúdo HTML

- **style:** Altera estilos CSS

## Eventos no JavaScript
### Eventos de Mouse
> click, dblclick, mouseover, mouseout controlam interações com ponteiro.

### Eventos de Formulário
> submit, change, input monitoram alterações em formulários.

### Eventos de Teclado
> keydown, keyup, keypress capturam interações via teclado.

~~~html
<button id="btn">Clique aqui</button>
<script>
    document.getElementById("btn").addEventListenere("click", () => {
        alert("Botão clicado");
    });
</script>
~~~
~~~html
<input type="text" id="campo">
<p id="resultado"></p>
<script>
    document.getElementById("campo").addEventListenere("keyup", () => {
        let valor = document.getElementById("campo").value;
        document.getElementById("resultado").innerText = valor;
    });
</script>
~~~

## Introdução às Requisições Assíncronas
- **Requisição:** JavaScript envia solicitações ao servidor Django sem interromper a navegação.
- **Processamento:**  Servidor processa a requisição e prepara os dados para resposta.
- **Resposta:** Dados retornam ao navegador, geralmente em formato JSON.
- **Atualização:** JavaScript atualiza a página sem recarregamento completo.

<img width="780" height="400" alt="image" src="https://github.com/user-attachments/assets/f1740a76-ca1d-419c-bd3b-61d8cb775646" />

- **Operações Síncronas vs. Assíncronas:** JavaScript executa código de forma assíncrona. Não bloqueia durante operações demoradas.
- **XMLHttpRequest:** Método tradicional para requisições. Ainda usado mas menos elegante.
- **Fetch API:** Interface moderna baseada em Promises. Mais simples e poderosa.
- **Promises:** Representam operações futuras. Permitem manipular resultados quando disponíveis.
