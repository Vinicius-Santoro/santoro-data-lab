# **Fundamentos de APIs**

## Objetivo

Entender o que é uma API, como funciona e quais são seus tipos e padrões.

## 1. O que é uma API?

### 1.1 Introdução

Uma API (Application Programming Interface) é um conjunto de regras que permite
que diferentes sistemas se comuniquem entre si de maneira estruturada e
previsível. Em termos simples, uma API atua como uma ponte que conecta
aplicações, possibilitando a troca de dados e funcionalidades sem que seja
necessário conhecer a implementação interna de cada sistema.

Segundo o livro "Designing Web APIs" (Brenda Jin, Saurabh Sahni, Amir Shevat,
2018), uma API bem projetada é como uma "promessa de serviço" que define
exatamente o que pode ser requisitado, o que será entregue e em quais condições. 

A importância das APIs cresceu com a transformação digital das empresas: elas
são a base da comunicação entre serviços em nuvem, da integração entre
parceiros de negócios e da inovação de produtos. Sem APIs, seria inviável
escalar sistemas complexos como redes sociais, plataformas de pagamento,
marketplaces e serviços de streaming.

Em resumo, APIs são fundamentais para:

- Conectar sistemas de forma eficiente e segura;
- Acelerar o desenvolvimento de software;
- Permitir integrações com parceiros e terceiros;
- Potencializar a inovação digital e a criação de novos negócios.

## 2. Tipos de API

### 2.1 REST

API REST (Representational State Transfer) é o tipo de API mais usado
atualmente.
Ela funciona como um cardápio de internet: você faz um pedido (requisição)
para um endereço (URL), e o servidor te entrega exatamente aquilo que você
pediu, normalmente em formato JSON.

- Usa os métodos padrão da internet: GET, POST, PUT, DELETE.
- Cada URL representa um recurso (ex.: /usuarios para usuários, /produtos para
produtos)
- É simples, rápida e fácil de entender.

**Exemplo:** Quando você entra num app para ver suas mensagens, o app faz um
GET para /mensagens, e recebe a lista.

### 2.2 SOAP
SOAP (Simple Object Access Protocol) é um tipo de API mais antigo e mais formal.
Ela funciona como uma troca de cartas muito bem organizada: você sempre envia
uma mensagem seguindo regras super específicas, usando o formato XML.

- Usa um padrão bem rígido de mensagens e respostas.
- É mais seguro e recomendado em sistemas bancários, de saúde e onde a
segurança é prioridade.
- Mais "pesado" e difícil de configurar do que REST.

**Exemplo:** Bancos ainda usam SOAP para transferências eletrônicas porque ele
garante que toda a comunicação siga normas de segurança rigorosas.

### 2.3 GraphQL
GraphQL é uma tecnologia mais moderna criada pelo Facebook.
Ela funciona como um cardápio à la carte: você pede exatamente o que quer e só
recebe aquilo, nem mais, nem menos.

- Você envia uma única requisição dizendo quais campos de dados precisa.
- Evita receber dados desnecessários ou ter que fazer várias requisições
diferentes.
- Muito útil em aplicativos com interfaces ricas, como redes sociais.

**Exemplo:** Em vez de buscar todos os dados de um usuário, você pede só o nome
e o e-mail, se for isso que precisa.

!!! Question "Pergunta"

    **Em APIs GraphQL, eu peço e recebo exatamente os dados que solicitei. No
    entanto, em APIs REST também é possível filtrar o que é retornado com
    query parameters. Qual é então a diferença?**

    Em APIs REST, você consegue sim usar query parameters para filtrar os dados
    que quer buscar, mas é diferente do que acontece em GraphQL.

    No REST, você usa query parameters para:

    - Filtrar resultados (ex.: buscar usuários de uma certa cidade)
    - Ordenar (ex.: por data de criação)
    - Paginar (ex.: trazer só 10 resultados por vez)

    **Exemplo:**
    
    ```GET /usuarios?cidade=SãoPaulo&limite=10```

    Esse exemplo pega só usuários da cidade de São Paulo, limitando a 10
    resultados. **Mas atenção:** o que você não consegue controlar no REST
    (normalmente) é quais campos de cada usuário vão vir.

    Ou seja, vem o objeto inteiro (nome, e-mail, telefone, endereço...) mesmo
    que você só queira o nome e o telefone.
    No GraphQL, você controla o formato da resposta. Você diz na requisição:
    "Me envie apenas o `nome` e o `telefone`", e a resposta vem só com isso.

    <table>
    <tr>
        <th>Característica</th>
        <th>REST (com query params)</th>
        <th>GraphQL</th>
    </tr>
    <tr>
        <td>Filtrar dados</td>
        <td>✅ Sim</td>
        <td>✅ Sim</td>
    </tr>
    <tr>
        <td>Escolher quais campos da resposta</td>
        <td>🚫 Não (vem padrão)</td>
        <td>✅ Sim (você escolhe)</td>
    </tr>
    <tr>
        <td>Flexibilidade na resposta</td>
        <td>Média</td>
        <td>Alta</td>
    </tr>
    </table>


### 2.4 WebSockets
WebSockets é um tipo especial de comunicação em tempo real.
É como se você abrisse um canal direto entre o cliente (ex.: seu navegador) e o
servidor: os dois podem mandar mensagens a qualquer momento, sem ter que ficar
"pedindo" toda hora.

- Ideal para chats, jogos online, notificações em tempo real.
- Permite troca de dados instantânea e contínua.
- Diferente do REST que precisa de uma requisição para cada resposta.

**Exemplo:** Em um app de chat, quando alguém te manda uma mensagem, ela
aparece na tela na hora sem você precisar atualizar a página.

!!! Question "Pergunta"

    **Como funciona a comunicação em tempo real via WebSocket e qual sua
    relação com Kafka e SQS?**

    - **WebSocket** é um protocolo que mantém uma conexão aberta entre o
    cliente e o servidor, permitindo comunicação em tempo real. Ele é ideal
    para chats, notificações instantâneas e jogos online.

    - **Kafka e SQS** são ferramentas de mensageria usadas para comunicação
    entre sistemas no backend, permitindo que serviços diferentes troquem
    mensagens de forma assíncrona.

    - **Integração:** Enquanto o WebSocket permite a comunicação em tempo real
    com o cliente, Kafka e SQS são usados para gerenciar e processar eventos e
    mensagens no backend, facilitando a escalabilidade e integração entre
    sistemas.

## 3. Componentes básicos

### 3.1 Endpoints

No contexto das APIs, um endpoint é uma URL específica que define um ponto de
acesso onde uma requisição HTTP pode ser feita para interagir com o sistema.
Cada endpoint está associado a uma ação específica ou a um recurso dentro da
API, como obter ou enviar dados.

Cada endpoint é geralmente mapeado para uma operação HTTP:

- GET: Obtém dados.
- POST: Cria novos dados.
- PUT: Atualiza dados existentes.
- DELETE: Remove dados.

**Exemplo:**

- ```GET https://api.exemplo.com/usuarios/123```: Retorna os dados do usuário
com ID 123.
- ```POST https://api.exemplo.com/usuarios```: Cria um novo usuário.

### 3.1 Rotas

A rota é o caminho ou direção que a requisição segue dentro do servidor para
chegar a um determinado recurso. 

**Exemplo:** Em uma API de usuários, a rota pode ser definida como `/usuarios`,
ou `/usuarios/{id}`.

!!! Question "Pergunta"

    **Qual a diferença de endpoint e rota?**

    A diferença entre endpoint e rota é sutil, mas fundamental para entender o
    funcionamento de uma API.

    - **Rota:** Refere-se ao caminho ou URL que a requisição segue para acessar
    um recurso.
    - **Endpoint:** É a combinação do método HTTP (como GET, POST, etc.) com a
    rota, ou seja, é o ponto de acesso completo que define a operação que será
    executada.

    **Exemplo:**

    - Rota: `/usuarios`
    - Endpoint:
        - `GET /usuarios`: Obtém a lista de usuários.
        - `POST /usuarios`: Cria um novo usuário.

#### 3.1.2 Boas práticas

<table>
  <tr>
    <th>Prática</th>
    <th>Descrição</th>
  </tr>
  <tr>
    <td>Use Substantivos no Plural para Recursos</td>
    <td>Utilize nomes no plural para representar recursos
    (ex: <code>/usuarios</code> em vez de <code>/usuario</code>).</td>
  </tr>
  <tr>
    <td>Mantenha URLs Simples e Descritivas</td>
    <td>As rotas devem ser intuitivas e refletir claramente o recurso
    (ex: <code>/produtos</code>, <code>/clientes/{id}</code>).</td>
  </tr>
  <tr>
    <td>Use Métodos HTTP Corretos</td>
    <td>Use <code>GET</code> para recuperar dados, <code>POST</code> para criar,
    <code>PUT</code>/<code>PATCH</code> para atualizar e <code>DELETE</code>
    para remover recursos.</td>
  </tr>
  <tr>
    <td>Use Níveis Hierárquicos para Relações</td>
    <td>Use estrutura hierárquica para indicar relacionamento entre recursos
    (ex: <code>/usuarios/{usuarioId}/pedidos</code>).</td>
  </tr>
  <tr>
    <td>Evite Verbos nas Rotas</td>
    <td>Não coloque verbos nas rotas, pois os métodos HTTP já indicam a ação
    (ex: <code>/usuarios</code> e não <code>/getUsuarios</code>).</td>
  </tr>
  <tr>
    <td>Use Query Parameters para Filtros</td>
    <td>Utilize query parameters para filtros, ordenação ou paginação
    (ex: <code>/usuarios?idade=30&ordenar=nome</code>).</td>
  </tr>
  <tr>
    <td>Evite Uso Excessivo de Sub-Rotas</td>
    <td>Mantenha as rotas simples e evite profundidade excessiva
    (ex: <code>/usuarios/{usuarioId}/enderecos</code> e não
    <code>/usuarios/{usuarioId}/endereco/{enderecoId}/telefone</code>).</td>
  </tr>
  <tr>
    <td>Use Convenções Padrão de API RESTful</td>
    <td>Mantenha consistência nas convenções de nomes e design de rotas
    (ex: <code>/produtos</code>, <code>/produtos/{id}</code>).</td>
  </tr>
  <tr>
    <td>Retorne Códigos de Status HTTP Adequados</td>
    <td>Utilize códigos de status HTTP para indicar sucesso ou falha
    (ex: <code>200 OK</code>, <code>404 Not Found</code>,
    <code>500 Internal Server Error</code>).</td>
  </tr>
  <tr>
    <td>Documente as Rotas</td>
    <td>Documente detalhadamente cada rota, incluindo métodos, parâmetros e
    tipos de resposta.</td>
  </tr>
</table>

## 4. Query e Path Parameters

Query Parameters são usados para filtrar, ordenar ou paginar os dados da API.
Eles são adicionados após o ponto de interrogação (?) na URL e seguem o formato
key=value.

**Exemplo:**

`/produtos?categoria=eletronicos&page=2`

Esses parâmetros não são necessários para a identificação do recurso, mas
ajudam a personalizar a resposta da API.

Path Parameters são usados para identificar recursos específicos na URL, sendo
uma parte essencial da rota. Eles são definidos diretamente no caminho da URL,
entre chaves, como em `/usuarios/{id}` ou `/produtos/{produtoId}/detalhes`.

Path parameters são obrigatórios para acessar um recurso específico e não são
opcionais como os query parameters, sendo parte da estrutura da URL.

## 5. Métodos HTTP

### 5.1 GET

**Descrição:**
Recupera dados de um servidor. É somente leitura, não altera o estado do
servidor.

**Uso típico:**
Buscar informações, como uma lista de usuários ou detalhes de um produto.

**Exemplo:**
```json
GET /api/usuarios/123 HTTP/1.1
```

!!! Question "Pergunta"

    **O que significa esse HTTP/1.1?**

    O trecho HTTP/1.1 indica a versão do protocolo HTTP que o cliente
    (por exemplo, um  navegador ou outro sistema) está usando para fazer a
    requisição.

    **Resumindo:**

    - HTTP → é o protocolo usado para comunicação entre cliente e servidor.
    - 1.1 → é a versão do protocolo.
        - HTTP/1.0 foi uma versão anterior (mais simples, menos eficiente).
        - HTTP/1.1 trouxe melhorias importantes, como reutilização de conexões
        (keep-alive), pipelines de requisições e melhor suporte a caches.
        - Depois surgiram HTTP/2 (muito mais rápido, com requisições
        multiplexadas) e HTTP/3 (baseado em QUIC).

    **De maneira bem direta:**

    HTTP/1.1 aqui informa para o servidor:

    "Oi, estou enviando uma requisição usando as regras do HTTP versão 1.1!"

### 5.2 POST

**Descrição:**
Envia dados para criar um novo recurso no servidor.

**Uso típico:**
Cadastro de um novo usuário, criação de pedidos, envio de formulários.

**Exemplo:**
```json
POST /api/usuarios HTTP/1.1
Host: exemplo.com
Content-Type: application/json

{
  "nome": "Vinicius",
  "email": "vinicius@exemplo.com"
}

```

### 5.3 PUT

**Descrição:**
Atualiza um recurso **por completo** no servidor. Você manda todos os dados do recurso, mesmo os que não mudaram.

**Uso típico:**
Substituir um objeto inteiro, como atualizar todo o cadastro de um usuário.

**Exemplo:**
```json
PUT /api/usuarios/123 HTTP/1.1
Host: exemplo.com
Content-Type: application/json

{
  "nome": "Vinicius Silva",
  "email": "vinicius@exemplo.com"
}
```

### 5.4 PATCH

**Descrição:**
Atualiza **parcialmente** um recurso (só os campos que mudaram).

**Uso típico:**
Alterar só o nome de um usuário, sem alterar o email, por exemplo.

**Exemplo:**
```json
PATCH /api/usuarios/123 HTTP/1.1
Host: exemplo.com
Content-Type: application/json

{
  "nome": "Vinicius S."
}
```

### 5.5 DELETE

**Descrição:**
Remove um recurso no servidor.

**Uso típico:**
Excluir usuários, posts, produtos etc.

**Exemplo:**
```json
DELETE /api/usuarios/123 HTTP/1.1
Host: exemplo.com
```

### 5.6 HEAD

**Descrição:**
Igual ao GET, **mas só traz os headers, sem o corpo** da resposta. Útil para saber se o recurso existe ou verificar metadados (tipo tamanho, tipo de arquivo).

**Uso típico:**
Checar se uma imagem existe sem baixar.

**Exemplo:**
```json
HEAD /api/imagem/123 HTTP/1.1
Host: exemplo.com
```

### 5.7 OPTIONS

**Descrição:**
Retorna os métodos HTTP suportados para um recurso (GET, POST, PUT, etc).

**Uso típico:**
Em CORS (Cross-Origin Resource Sharing) para saber se é permitido acessar aquele recurso.

**Exemplo:**
```json
OPTIONS /api/usuarios HTTP/1.1
Host: exemplo.com
```

**Resposta possível:**
```json
Allow: GET, POST, OPTIONS
```

!!! Question "Pergunta"

    **O que seria esse CORS?**

    CORS (Cross-Origin Resource Sharing) é um mecanismo de segurança
    implementado pelos navegadores que controla como os recursos de uma
    página web podem ser acessados a partir de domínios diferentes (ou seja,
    de origens diferentes).

    **O que significa "origem" no contexto de CORS?**

    A "origem" é composta pelo ==protocolo (HTTP, HTTPS)==, o domínio
    (ex: exemplo.com) e a porta (ex: :8080). Por exemplo, `https://exemplo.com`
    tem uma origem diferente de `http://outroexemplo.com` ou
    `https://exemplo.com:8080`.

<div class="mdx-switch">
  <button data-md-color-primary="red"><code>red</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-primary]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      var attr = this.getAttribute("data-md-color-primary")
      document.body.setAttribute("data-md-color-primary", attr)
      var name = document.querySelector("#__code_1 code span.l")
      name.textContent = attr.replace("-", " ")
    })
  })
</script>

    ```mermaid
    sequenceDiagram
        participant Cliente
        participant Navegador
        participant Servidor

        Cliente->>Navegador: Envia requisição (cross-origin)
        Navegador->>Servidor: Envia pré-verificação OPTIONS
        Servidor-->>Navegador: Retorna cabeçalhos CORS (ex: Access-Control-Allow-Origin)
        Navegador->>Cliente: Permite ou bloqueia o acesso
        Cliente->>Navegador: Envia requisição real (GET, POST, etc.)
        Navegador->>Servidor: Envia requisição real
        Servidor-->>Navegador: Retorna resposta real
        Navegador->>Cliente: Retorna dados para o cliente
    ```


## Status Code