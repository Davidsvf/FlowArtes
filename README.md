# FlowArtes — Plataforma de Gestão da Escola de Dança Ent'Artes

O FlowArtes é uma aplicação web full-stack desenvolvida para apoiar a gestão da escola de dança **Ent'Artes**, em Portugal.

O projeto foi criado com o objetivo de centralizar processos administrativos e operacionais que anteriormente estavam distribuídos por diferentes ferramentas, folhas de cálculo e processos manuais.

A aplicação permite gerir alunos, encarregados de educação, professores, aulas, horários, inventário, eventos, notificações e faturação, garantindo diferentes níveis de acesso consoante o perfil do utilizador.

---

## Missão do Projeto

A missão do FlowArtes é simplificar e centralizar a gestão diária da Ent'Artes, reduzindo tarefas administrativas repetitivas e facilitando o acesso à informação.

A plataforma pretende funcionar como um ponto central de interação entre:

- Administração;
- Staff;
- Professores;
- Encarregados de educação.

O sistema foi desenvolvido tendo como base necessidades reais de uma escola de dança, procurando organizar processos que normalmente se encontram dispersos por várias ferramentas.

Para além da gestão de dados, o projeto implementa regras de negócio relacionadas com o funcionamento da escola, como:

- pedidos e aprovação de aulas;
- validação de participantes;
- gestão da disponibilidade dos professores;
- acompanhamento do ciclo de vida das aulas;
- controlo de acessos por perfil;
- requisições de material;
- gestão de faturação;
- eventos e notificações.

Este projeto foi desenvolvido em contexto académico, utilizando a Ent'Artes como caso de estudo real.

---

## Principais Funcionalidades

### Autenticação e Controlo de Acessos

A aplicação suporta quatro perfis de utilizador:

| Perfil | Descrição |
|---|---|
| `admin` | Administração da plataforma |
| `staff` | Gestão da escola / Direção |
| `coach` | Professores |
| `parent` | Encarregados de educação |

Cada perfil possui acesso a funcionalidades específicas de acordo com as respetivas responsabilidades.

---

### Gestão de Alunos

Permite gerir a informação dos alunos da escola e a respetiva associação aos encarregados de educação.

---

### Gestão de Professores

Os professores podem consultar e gerir informação relacionada com:

- aulas;
- participantes;
- disponibilidade;
- validações.

---

### Gestão de Aulas

O sistema implementa um fluxo completo de gestão de aulas, incluindo:

- pedido de aulas;
- aprovação por parte do staff;
- gestão de participantes;
- validação por professores e encarregados de educação;
- cancelamento;
- conclusão;
- transições automáticas de estado.

---

## Ciclo de Vida das Aulas

As aulas seguem diferentes estados ao longo do seu ciclo de vida:

```text
Requested
   |
   v
StaffApproved
   |
   v
Approved
   |
   v
Finished
   |
   v
Pending
   |
   v
Validated
```

Existem ainda os estados:

```text
Rejected
Cancelled
```

Algumas das transições são realizadas automaticamente pelo sistema através do `ClassLifecycleWorker`.

Por exemplo:

```text
Approved -> Finished
Finished -> Pending
```

Este serviço é executado em background e verifica periodicamente as aulas que necessitam de alteração automática de estado.

---

## Agenda e Disponibilidades

A aplicação inclui funcionalidades relacionadas com:

- gestão de horários;
- disponibilidade dos professores;
- planeamento das aulas;
- prevenção de conflitos de horários.

---

## Inventário

O sistema permite gerir materiais e equipamentos da escola, incluindo processos de requisição e respetivo acompanhamento.

---

## Eventos e Notificações

A plataforma contempla funcionalidades para:

- gestão de eventos;
- criação de notificações;
- acompanhamento de informação relevante pelos diferentes utilizadores.

---

## Faturação

O projeto inclui funcionalidades relacionadas com a gestão e acompanhamento da faturação associada às atividades da escola.

---

## Tecnologias Utilizadas

| Área | Tecnologia |
|---|---|
| Backend | ASP.NET Core 10 |
| Linguagem Backend | C# |
| ORM | Entity Framework Core 10 |
| Base de Dados | SQL Server |
| Base de Dados de Testes | SQLite |
| Frontend | React 19 |
| Build Tool | Vite |
| Routing | React Router |
| Autenticação | JWT através de cookie HttpOnly |
| Testes | xUnit |
| Mocking | Moq |
| Assertions | FluentAssertions |
| Testes de Integração | WebApplicationFactory |
| API Testing | Postman |
| CI/CD | GitHub Actions |
| Cloud | Azure |

---

## Arquitetura

A aplicação segue uma arquitetura cliente-servidor.

```text
React
  |
  | HTTP / REST
  v
ASP.NET Core API
  |
  v
Entity Framework Core
  |
  v
SQL Server
```

O frontend comunica com a API através de endpoints REST.

O backend é responsável pela:

- lógica de negócio;
- autenticação;
- autorização;
- validação;
- acesso à base de dados;
- processamento das regras da aplicação.

---

## Estrutura do Projeto

```text
DanceSchoolApp.slnx
|
├── DanceSchoolApp.Server/
|   ├── Controllers/
|   ├── Services/
|   ├── Models/
|   ├── DTOs/
|   ├── Data/
|   └── Program.cs
|
├── danceschoolapp.client/
|   └── Aplicação React
|
├── DanceSchoolApp.Tests/
|   ├── Unit/
|   └── Integration/
|
├── Doc/
|   ├── V1BasicAPI.md
|   ├── V2AdvancedAPI.md
|   ├── V3TestsFramework.md
|   └── EndpointsMapping.md
|
├── PostmanCfg/
|
└── README.md
```

---

## Variáveis de Ambiente

A aplicação utiliza variáveis de ambiente para informação sensível e configuração externa.

| Variável | Finalidade |
|---|---|
| `DanceSchoolApp_DB` | Connection string do SQL Server |
| `DanceSchoolApp_JWT_Secret` | Chave utilizada para assinatura dos tokens JWT |
| `DanceSchoolApp_Email_Password` | Password utilizada no serviço SMTP |

As credenciais reais não devem ser adicionadas ao repositório.

---

## Como Executar

### Backend

```bash
cd DanceSchoolApp.Server
dotnet build
dotnet run
```

Durante o desenvolvimento, a API é executada localmente e o frontend pode comunicar com o backend através do proxy configurado no Vite.

---

### Frontend

```bash
cd danceschoolapp.client
npm install
npm run dev
```

---

### Visual Studio

Abrir:

```text
DanceSchoolApp.slnx
```

e executar o projeto `DanceSchoolApp.Server`.

A configuração do projeto permite iniciar o backend e o servidor de desenvolvimento do frontend.

---

## Testes

O projeto inclui testes unitários e testes de integração.

Para executar todos os testes:

```bash
dotnet test
```

Os testes abrangem diferentes áreas da aplicação, incluindo:

- autenticação;
- autorização;
- faturação;
- gestão de aulas;
- reservas;
- validações;
- regras de negócio;
- permissões;
- ciclo de vida das aulas.

Os testes de integração utilizam uma base de dados SQLite em memória, permitindo testar os endpoints e o comportamento da aplicação sem depender da base de dados de produção.

---

## Documentação

A pasta `Doc` contém documentação adicional relacionada com a API e o desenvolvimento do projeto.

| Documento | Descrição |
|---|---|
| `V1BasicAPI.md` | Endpoints base da aplicação |
| `V2AdvancedAPI.md` | Endpoints específicos dos diferentes perfis |
| `V3TestsFramework.md` | Estrutura de testes e melhorias introduzidas |
| `EndpointsMapping.md` | Relação entre páginas do frontend e endpoints da API |

A pasta `PostmanCfg` contém a coleção utilizada para testar os endpoints da API.

---

## Equipa

O projeto foi desenvolvido pela equipa **DRVP Tech**.

| Nome | Número de Aluno |
|---|---|
| João Pereira | a31505 |
| Paulo Silva | a31506 |
| David Faria | a31517 |
| Vítor Rezende | a31521 |
| Rafael Costa | a31524 |

**Instituição:** Instituto Politécnico do Cávado e do Ave — Escola Superior de Tecnologia  
**Curso:** Licenciatura em Engenharia de Sistemas Informáticos  
**Ano Letivo:** 2025/2026

---

## Estado do Projeto

O FlowArtes foi desenvolvido em contexto académico como solução para um caso de estudo baseado nas necessidades de uma escola de dança real.

O projeto demonstra a implementação de uma aplicação full-stack com:

- frontend e backend separados;
- API REST;
- autenticação e autorização;
- base de dados relacional;
- regras de negócio;
- testes automatizados;
- CI/CD;
- integração com serviços externos.

Algumas funcionalidades encontram-se completamente implementadas, enquanto outras estão preparadas para evolução futura.

---

## Objetivos de Aprendizagem

Durante o desenvolvimento deste projeto foram aplicados conhecimentos relacionados com:

- desenvolvimento full-stack;
- ASP.NET Core;
- React;
- APIs REST;
- autenticação e autorização;
- bases de dados relacionais;
- Entity Framework Core;
- testes unitários;
- testes de integração;
- arquitetura de software;
- Git e GitHub;
- CI/CD;
- desenvolvimento em equipa;
- documentação de software.

---

## Contexto Académico

Este repositório representa um projeto desenvolvido em contexto académico.

A aplicação não deve ser interpretada como um produto comercial final, mas como uma solução funcional criada para aplicar conceitos de engenharia de software a um problema próximo de um cenário real.