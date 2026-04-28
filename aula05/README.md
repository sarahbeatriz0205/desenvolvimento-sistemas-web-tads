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
- **Objetivo:** Enviar uma resposta sem recarregar a página

### Processo
- **Requisição:** JavaScript envia solicitações ao servidor Django sem interromper a navegação.
- **Processamento:**  Servidor processa a requisição e prepara os dados para resposta.
- **Resposta:** Dados retornam ao navegador, geralmente em formato JSON.
- **Atualização:** JavaScript atualiza a página sem recarregamento completo.

<img width="780" height="400" alt="image" src="https://github.com/user-attachments/assets/f1740a76-ca1d-419c-bd3b-61d8cb775646" />



- **Operações Síncronas vs. Assíncronas:** JavaScript executa código de forma assíncrona. Não bloqueia durante operações demoradas.
- **XMLHttpRequest:** Método tradicional para requisições. Ainda usado mas menos elegante.
- **Fetch API:** Interface moderna baseada em Promises. Mais simples e poderosa.
- **Promises:** Representam operações futuras. Permitem manipular resultados quando disponíveis.

# Fetch API
- A Fetch API no JavaScript permite realizar requisições HTTP assíncronas para carregar dados de APIs de forma simples usando Promises
- A estrutura básica usa fetch(url) seguida de .then() para processar a resposta.
- Promises: São objetos JavaScript que representam o sucesso ou a falha de uma requisição HTTP assíncrona.
- O método fetch() inicia a busca de um recurso e retorna imediatamente uma Promise, permitindo que o código continue executando enquanto aguarda a resposta (pendente), evitando travamentos

> Endpoint: Uma URL específica em uma API que recebe/envia dados
~~~javascript
// Chamadas encadeadas
fetch('https://api.exemplo.com/dados') // retorna um obj do tipo fetch que tem por natureza o método .then
  .then(response => response.json()) // retorna um obj do tipo fetch
  .then(data => console.log(data))
  .catch(error => console.error('Erro:', error));
~~~

<img width="1400" height="864" alt="image" src="https://github.com/user-attachments/assets/ba4668b6-4c51-4d2a-84a5-471894e5884b" />

#### Método para requisição que devolve HTML
~~~javascript
function carregarResultadoIMC(event) {
    event.preventDefault(); // Impede o envio do formulário padrão, senão ele geraria um reload da página
    const url_requisicao_post = document.getElementById('url_requisicao').dataset.url_post //Captura qual é a URL que deve ser requisitada
    // vai receber a url e um dict com as informações da requisição, como método, cabeçalho e corpo da requisição
    fetch(url_requisicao_post,{ // Usando a API fetch para fazer a requisição POST, colocando o cabecalho correto e o corpo da requisição
        method: 'POST', // tipo do método
        headers: { //Cabeçalho da requisição com o tipo de conteúdo e o token CSRF para segurança
            "Content-Type": "application/json", // especifica o tipo conteúdo da requisição
            "X-CSRFToken": document.querySelector("[name=csrfmiddlewaretoken]").value // captura o token CRSF que está no HTML. Conteúdo obrigatório
        },
        // body: conteúdo do forms, nesse contexto
        body: JSON.stringify({ //O JSON.stringify converte o objeto JavaScript em uma string JSON, a partir dos valores dos campos do formulário
            peso: document.getElementById('peso').value,
            altura: document.getElementById('altura').value
        })
    })
    // quando o fetch() for concluído, retorne a resposta
        .then(response => response.text()) // O then espera a resposta do servidor e quando ela chegar, converte para texto
        .then(data => { // O segundo then recebe o texto retornado e atualiza o conteúdo do elemento com id 'resultado_imc'
            document.getElementById('resultado').innerHTML = data; // data: dados recebidos
            console.log(data);
        })
        .catch(error => { // Caso ocorra algum erro na requisição, ele será capturado aqui
            console.error('Erro ao carregar resultado IMC:', error);
            window.alert("Ocorreu um erro ao calcular o IMC. Por favor, tente novamente.",error);
        });
}
~~~


#### Fluxo do código
~~~mermaid
graph TD
    A[Usuário clica no botão Enviar] --> B[event.preventDefault]
    B --> C[Captura URL do dataset]
    C --> D[Captura Valores: Peso, Altura e Token CSRF]
    
    subgraph Fetch_API [Requisição HTTP POST]
        D --> E[JSON.stringify: Transforma dados em String]
        E --> F[Envia Fetch para o Servidor]
        F --> G{Servidor Responde?}
    end

    G -- Sucesso (200 OK) --> H[.then: Converte resposta para texto]
    H --> I[Atualiza innerHTML do #resultado_imc]
    I --> J[Fim do Processo]

    G -- Falha (Erro de Rede/Servidor) --> K[.catch: Captura Erro]
    K --> L[Exibe Alerta de Erro e Console.error]
~~~

## HTMX + Django
- O HTMX é uma biblioteca JavaScript leve (~14kb) que permite adicionar comportamentos dinâmicos diretamente no HTML, usando atributos especiais
- Tecnologia utilizada do lado do cliente

>[!TIP]
> O servidor retorna HTML pronto. O HTMX injeta diretamente no elemento alvo — sem JavaScript extra
~~~html
<button
 hx-get="/lista/" -> acessa essa view por essa rota
 hx-target="#lista"> -> entrega o HTML renderizado nessa view
 Carregar
</button>
~~~

- HTMX intercepta eventos do usuário (clique, digitação, envio de forms) e faz requisições HTTP
- A resposta é HTML puro que é inserido diretamente na página

<img width="677" height="374" alt="image" src="https://github.com/user-attachments/assets/6c12867f-246f-4378-8307-7c0612db8017" /><br>
> Exemplo visual

### Tags HTMX
- **hx-get:** Faz uma requisição GET quando o elemento é acionado
- **hx-post:** Envia um formulário via POST sem recarregar a página
- **hx-target:** Define qual elemento HTML receberá o conteúdo retornado
- **hx-swap:** Controla como o conteúdo é inserido no elemento alvo
- **hx-trigger:** Define qual evento dispara a requisição (padrão: click)
- **hx-indicator:** Mostra um indicador de carregamento durante a requisição
