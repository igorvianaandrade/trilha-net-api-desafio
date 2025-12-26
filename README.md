# DIO - Trilha .NET - API e Entity Framework
www.dio.me

## Desafio de projeto
Para este desafio, você precisará usar seus conhecimentos adquiridos no módulo de API e Entity Framework, da trilha .NET da DIO.

## Contexto
Você precisa construir um sistema gerenciador de tarefas, onde você poderá cadastrar uma lista de tarefas que permitirá organizar melhor a sua rotina.

Essa lista de tarefas precisa ter um CRUD, ou seja, deverá permitir a você obter os registros, criar, salvar e deletar esses registros.

A sua aplicação deverá ser do tipo Web API ou MVC, fique a vontade para implementar a solução que achar mais adequado.

A sua classe principal, a classe de tarefa, deve ser a seguinte:

![Diagrama da classe Tarefa](diagrama.png)

Não se esqueça de gerar a sua migration para atualização no banco de dados.

## Métodos esperados
É esperado que você crie o seus métodos conforme a seguir:


**Swagger**


![Métodos Swagger](swagger.png)


**Endpoints**


| Verbo  | Endpoint                | Parâmetro | Body          |
|--------|-------------------------|-----------|---------------|
| GET    | /Tarefa/{id}            | id        | N/A           |
| PUT    | /Tarefa/{id}            | id        | Schema Tarefa |
| DELETE | /Tarefa/{id}            | id        | N/A           |
| GET    | /Tarefa/ObterTodos      | N/A       | N/A           |
| GET    | /Tarefa/ObterPorTitulo  | titulo    | N/A           |
| GET    | /Tarefa/ObterPorData    | data      | N/A           |
| GET    | /Tarefa/ObterPorStatus  | status    | N/A           |
| POST   | /Tarefa                 | N/A       | Schema Tarefa |

Esse é o schema (model) de Tarefa, utilizado para passar para os métodos que exigirem

```json
{
  "id": 0,
  "titulo": "string",
  "descricao": "string",
  "data": "2022-06-08T01:31:07.056Z",
  "status": "Pendente"
}
```


## Solução
O código está pela metade, e você deverá dar continuidade obedecendo as regras descritas acima, para que no final, tenhamos um programa funcional. Procure pela palavra comentada "TODO" no código, em seguida, implemente conforme as regras acima.

# 📘 Comandos EF - Entity Framework Core

Este guia descreve os passos necessários para configurar e utilizar o **Entity Framework Core** em projetos .NET, incluindo instalação, configuração de contexto, migrations e conexão com o banco de dados SQL Server.

---

## ⚙️ Instalação Global (executar apenas uma vez)

Ferramenta para auxiliar na execução de comandos EF no console:

```bash
dotnet tool install --global dotnet-ef
```

---

## 📦 Pacotes necessários no projeto

Cada projeto deve instalar os seguintes pacotes (verifique no arquivo `ModuloAPI.csproj`):

```bash
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

---

## 🗂 Estrutura do Projeto

- **Entities/** → Pasta para salvar as classes (entidades/modelos) que serão transformadas em tabelas no BD.  
- **Context/** → Pasta para salvar a classe de contexto (ex: `AgendaContext.cs`), responsável por centralizar as informações do BD e realizar a comunicação.

---

## 🏗 Configuração do Contexto

1. Criar uma classe que **herda de `DbContext`** (biblioteca `EntityFrameworkCore`).  
2. Criar um **construtor** que recebe a conexão do BD e passa para a classe base `DbContext`.  
3. Criar propriedades `DbSet<T>` para representar tabelas.  
   - Exemplo:  
   ```csharp
   public DbSet<Contato> Contatos { get; set; }
   ```

---

## 🔑 Configuração da Conexão

Na pasta **Properties**, temos arquivos JSON para configuração de ambientes (dev/test/prod).  
No arquivo `appSettings.Development.json`, adicionar a chave de conexão:

```json
"ConnectionStrings": {
  "ConexaoPadrao": "Server=;Initial Catalog=;Integrated Security=True"
}
```

No `Program.cs`, registrar o contexto:

```csharp
// Add services to the container.
builder.Services.AddDbContext<AgendaContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("ConexaoPadrao")));
```

> 💡 **Boas práticas**: Todas as configurações devem ser armazenadas nos arquivos JSON.

---

## 🛠️ Migrations (Mapeamento EF)

1. Verifique se o SQL Server está em execução (`Sql Server Configuration Manager -> SqlServer (SqlExpress)`).
2. Criar uma migration:
   ```bash
   dotnet ef migrations add CriacaoTabelaContato
   ```
   - Será criada a pasta **Migrations** com arquivos de criação (`Up`) e deleção (`Down`) de tabelas.
3. Executar a migration no banco:
   ```bash
   dotnet ef database update
   ```

---

## 📊 Verificação no Banco

- Abrir o SQL Server e verificar o banco **Agenda**.  
- Conferir a tabela **Contato**.  
- O EF mantém a tabela **__EFMigrationsHistory** para gerenciar o histórico de migrations aplicadas.

---

## 🌐 Criar Controller e Endpoints

Após configurar o contexto e aplicar migrations, crie um **Controller** para expor os endpoints da API e manipular os dados das entidades.

---

## ✅ Resumo

- Instalar `dotnet-ef` globalmente.  
- Adicionar pacotes `EntityFrameworkCore.Design` e `EntityFrameworkCore.SqlServer`.  
- Criar pastas **Entities** e **Context**.  
- Configurar `DbContext` e `DbSet`.  
- Registrar conexão no `appSettings.json` e `Program.cs`.  
- Criar e aplicar migrations.  
- Verificar tabelas no SQL Server.  
- Criar controllers para expor endpoints.

---