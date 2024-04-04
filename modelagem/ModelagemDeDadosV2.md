# Modelagem de Dados

A modelagem de dados para o projeto SupportPlus inclui as seguintes entidades:

1. **Ticket**:
   - **Id**: Identificador único do ticket (chave primária).
   - **Título**: Título ou assunto do ticket. (VARCHAR(100))
   - **Descrição**: Descrição detalhada do problema ou solicitação. (TEXT)
   - **Status**: Status atual do ticket (aberto, em progresso, pendente, resolvido, fechado).
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

4. **AuditLog**:
   - **Id**: Identificador único do log de auditoria (chave primária).
   - **Data e Hora**: Data e hora em que a operação foi realizada. (DATETIME)
   - **Operação**: Tipo de operação realizada (por exemplo, Criação, Atualização, Remoção).
   - **Entidade Afetada**: Nome da entidade afetada pela operação.
   - **Id da Entidade Afetada**: Id da entidade afetada pela operação.
   - **Id do Usuário**: Chave estrangeira que faz referência ao usuário que realizou a operação. (FK)

5. **TicketStatus (Enum)**:
   - Representa os diferentes estados possíveis de um ticket (aberto, em progresso, pendente, resolvido, fechado).

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
                |     |   EmailConfirmation |        |
                +-----|------------------+            |
                |     | Id_Usuário (FK)  |<----------+
                |     | Código de        |
                |     | Confirmação      |
                |     | Data de Expiração|
                |     | Data de Criação  |
                +----------------------+

                +-----------------------+
                |        AuditLog       |
                +-----------------------+
                | Id (PK)               |
                | Data e Hora           |
                | Operação              |
                | Entidade Afetada      |
                | Id da Entidade Afetada|
                | Id_Usuário (FK)       |
                +-----------------------+

                +-----------------------+
                |      TicketStatus     |
                +-----------------------+
                | Id (PK)               |
                | Status                |
                +-----------------------+
```

Esta estrutura conceitual reflete as entidades principais do projeto SupportPlus, bem como o novo enum `TicketStatus` e a tabela auxiliar `AuditLog` para registro de auditoria. Essa modelagem fornece uma base sólida para o desenvolvimento do sistema de suporte e chamados, considerando as melhores práticas e requisitos do projeto.
