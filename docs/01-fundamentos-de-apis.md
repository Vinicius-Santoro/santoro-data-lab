# **Fundamentos Teóricos de APIs**

## Objetivo

Entender o que é uma API, como funciona e quais são seus tipos e padrões.

## 1. O que é uma API?

### 1.1 Introdução

Uma API (Application Programming Interface) é um conjunto de regras que permite que diferentes sistemas se comuniquem entre si de maneira estruturada e previsível. Em termos simples, uma API atua como uma ponte que conecta aplicações, possibilitando a troca de dados e funcionalidades sem que seja necessário conhecer a implementação interna de cada sistema.

Segundo o livro "Designing Web APIs" (Brenda Jin, Saurabh Sahni, Amir Shevat, 2018), uma API bem projetada é como uma "promessa de serviço" que define exatamente o que pode ser requisitado, o que será entregue e em quais condições. 

A importância das APIs cresceu com a transformação digital das empresas: elas são a base da comunicação entre serviços em nuvem, da integração entre parceiros de negócios e da inovação de produtos. Sem APIs, seria inviável escalar sistemas complexos como redes sociais, plataformas de pagamento, marketplaces e serviços de streaming.

Em resumo, APIs são fundamentais para:

- Conectar sistemas de forma eficiente e segura;
- Acelerar o desenvolvimento de software;
- Permitir integrações com parceiros e terceiros;
- Potencializar a inovação digital e a criação de novos negócios.

## 2. Tipos de API

### 2.1 REST

API REST (Representational State Transfer) é o tipo de API mais usado atualmente.
Ela funciona como um cardápio de internet: você faz um pedido (requisição) para um endereço (URL), e o servidor te entrega exatamente aquilo que você pediu, normalmente em formato JSON.

- Usa os métodos padrão da internet: GET, POST, PUT, DELETE.
- Cada URL representa um recurso (ex.: /usuarios para usuários, /produtos para produtos)
- É simples, rápida e fácil de entender.

**Exemplo:** Quando você entra num app para ver suas mensagens, o app faz um GET para /mensagens, e recebe a lista.

### 2.2 SOAP
SOAP (Simple Object Access Protocol) é um tipo de API mais antigo e mais formal.
Ela funciona como uma troca de cartas muito bem organizada: você sempre envia uma mensagem seguindo regras super específicas, usando o formato XML.

- Usa um padrão bem rígido de mensagens e respostas.
- É mais seguro e recomendado em sistemas bancários, de saúde e onde a segurança é prioridade.
- Mais "pesado" e difícil de configurar do que REST.

**Exemplo:** Bancos ainda usam SOAP para transferências eletrônicas porque ele garante que toda a comunicação siga normas de segurança rigorosas.

### 2.3 GraphQL
GraphQL é uma tecnologia mais moderna criada pelo Facebook.
Ela funciona como um cardápio à la carte: você pede exatamente o que quer e só recebe aquilo, nem mais, nem menos.

- Você envia uma única requisição dizendo quais campos de dados precisa.
- Evita receber dados desnecessários ou ter que fazer várias requisições diferentes.
- Muito útil em aplicativos com interfaces ricas, como redes sociais.

**Exemplo:** Em vez de buscar todos os dados de um usuário, você pede só o nome e o e-mail, se for isso que precisa.

!!! Question

    **Em APIs GraphQL, eu peço e recebo exatamente os dados que solicitei. No entanto, em APIs REST também é possível filtrar o que é retornado com query parameters. Qual é então a diferença?**

    Em APIs REST, você consegue sim usar query parameters para filtrar os dados que quer buscar, mas é diferente do que acontece em GraphQL.

    No REST, você usa query parameters para:

    - Filtrar resultados (ex.: buscar usuários de uma certa cidade)
    - Ordenar (ex.: por data de criação)
    - Paginar (ex.: trazer só 10 resultados por vez)

    **Exemplo:**
    
    ```GET /usuarios?cidade=SãoPaulo&limite=10```

    Esse exemplo pega só usuários da cidade de São Paulo, limitando a 10 resultados. **Mas atenção:** o que você não consegue controlar no REST (normalmente) é quais campos de cada usuário vão vir.

    Ou seja, vem o objeto inteiro (nome, e-mail, telefone, endereço...) mesmo que você só queira o nome e o telefone.
    No GraphQL, você controla o formato da resposta. Você diz na requisição:
    "Me envie apenas o `nome` e o `telefone`", e a resposta vem só com isso.

    | Característica                  | REST (com query params)   | GraphQL                        |
    |:---------------------------------|:---------------------------|:-------------------------------|
    | Filtrar dados                    | ✅ Sim                     | ✅ Sim                         |
    | Escolher quais campos da resposta| 🚫 Não (vem padrão)         | ✅ Sim (você escolhe)           |
    | Flexibilidade na resposta        | Média                      | Alta                           |

### 2.4 WebSockets
WebSockets é um tipo especial de comunicação em tempo real.
É como se você abrisse um canal direto entre o cliente (ex.: seu navegador) e o servidor: os dois podem mandar mensagens a qualquer momento, sem ter que ficar "pedindo" toda hora.

- Ideal para chats, jogos online, notificações em tempo real.
- Permite troca de dados instantânea e contínua.
- Diferente do REST que precisa de uma requisição para cada resposta.

**Exemplo:** Em um app de chat, quando alguém te manda uma mensagem, ela aparece na tela na hora sem você precisar atualizar a página.

!!! Question

    **Como funciona a comunicação em tempo real via WebSocket e qual sua relação com Kafka e SQS?**

    - **WebSocket** é um protocolo que mantém uma conexão aberta entre o cliente e o servidor, permitindo comunicação em tempo real. Ele é ideal para chats, notificações instantâneas e jogos online.

    - **Kafka e SQS** são ferramentas de mensageria usadas para comunicação entre sistemas no backend, permitindo que serviços diferentes troquem mensagens de forma assíncrona.

    - **Integração:** Enquanto o WebSocket permite a comunicação em tempo real com o cliente, Kafka e SQS são usados para gerenciar e processar eventos e mensagens no backend, facilitando a escalabilidade e integração entre sistemas.

## 3. Componentes básicos

### 3.1 Endpoints

No contexto das APIs, um endpoint é uma URL específica que define um ponto de acesso onde uma requisição HTTP pode ser feita para interagir com o sistema. Cada endpoint está associado a uma ação específica ou a um recurso dentro da API, como obter ou enviar dados.

Cada endpoint é geralmente mapeado para uma operação HTTP:

- GET: Obtém dados.
- POST: Cria novos dados.
- PUT: Atualiza dados existentes.
- DELETE: Remove dados.

**Exemplo:**

- ```GET https://api.exemplo.com/usuarios/123```: Retorna os dados do usuário com ID 123.
- ```POST https://api.exemplo.com/usuarios```: Cria um novo usuário.

### 3.1 Rotas

A rota é o caminho ou direção que a requisição segue dentro do servidor para chegar a um determinado recurso. 

**Exemplo:** Em uma API de usuários, a rota pode ser definida como `/usuarios`, ou `/usuarios/{id}`.

!!! Question

    **Qual a diferença de endpoint e rota?**

    A diferença entre endpoint e rota é sutil, mas fundamental para entender o funcionamento de uma API.

    - **Rota:** Refere-se ao caminho ou URL que a requisição segue para acessar um recurso.
    - **Endpoint:** É a combinação do método HTTP (como GET, POST, etc.) com a rota, ou seja, é o ponto de acesso completo que define a operação que será executada.

    **Exemplo:**

    - Rota: `/usuarios`
    - Endpoint:
        - `GET /usuarios`: Obtém a lista de usuários.
        - `POST /usuarios`: Cria um novo usuário.

#### 3.1.2 Boas práticas

| **Prática**                                   | **Descrição**                                                                                                                                                  |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Substantivos no Plural para Recursos**  | Utilize nomes no plural para representar recursos (ex: `/usuarios` em vez de `/usuario`).                                                                    |
| **Mantenha URLs Simples e Descritivas**       | As rotas devem ser intuitivas e refletir claramente o recurso (ex: `/produtos`, `/clientes/{id}`).                                                              |
| **Use Métodos HTTP Corretos**                 | Use `GET` para recuperar dados, `POST` para criar, `PUT`/`PATCH` para atualizar e `DELETE` para remover recursos.                                               |
| **Use Níveis Hierárquicos para Relações**     | Use estrutura hierárquica para indicar relacionamento entre recursos (ex: `/usuarios/{usuarioId}/pedidos`).                                                     |
| **Evite Verbos nas Rotas**                    | Não coloque verbos nas rotas, pois os métodos HTTP já indicam a ação (ex: `/usuarios` e não `/getUsuarios`).                                                     |
| **Use Query Parameters para Filtros**         | Utilize query parameters para filtros, ordenação ou paginação (ex: `/usuarios?idade=30&ordenar=nome`).                                                         |
| **Evite Uso Excessivo de Sub-Rotas**          | Mantenha as rotas simples e evite profundidade excessiva (ex: `/usuarios/{usuarioId}/enderecos` e não `/usuarios/{usuarioId}/endereco/{enderecoId}/telefone`). |
| **Use Convenções Padrão de API RESTful**      | Mantenha consistência nas convenções de nomes e design de rotas (ex: `/produtos`, `/produtos/{id}`).                                                           |
| **Retorne Códigos de Status HTTP Adequados**  | Utilize códigos de status HTTP para indicar sucesso ou falha (ex: `200 OK`, `404 Not Found`, `500 Internal Server Error`).                                    |
| **Documente as Rotas**                        | Documente detalhadamente cada rota, incluindo métodos, parâmetros e tipos de resposta.                                                                        |

### 4. Query e Path Parameters

Query Parameters são usados para filtrar, ordenar ou paginar os dados da API. Eles são adicionados após o ponto de interrogação (?) na URL e seguem o formato key=value.

**Exemplo:**

`/produtos?categoria=eletronicos&page=2`

Path Parameters são usados para identificar recursos específicos na URL, sendo uma parte essencial da rota. Eles são definidos diretamente no caminho da URL, entre chaves, como em `/usuarios/{id}` ou `/produtos/{produtoId}/detalhes`. Path parameters são obrigatórios para acessar um recurso específico e não são opcionais como os query parameters, sendo parte da estrutura da URL.

## Métodos HTTP e Status Codes

- GET, POST, PUT, PATCH, DELETE – Diferenças e usos ideais.
- Status codes HTTP: 2xx (sucesso), 3xx (redirecionamento), 4xx (erro do cliente), 5xx (erro do servidor).
- Padrões para respostas de erro (estrutura de erro, mensagens padronizadas).

## RESTful APIs e boas práticas

- O que significa ser RESTful.
- Princípios do REST: Stateless, Cacheable, Layered System, Uniform Interface.
- HATEOAS: Hypermedia as the Engine of Application State.

## OpenAPI (Swagger)

- O que é e por que usar.
- Como documentar APIs corretamente.
- Ferramentas para trabalhar com OpenAPI (Swagger UI, Redoc).