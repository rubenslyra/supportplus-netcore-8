# Modelagem de Dados

A modelagem de dados para o projeto SupportPlus inclui as seguintes entidades:

1. **Ticket**:
   - **Id**: Identificador único do ticket (chave primária).
   - **Título**: Título ou assunto do ticket. (VARCHAR(100))
   - **Descrição**: Descrição detalhada do problema ou solicitação. (TEXT)
   - **Status**: Status atual do ticket (aberto, em progresso, fechado, etc.). (ENUM)
   - **Data de Criação**: Data e hora de criação do ticket. (DATETIME)
   - **Data de Alteração**: Data e hora da última alteração do ticket. (DATETIME)
   - **Data de Remoção**: Data e hora de remoção lógica do ticket. (DATETIME)
   - **Id do Usuário**: Chave estrangeira que faz referência ao usuário que criou o ticket. (FK)

2. **Comentário (Comment)**:
   - **Id**: Identificador único do comentário (chave primária).
   - **Mensagem**: Conteúdo do comentário. (TEXT)
   - **Data de Criação**: Data e hora de criação do comentário. (DATETIME)
   - **Data de Alteração**: Data e hora da última alteração do comentário. (DATETIME)
   - **Data de Remoção**: Data e hora de remoção lógica do comentário. (DATETIME)
   - **Id do Ticket**: Chave estrangeira que faz referência ao ticket ao qual o comentário está associado. (FK)
   - **Id do Usuário**: Chave estrangeira que faz referência ao usuário que fez o comentário. (FK)

3. **Confirmação de E-mail (EmailConfirmation)**:
   - **Id do Usuário**: Chave estrangeira que faz referência ao usuário associado à confirmação de e-mail. (FK)
   - **Código de Confirmação**: Código único gerado para confirmar o e-mail. (VARCHAR(50))
   - **Data de Expiração**: Data e hora de expiração do código de confirmação. (DATETIME)
   - **Data de Criação**: Data e hora de criação do registro. (DATETIME)

4. **Operador (Operator)**:
   - **Matrícula**: Identificador único do operador (chave primária). (INT)
   - **Nome**: Nome do operador. (VARCHAR(100))
   - **Cargo**: Cargo do operador. (VARCHAR(100))

5. **Analista (Analyst)**:
   - **Matrícula**: Identificador único do analista (chave primária). (INT)
   - **Nome**: Nome do analista. (VARCHAR(100))
   - **Estado**: Estado atual do analista (ocupado, disponível, ausente, off, etc.). (ENUM)

6. **Analistas de Plantão (OnCallAnalysts)**:
   - **Id**: Identificador único do registro (chave primária). (INT)
   - **Matrícula do Analista**: Chave estrangeira que faz referência ao analista de plantão. (FK)
   - **Data de Início**: Data e hora de início do período de plantão. (DATETIME)
   - **Data de Término**: Data e hora de término do período de plantão. (DATETIME)

7. **TicketOperatorAssignment**:
   - **Id**: Identificador único do registro (chave primária). (INT)
   - **Id do Ticket**: Chave estrangeira que faz referência ao ticket associado. (FK)
   - **Matrícula do Operador**: Chave estrangeira que faz referência ao operador designado para o ticket. (FK)
   - **Data de Atribuição**: Data e hora em que o operador foi designado para o ticket. (DATETIME)

Com base nessas entidades, podemos criar o diagrama de entidade-relacionamento (ER) para representar a estrutura de dados do projeto SupportPlus. Este diagrama mostrará as relações entre as entidades e suas respectivas chaves primárias e estrangeiras. Vou criar uma representação simplificada do diagrama:

```
    +-----------------------+        +---------------------+
    |        Ticket         |        |      Comentário     |
    +-----------------------+        +---------------------+
    | Id (PK)               |<----+  | Id (PK)             |
    | Título                |     |  | Mensagem            |
    | Descrição             |     |  | Data de Criação     |
    | Status                |     |  | Data de Alteração   |
    | Data de Criação       |     |  | Data de Remoção     |
    | Data de Alteração     |     |  | Id_Ticket (FK)      |
    | Data de Remoção       |     |  | Id_Usuário (FK)     |
    | Id_Usuário (FK)       |     |  +---------------------+
    +-----------------------+     |
          |                       |
          |     +-----------------|-------------------+
          |     |                 |                   |
          +-----|-----------------+                   |
                |                                     |
                |     +------------------+            |
                |     |    EmailConfirmation |        |
               

 +-----|------------------+            |
                |     | Id_Usuário (FK)  |<----------+
                |     | Código de        |
                |     | Confirmação      |
                |     | Data de Expiração|
                |     | Data de Criação  |
                +-----|------------------+

    +---------------------+        +---------------------+
    |       Operador      |        |       Analista      |
    +---------------------+        +---------------------+
    | Matrícula (PK)      |        | Matrícula (PK)      |
    | Nome                |        | Nome                |
    | Cargo               |        | Estado              |
    +---------------------+        +---------------------+

    +-----------------------+
    | OnCallAnalysts        |
    +-----------------------+
    | Id (PK)               |
    | Matrícula do Analista |-----+
    | Data de Início        |     |
    | Data de Término       |     |
    +-----------------------+     |
                                   |
    +-----------------------+     |
    | TicketOperatorAssignment|     |
    +-----------------------+     |
    | Id (PK)               |     |
    | Id do Ticket (FK)     |-----+
    | Matrícula do Operador |     |
    | Data de Atribuição    |     |
    +-----------------------+

```


### Transações e Segurança

Ao adicionar dados diretamente ao banco de dados, é fundamental garantir que as transações sejam realizadas de forma segura e que os dados estejam consistentes. Aqui estão algumas práticas recomendadas:

1. **Use Transações para Agrupar Operações Relacionadas**:
   - Utilize transações para agrupar operações relacionadas e garantir que todas sejam executadas com sucesso ou desfeitas em caso de falha.

2. **Validação de Dados**:
   - Valide todos os dados de entrada para garantir que estejam em conformidade com as regras de negócios e prevenir inserção de dados inválidos ou maliciosos.

3. **Registro de Auditoria**:
   - Registre todas as operações críticas realizadas no banco de dados para fins de auditoria e rastreabilidade. A tabela `AuditLog` pode ser usada para esse fim.

4. **Segurança dos Dados**:
   - Utilize técnicas de criptografia para proteger dados sensíveis, como senhas e informações de cartão de crédito.

5. **Tratamento de Exceções Adequado**:
   - Implemente tratamento de exceções robusto para lidar com falhas durante a execução de operações de banco de dados.

6. **Limite de Acesso aos Dados**:
   - Conceda apenas as permissões mínimas necessárias para acessar os dados do banco de dados, seguindo o princípio do menor privilégio.

Certifique-se de implementar essas práticas em todas as operações de banco de dados em seu projeto SupportPlus.


Espero que esta atualização atenda às suas necessidades. Se precisar de mais alguma coisa, não hesite em me avisar!
