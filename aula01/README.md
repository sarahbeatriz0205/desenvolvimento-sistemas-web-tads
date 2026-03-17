# Protocolo HTTP - Mecânica de comunicação
- **HTTP:** Protocolo de Transferência de Hipertexto
- Envia a página de marcação (geralmente HTML) para o cliente que requisitou
- Permite a comunicação entre navegador web e servidor

## Características
- **Comunicação padronizada:** Garante que os navegadores e servidores se entendam, padronizando o que é enviado nessa comunicação
- **Transferência de dados:** Habilita o recebimento e o envio de dados
- **Acesso a recursos:** Oferece suporte para que você acesse páginas web, pdfs, imagens, e outros recursos (quaisquer tipos de arquivos)

## Modelo Cliente-Servidor
- O protocolo HTTP opera no modelo cliente/servidor
- Cliente: navegador (para o contexto da disciplina, mas pode ser dispositivo móvel ou outras opções)
- O cliente pede recursos ao servidor e deve ter uma resposta dele

## Exemplo de requisição e resposta HTTP
### Requisição
<img width="491" height="303" alt="image" src="https://github.com/user-attachments/assets/7278a924-247e-4b81-9a6f-d7148b30ff30" />

- Requisição HTTP do tipo GET (requisição simples)
- Envia para o Host (endereço)
- Path: indica o recurso específico que o cliente deseja acessar
- Header: contém metadados que contextualizam o que está sendo pedido na requisição

### Resposta
<img width="487" height="231" alt="image" src="https://github.com/user-attachments/assets/97340085-6efd-4929-81e4-e29a8be28763" />

- Status code 200: Indica que "deu certo", juntamente com o Status message
- Header: contém metadados que contextualizam o que está sendo pedido na requisição
> date: data da requisição e/ou da resposta

> cache-control: max age controla o tempo em que o dado vai ficar no cache

> content-type: tipo do conteúdo

## Métodos HTTP
- **GET:** Obter e solicitar dados de um recurso
- **POST:** Enviar dados para o servidor (formulários, uploads)

## Cabeçalhos HTTP
- **Tipo de conteúdo:** Indica o tipo de dado
- **Codificação de caracteres:** Especifica como os caracteres devem se mostrar (ex: UTF-8)
- **Tempo de expiração:** Controla o tempo em que certos dados ficam no cache

## Cookies e sessões
- **Cookies:** Pequenos arquivos de texto armazenados no navegador do usuário
- **Sessões:** Login em algum sistema significa abertura de sessão. Guarda dados no servidor durante a sessão
