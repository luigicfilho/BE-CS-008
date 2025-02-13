# Aula 1 - Introdução ao REST e Arquitetura RESTful
 
### Problema Gerador
    Entendimento de APIs e APIs RESTful;

## Conceitos
    APIs e Web APIs;
    Ciclo de Requisição e Resposta;
        Negociação de Conteúdo;
            Cabeçalhos;
            Recursos;
        Media Types;
            XML e JSON;
    Métodos HTTP;
        Métodos HTTP e sua relação com as operações CRUD.
    Conceitos de REST
        Princípios e boas práticas da arquitetura REST
        HATEOAS.

## O que será abordado na aula prática
- Explicação sobre o Problema Gerador (TaskFlow) e etapas
- Explicação sobre os tipos de API (minimal e controller) vantagens e desvantagens
- Explicação sobre a dificuldade de implementação do HATEOAS.
- Explicação sobre nomeclatura e boas práticas
- Explicação sobre como estruturar o projeto (Api, App, Domain, Infra)[DDD]

### Comandos Aula
- Cria WebApi padrão (Minimal API)
```cs
    dotnet new webapi --name "Ada.WebApi1"
```
- Cria WebApi usando controllers (Segunda aula)
```cs
    dotnet new webapi --use-controllers --name "Ada.WebApi2"
```
- Utiliza certificados para desenvolvimento
```cs
    dotnet dev-certs https --trust
```
- Executa
```cs
    dotnet run 
```

### Exercício

Os detalhes sobre o exercício estão no arquivo Exercicio1.md no mesmo diretório desde Readme.
Também se encontra uma coleção do Postman para Testes E2E, para correção do exercício.

### Tutorial
1. Cria WebApi padrão (Minimal API)
```cs
    dotnet new webapi --name "Ada.WebApi1"
```
2. Utiliza certificados para desenvolvimento
```cs
    dotnet dev-certs https --trust
```
3. Cria as entidades
```cs
    internal record Movie(string Title, int Rating, bool Seen) { }
```
4. Cria uma lista de exemplo
```cs
    List<Movie> movies = new()
    {
        new("The Matrix", 5, true),
        new("Inception", 2, true),
        new("Ice Age", 4, true),
        new("Shrek", 5, true),
        new("Rambo", 0, false)
    };
```
5. Configura o endpoint para retornar a lista
```cs
    app.MapGet("/movies", () =>
    {
        return movies;
    });
```
6. Instala o pacote NSwag
```cs
    dotnet add package NSwag.AspNetCore
```
7. Configura o uso do OpenApi
```cs
    builder.Services.AddOpenApiDocument();
```
8. Registra o middleware de OpenAPI e Uso do Swagger
```cs
    if (app.Environment.IsDevelopment())
    {
        app.UseOpenApi();
        app.UseSwaggerUi();
    }
```
9. Execute e olhe o Swagger
```cs
    dotnet run 
```
10. Adiciona o endpoint para retornar um filme dado seu título
```cs
    app.MapGet("/movies/{title}", (string title) =>
    {
        return Results.Ok(movies.Where(x => x.Title.Contains(title)).ToList());
    });
```
11. Adiciona o endpoint para criar um novo filme
```cs
    app.MapPost("/movies", (Movie movie) =>
    {
        if (string.IsNullOrEmpty(movie.Title))
            throw new ArgumentException(nameof(movie.Title));

        movies.Add(movie);

        return Results.NoContent();
    });
```
12. Adiciona o endpoint para remoção de um filme dado o título
```cs
    app.MapDelete("/movies/{title}", (string title) =>
    {
        if (string.IsNullOrEmpty(title))
            throw new ArgumentException(null, nameof(title));

        Movie? toDelete = movies.SingleOrDefault(x => x.Title == title);

        if (toDelete == null)
            return Results.Problem($"Movie with the title {title} not found", statusCode: StatusCodes.Status500InternalServerError);

        movies.Remove(toDelete);

        return Results.NoContent();
    });
```
13. Verifique outro tipo de UI (ReDoc)
```cs
    //app.UseSwaggerUi(); Comente o swagger
    app.UseReDoc(); //adicione o uso do ReDoc
```
#### Passos opicionais:
14. Adicione o pacote do EntityFramework usando memória (melhor forma de desenvolver)
```cs
    dotnet add package Microsoft.EntityFrameworkCore.InMemory
    dotnet add package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
```
15. Crie um novo arquivo (nosso contexto de banco de dados)
```cs
    using Microsoft.EntityFrameworkCore;
    namespace MovieAPI;
    internal class MovieDbContext : DbContext
    {
        public MovieDbContext(DbContextOptions<MovieDbContext> options)
        : base(options) { }

        public DbSet<Movie> Movies => Set<Movie>();
    }
```
16. Registre nosso contexto e use um banco de dados em memória
```cs
    builder.Services.AddDbContext<MovieDbContext>(opt => opt.UseInMemoryDatabase("Movies"));
```
17. Altere nosso registro Movie para conter um campo Id
```cs
    internal record Movie(int Id, string Title, int Rating, bool Seen) { }
```
18. Remova nossa instancia de lista de filmes
```cs
    //List<Movie> movies = new()
    //{
    //    new("The Matrix", 5, true),
    //    new("Inception", 2, true),
    //    new("Ice Age", 4, true),
    //    new("Shrek", 5, true),
    //    new("Rambo", 0, false)
    //};
```
19. Altere nosso endpoint de obter lista de filme para usar o banco de dados
```cs
    app.MapGet("/movies", (MovieDbContext db) =>
    {
        return db.Movies.ToList();
    });
```
20. Altere nosso endpoint de adicionar um novo filme para usar o banco de dados
```cs
    app.MapPost("/movies", (Movie movie, MovieDbContext db) =>
    {
        if (string.IsNullOrEmpty(movie.Title))
            throw new ArgumentException(nameof(movie.Title));

        db.Movies.Add(movie);
        db.SaveChanges();

        return Results.Created($"/todoitems/{movie.Title}", movie);
    });
```
21. Comente os outros endpoints ou altere-os para também usar o banco de dados
```cs
    //app.MapGet("/movies/{title}", (string title) =>
    //{
    //    return Results.Ok(movies.Where(x => x.Title.Contains(title)).ToList());
    //});

    //app.MapDelete("/movies/{title}", (string title) =>
    //{
    //    if (string.IsNullOrEmpty(title))
    //        throw new ArgumentException(null, nameof(title));

    //    Movie? toDelete = movies.SingleOrDefault(x => x.Title == title);

    //    if (toDelete == null)
    //        return Results.Problem($"Movie with the title {title} not found", statusCode: StatusCodes.Status500InternalServerError);

    //    movies.Remove(toDelete);

    //    return Results.NoContent();
    //});
```
22. Altere novamente para usar o Swagger
```cs
    app.UseSwaggerUi();
    //app.UseReDoc();
```
23. Teste os endpoints, obtendo a lista (vazia), crie um novo filme, obtenha novamente  para receber o item criado.

###### Links
- [Documentação Mozilla - PT](https://developer.mozilla.org/pt-BR/docs/Web/HTTP)
- [Lista de tipos de midia - IANA - EN](https://www.iana.org/assignments/media-types/media-types.xhtml)
- [Post do Uncle Bob sobre Arquitetura Limpa - EN](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
