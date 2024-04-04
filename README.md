# SupportPlus

SupportPlus é um sistema de suporte baseado em .NET Core 8.0 e ASP.NET Core, desenvolvido com a abordagem de Domain-Driven Design (DDD) e seguindo as melhores práticas de desenvolvimento de software. Este projeto serve como exemplo de como construir um sistema de suporte robusto e escalável utilizando tecnologias modernas.

## Tecnologias Utilizadas

- **.NET Core 8.0**: Plataforma de desenvolvimento de software multiplataforma e de código aberto.
- **ASP.NET Core**: Framework para criação de aplicativos web e APIs usando .NET.
- **Entity Framework Core**: ORM (Object-Relational Mapper) para acesso a banco de dados.
- **PostgreSQL**: Banco de dados relacional usado para persistência de dados.
- **Mailtrap**: Serviço de teste de envio de e-mails para desenvolvedores.
- **Firebase**: Plataforma de desenvolvimento de aplicativos móveis e web do Google.
- **Azure Functions (simulado)**: Serviços para processamento de eventos baseados em servidor sem provisionamento.
- **Swagger**: Ferramenta para documentação e teste de APIs.
- **FluentValidation**: Biblioteca para validação de objetos em .NET.
- **Serilog**: Framework de logging para .NET.
- **XUnit**: Framework de teste de unidade para .NET.
- **Moq**: Biblioteca para criação de mocks em testes unitários.

## Abordagens e Estrutura do Projeto

O projeto SupportPlus segue as seguintes abordagens e estrutura:

- **Domain-Driven Design (DDD)**: Organização do código em torno do domínio do problema, separando as responsabilidades em camadas de aplicação, domínio e infraestrutura.
- **Arquitetura Hexagonal (Ports and Adapters)**: Separação clara entre a lógica de negócios e as preocupações de infraestrutura, facilitando a substituição de componentes e testabilidade.
- **Padrão Repository**: Abstração do acesso a dados para desacoplar a lógica de negócios do tipo de armazenamento de dados.
- **Testes Unitários e Integração**: Cobertura de testes para garantir a qualidade do código e o comportamento esperado do sistema.
- **Logging e Auditoria**: Registro de atividades importantes para auditoria e diagnóstico de problemas.

A estrutura do projeto é organizada da seguinte forma:

- **SupportPlus.API**: Projeto ASP.NET Core responsável por expor APIs RESTful para interação com o sistema.
- **SupportPlus.Application**: Camada de aplicação que contém os serviços de aplicação responsáveis por orquestrar as operações de negócio.
- **SupportPlus.Domain**: Camada de domínio que contém as entidades de negócio e as interfaces de serviços.
- **SupportPlus.Infrastructure**: Camada de infraestrutura que contém implementações de serviços externos, como acesso a banco de dados e envio de e-mails.
- **SupportPlus.Tests**: Projeto de testes unitários e de integração para garantir a qualidade do código.

## Modelo de Negócio do Projeto de Exemplo

O modelo de negócio do SupportPlus consiste em um sistema de suporte onde os usuários podem abrir tickets para relatar problemas ou solicitar assistência. Os tickets podem conter múltiplos comentários para comunicação entre o usuário e os agentes de suporte. Além disso, o sistema oferece recursos de confirmação de e-mail para autenticação de usuários e auditoria para registro de atividades importantes.

## Formas de Uso

Para utilizar o SupportPlus, siga estas etapas:

1. Clone o repositório do GitHub: `git clone https://github.com/rubenslyra/supportplus-netcore-8`
2. Abra o projeto em sua IDE favorita.
3. Configure as credenciais de acesso ao banco de dados PostgreSQL no arquivo `appsettings.json` na camada de infraestrutura.
4. Execute as migrações do Entity Framework Core para criar o banco de dados: `dotnet ef database update`.
5. Execute o projeto e explore a API usando Swagger ou faça chamadas diretas para as rotas de API documentadas.

## Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests para melhorar o projeto.

## Licença

Este projeto está licenciado sob a licença MIT. Consulte o arquivo `LICENSE` para obter mais detalhes.

![Desktopa](https://github.com/rubenslyra/supportplus-netcore-8/assets/37023108/737b3c14-9f4d-46a3-9eb2-308435b99123)


