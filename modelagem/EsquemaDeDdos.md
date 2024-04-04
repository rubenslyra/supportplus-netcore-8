# Modelagem de Dados

A modelagem de dados para o projeto SupportPlus pode incluir as seguintes entidades:

1. **Ticket**:
   - **Id**: Identificador único do ticket (chave primária).
   - **Título**: Título ou assunto do ticket. (máximo de 100 caracteres)
   - **Descrição**: Descrição detalhada do problema ou solicitação.
   - **Status**: Status atual do ticket (aberto, em progresso, fechado, etc.).
   - **Data de Criação**: Data e hora de criação do ticket.
   - **Data de Alteração**: Data e hora da última alteração do ticket.
   - **Data de Remoção**: Data e hora de remoção lógica do ticket (após 1 ano).
   - **Id do Usuário**: Chave estrangeira que faz referência ao usuário que criou o ticket.
   - **Id do Usuário que Removeu**: Chave estrangeira que faz referência ao usuário que removeu o ticket (null se não removido).

2. **Comentário (Comment)**:
   - **Id**: Identificador único do comentário (chave primária).
   - **Mensagem**: Conteúdo do comentário. (máximo de 500 caracteres)
   - **Data de Criação**: Data e hora de criação do comentário.
   - **Id do Ticket**: Chave estrangeira que faz referência ao ticket ao qual o comentário está associado.
   - **Id do Usuário**: Chave estrangeira que faz referência ao usuário que fez o comentário.

3. **Confirmação de E-mail (EmailConfirmation)**:
   - **Id do Usuário**: Chave estrangeira que faz referência ao usuário associado à confirmação de e-mail.
   - **Código de Confirmação**: Código único gerado para confirmar o e-mail.
   - **Data de Expiração**: Data e hora de expiração do código de confirmação.

Com base nessas entidades, podemos criar o diagrama de entidade-relacionamento (ER) para representar a estrutura de dados do projeto SupportPlus. Este diagrama mostrará as relações entre as entidades e suas respectivas chaves primárias e estrangeiras. Vou criar uma representação simplificada do diagrama:

```
    +---------------------+        +-----------------+
    |        Ticket       |        |    Comentário   |
    +---------------------+        +-----------------+
    | Id (PK)             |<----+  | Id (PK)         |
    | Título              |     |  | Mensagem        |
    | Descrição           |     |  | Data de Criação |
    | Status              |     |  | Id_Ticket (FK)  |
    | Data de Criação     |     |  | Id_Usuário (FK) |
    | Data de Alteração   |     |  +-----------------+
    | Data de Remoção     |     |
    | Id_Usuário (FK)     |     |
    | Id_Usuário_Remocao (FK) |  |
    +---------------------+     |
          |                      |
          |     +----------------|-------------------+
          |     |                |                   |
          +-----|----------------+                   |
                |                                    |
                |     +------------------+           |
                |     |  Confirmação de  |           |
                +-----|      E-mail      |           |
                      +------------------+           |
                      | Id_Usuário (FK)  |-----------+
                      | Código de        |
                      | Confirmação      |
                      | Data de Expiração|
                      +------------------+
```


Para adicionar dados diretamente ao banco de dados, é fundamental garantir que as transações sejam realizadas de forma segura e que os dados estejam consistentes. Vou fornecer uma instrução geral sobre como fazer isso, abordando as práticas recomendadas para transações e segurança.

### Adicionando Dados com Transações e Segurança

1. **Use Parâmetros SQL Parametrizados**:
   - Ao criar consultas SQL, use parâmetros parametrizados em vez de concatenar valores diretamente na string de consulta. Isso ajuda a evitar ataques de injeção SQL.

```csharp
using (var connection = new NpgsqlConnection(connectionString))
{
    await connection.OpenAsync();
    
    var sqlCommand = "INSERT INTO Tickets (Title, Description, ...) VALUES (@Title, @Description, ...)";
    using (var command = new NpgsqlCommand(sqlCommand, connection))
    {
        command.Parameters.AddWithValue("@Title", ticket.Title);
        command.Parameters.AddWithValue("@Description", ticket.Description);
        // Adicione outros parâmetros conforme necessário
        
        await command.ExecuteNonQueryAsync();
    }
}
```

2. **Use Transações para Garantir Consistência**:
   - Use transações para agrupar operações de banco de dados e garantir atomicidade, consistência, isolamento e durabilidade (ACID properties).

```csharp
using (var transaction = connection.BeginTransaction())
{
    try
    {
        // Execute as operações de banco de dados aqui
        
        transaction.Commit(); // Confirma as operações se tudo ocorrer bem
    }
    catch (Exception ex)
    {
        transaction.Rollback(); // Desfaz as operações se ocorrer um erro
        // Tratar a exceção ou lançá-la novamente
    }
}
```

3. **Trate Exceções de Forma Adequada**:
   - Implemente tratamento de exceções robusto para lidar com falhas durante a execução de operações de banco de dados.

```csharp
try
{
    // Operações de banco de dados aqui
}
catch (NpgsqlException ex)
{
    // Tratar a exceção específica do PostgreSQL
}
catch (Exception ex)
{
    // Tratar outras exceções
}
```

4. **Use Criptografia para Dados Sensíveis**:
   - Se os dados incluírem informações sensíveis, como senhas, utilize técnicas de criptografia para protegê-los.

```csharp
string senhaCriptografada = CriptografarSenha(user.Senha);
```

5. **Validação de Dados**:
   - Valide todos os dados de entrada para garantir que estejam em conformidade com as regras de negócios e prevenir inserção de dados inválidos ou maliciosos.

```csharp
if (!IsValidEmail(user.Email))
{
    throw new ArgumentException("Email inválido.");
}
```

6. **Limite de Acesso aos Dados**:
   - Utilize o princípio do menor privilégio e conceda apenas as permissões mínimas necessárias para acessar os dados do

 banco de dados.

7. **Auditoria**:
   - Registre todas as operações críticas realizadas no banco de dados para fins de auditoria e rastreabilidade.

### Considerações Finais

Ao adicionar dados diretamente ao banco de dados, é crucial seguir as práticas recomendadas para transações e segurança para garantir a integridade e a segurança dos dados. Certifique-se de implementar essas práticas em todas as operações de banco de dados em seu projeto.

