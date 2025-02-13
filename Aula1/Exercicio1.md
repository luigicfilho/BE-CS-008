# Exercício: API de Gerenciamento de Produtos

## Objetivo

Desenvolver uma API RESTful para gerenciar produtos de um e-commerce, utilizando .NET e C#. A API deve permitir operações CRUD (Criar, Ler, Atualizar, Deletar) para produtos, com informações como nome, descrição, preço e categoria.


## Descrição

Você será responsável por criar uma API que permita o gerenciamento de produtos de um e-commerce. A API deve ser capaz de:

- **Criar** novos produtos.
- **Ler** a lista de produtos ou detalhes de um produto específico.
- **Atualizar** informações de um produto existente.
- **Deletar** um produto.

### Modelos

Cada produto deve ter as seguintes informações:

- **Id** (int, identificador único)
- **Nome** (string, nome do produto)
- **Descrição** (string. descrição detalhada do produto)
- **Preço** (decimal, valor do produto)
- **Categoria** (string, categoria do produto, como "Eletrônicos", "Roupas", etc.)

### Código de Status HTTP

Utilize os códigos de status HTTP adequados para indicar o resultado de cada operação:

- 200 OK para operações de leitura bem-sucedidas.
- 201 Created para criação de produto bem-sucedida.
- 204 No Content para operações de atualização e exclusão bem-sucedidas.
- 400 Bad Request para requisições inválidas (dados incorretos, etc.).
- 404 Not Found para produtos não encontrados.
- 422 Unprocessable Entity para quando os dados enviados sejam inválidos
- 500 Internal Server Error para erros internos do servidor.

### Validações

Implemente validações nos dados de entrada para garantir a integridade dos dados:

- Campos obrigatórios (Nome, Preco).
- Tipos de dados corretos (Preco deve ser decimal).

### Boas Práticas
- Utilize convenções de nomenclatura claras e consistentes.
- Organize o código em camadas (Controllers, Services, Repositories) para melhor manutenibilidade.
- Implemente tratamento de erros adequado.
- Documente a API utilizando Swagger ou outra ferramenta similar.

## Requisitos Técnicos
1. **Tecnologia:** Utilize .NET com C# para desenvolver a API.
2. **Formato de Dados:** A API deve trabalhar com JSON para entrada e saída de dados.
3. **Endpoints:**
    - Listar todos os produtos
        - Método: GET /api/produtos
        - Resposta (sucesso)(Exemplo):
            ```json
            [
            {
                "id": 1,
                "nome": "Smartphone XYZ",
                "descricao": "Um smartphone avançado com câmera de 48MP.",
                "preco": 1999.99,
                "categoria": "Eletrônicos"
            },
            {
                "id": 2,
                "nome": "Notebook ABC",
                "descricao": "Notebook com 16GB de RAM e SSD de 512GB.",
                "preco": 4500.00,
                "categoria": "Eletrônicos"
            }
            ...
            ]
            ```
        - Retorno esperado:
            - Código 200 OK com a lista de todos os produtos.
    - Buscar um produto por ID
        - Método: GET /api/produtos/{id}
        - Retorno esperado:
            - Código 200 OK com os detalhes do produto específico.
            - Código 404 Not Found se o produto não existir.
            - Código 400 Bad Request caso a requisição seja inválida
    - Criar um novo produto
        - Método: POST /api/produtos
        - Corpo da requisição (JSON):
            ```json
            {
            "nome": "Smartphone X",
            "descricao": "Celular de última geração",
            "preco": 2999.99,
            "categoria": "Eletrônicos"
            }
            ```
        - Resposta (sucesso)(Exemplo):
            ```json
            {
            "id": 1,
            "nome": "Smartphone X",
            "descricao": "Celular de última geração",
            "preco": 2999.99,
            "categoria": "Eletrônicos"
            }
            ```
        - Retorno esperado:
            - Código 201 Created com os dados do produto cadastrado.
            - Código 400 Bad Request caso a requisição seja inválida
            - Código 422 Unprocessable Entity caso falha os dados não sejam válidos
    - Atualizar um produto
        - Método: PUT /api/produtos/{id}
        - Corpo da requisição (JSON):
            ```json
            {
            "nome": "Smartphone X Pro",
            "descricao": "Celular atualizado com mais memória",
            "preco": 3499.99,
            "categoria": "Eletrônicos"
            }
            ```
        - Resposta (sucesso)(Exemplo):
            ```json
            {
            "id": 1,
            "nome": "Smartphone X Pro",
            "descricao": "Celular atualizado com mais memória",
            "preco": 3499.99,
            "categoria": "Eletrônicos"
            }
            ```
        - Retorno esperado:
            - Código 200 OK com os dados do produto atualizado.
            - Código 404 Not Found se o produto não existir.
            - Código 400 Bad Request caso a requisição seja inválida
            - Código 422 Unprocessable Entity caso falha os dados não sejam válidos
    - Deletar um produto
        - Método: DELETE /api/produtos/{id}
        - Retorno esperado:
            - Código 204 No Content se a exclusão for bem-sucedida.
            - Código 404 Not Found se o produto não existir.
            - Código 400 Bad Request caso a requisição seja inválida
4. **Validação:** Implemente validações básicas, como garantir que o nome e o preço sejam fornecidos e que o preço seja um valor positivo.
    1. Tratamento de Erros:
        - Retorne códigos de status HTTP apropriados, como 400 Bad Request para requisições inválidas ou 404 Not Found para produtos não encontrados.
        - Inclua mensagens de erro descritivas no corpo da resposta, se aplicável.
5. **Persistência:** Utilize um banco de dados simples, como SQLite, para armazenar os produtos.

## Requisitos Não Funcionais
- Utilizar ASP.NET Core para construir a API.
- Utilizar Entity Framework Core para acesso ao banco de dados.
- Armazenar os produtos em um banco de dados relacional (ex: SQLite).
- Seguir boas práticas de desenvolvimento RESTful.
- Implementar tratamento de erros e retornos adequados para status HTTP.

## Bônus (Opcional)
- Implemente paginação para a listagem de produtos.
- Adicione autorização básica (ApiKey) para proteger os endpoints de criação, atualização e exclusão.

## Critérios de Avaliação

### Entrega

- Crie um repositório no GitHub (ou outra plataforma de versionamento) e compartilhe o link.
- Inclua um arquivo README.md com instruções para executar a API.
- Certifique-se de que o código esteja bem organizado.

### Critérios

1. **Funcionalidade:**
    - A API deve realizar todas as operações CRUD corretamente.
    - Implementação correta dos endpoints RESTful.
    - Uso adequado do Entity Framework Core para persistência de dados.
    - Organização do código seguindo princípios como Clean Code.
2. **Qualidade do Código:**
    - O código deve estar limpo, bem organizado e seguir boas práticas.
    - Testes básicos utilizando Postman ou outra ferramenta similar.
3. **Documentação:** 
    - O README.md deve conter instruções claras para execução do projeto.
4. **Tratamento de Erros:** 
    - A API deve lidar adequadamente com erros e retornar respostas significativas.