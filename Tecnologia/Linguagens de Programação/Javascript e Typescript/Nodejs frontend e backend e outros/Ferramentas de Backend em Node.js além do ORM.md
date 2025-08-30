# Ferramentas de Backend em Node.js além do ORM

Em um ecossistema de backend Node.js, diversas ferramentas complementam o ORM (Object-Relational Mapping) para resolver problemas comuns de servidores, APIs e integração com serviços externos. Essas ferramentas se agrupam em **tipos** — conjuntos que compartilham responsabilidades e padrões de uso. Abaixo, apresentam-se os principais tipos e exemplos representativos, detalhando o problema que cada ferramenta resolve e as nuances técnicas que justificam sua classificação.

## 1. Frameworks de Roteamento e Middleware

Essas bibliotecas definem como as requisições HTTP são recebidas, processadas e encaminhadas pelos "middlewares" (funções que interceptam a requisição/resposta).

| Ferramenta     | Tipo                                 | O que resolve                                                                                        | Nuances                                                                                                                                                            |
| -------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Express.js** | *Microframework HTTP*                | Gerenciamento de rotas, parsing de corpo de requisição, tratamento de erros, pipelines de middleware | Extensível via plugins ("express.json", "express.Router"). Não impõe arquitetura (à escolha do dev), o que traz flexibilidade mas exige disciplina de estruturação |
| **Koa.js**     | *Microframework HTTP assíncrono*     | Similar ao Express, mas com foco em async/await e composição de middleware via generator functions   | Contexto (`ctx`) unificado para requisição e resposta. Não inclui middleware embutido, forçando seleção explícita de componentes (must-use)                        |
| **NestJS**     | *Framework opinativo de arquitetura* | Oferece estrutura modular, injeção de dependência e padrões de projeto do mundo Java/C#              | Baseado em decorators do TypeScript para definir controllers, providers e módulos. Suporta GraphQL, WebSockets e micro-services de forma integrada                 |

## 2. Validação de Dados

Bibliotecas dedicadas a garantir integridade e conformidade dos dados vindos de usuários, APIs ou bancos.

| Ferramenta | Tipo                                        | O que resolve                                                                              | Nuances                                                                                                                       |
| ---------- | ------------------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| **Joi**    | *Validador de schema*                       | Define schemas declarativos para objetos, strings, números e arrays; gera erros detalhados | Permite composição de schemas e validação síncrona ou assíncrona. Gatilhos customizados (`.custom()`) para regras específicas |
| **Yup**    | *Validador de schema inspirado em promises* | Similar ao Joi, mas leve e com API baseada em chaining de promessas                        | Integração fácil com formulários front-end (React Hook Form, Formik). Suporta transformações de dados além de validação       |

## 3. Autenticação e Autorização

Ferramentas para gerenciar identidade, sessões e permissões de acesso.

| Ferramenta                  | Tipo                                 | O que resolve                                                                | Nuances                                                                                                            |
| --------------------------- | ------------------------------------ | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Passport.js**             | *Middleware de autenticação*         | Abstrai estratégias (OAuth, JWT, local, SAML) com interface padronizada      | Mais de 500 "strategies" oficiais e da comunidade. Integração simples: `passport.use()`, `passport.authenticate()` |
| **Auth0 SDK / NextAuth.js** | *Framework de autenticação completo* | Gerencia fluxos OAuth, OpenID Connect e sessões sem escrever backend de auth | Centralização de identidade via provedores externos. Geração automática de rotas de login, callback e logout       |

## 4. Logging e Monitoramento

Bibliotecas para registrar eventos, erros e métricas de performance.

| Ferramenta  | Tipo                      | O que resolve                                                      | Nuances                                                                                                           |
| ----------- | ------------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| **Winston** | *Logger multi‐transporte* | Registros em diferentes transports (console, arquivo, HTTP, banco) | Permite níveis de log customizados e formatos (JSON, texto). Gerenciamento de exceptions e rejections de promises |
| **Pino**    | *Logger ultrarrápido*     | Performance de logging para alta concorrência                      | Serialização otimizada em JSON. CLI companion para leitura e filtragem                                            |

## 5. Testes e Mocking

Ferramentas para garantir qualidade de código por meio de testes unitários, de integração e mocks.

| Ferramenta               | Tipo                                     | O que resolve                                                                                          | Nuances                                                                                                           |
| ------------------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| **Jest**                 | *Test runner completo*                   | Execução de testes, mocks de módulos e timers, cobertura de código                                     | Snapshot testing para componentes React e objetos complexos. Configuração zero‐config para a maioria dos projetos |
| **Mocha + Chai + Sinon** | *Test runner + assertions + spies/mocks* | Combinação flexível de runner (Mocha), biblioteca de asserções (Chai) e ferramentas de mocking (Sinon) | Suporta estilos BDD e TDD. Requer configuração manual de cobertura e reporters                                    |

## 6. Filas de Mensagens e Tarefas Assíncronas

Ferramentas para orquestrar jobs em background, processar filas e distribuir carga.

| Ferramenta | Tipo                             | O que resolve                                                 | Nuances                                                                                    |
| ---------- | -------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **BullMQ** | *Queue manager baseado em Redis* | Enfileirar jobs, retries automáticos e agendamento de tarefas | Dashboard oficial para monitoramento. Suporta fluxos (flows) e cadeias de jobs dependentes |
| **Agenda** | *Scheduler baseado em MongoDB*   | Agendamento de jobs recorrentes, persistência em Mongo        | MongoDB-driven, ideal para stacks que já usam esse banco. API de cron-like jobs            |

## 7. Documentação e Teste de API

Ferramentas para gerar, validar e testar especificações de API.

| Ferramenta                                 | Tipo                                 | O que resolve                                                               | Nuances                                                                                            |
| ------------------------------------------ | ------------------------------------ | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **Swagger (OpenAPI) + swagger-ui-express** | *Documentador de API*                | Gera documentação interativa a partir de especificação OpenAPI              | Anotações via YAML/JSON ou decorators (NestJS). UI customizável para execução de chamadas de teste |
| **Postman / Newman**                       | *Client de API + runner de coleções* | Envios de requisições, validações de resposta e execução automatizada em CI | Collections exportáveis para versão de controle. Scripts pré e pós‐requisição em JavaScript        |

## Por que cada grupo é um "tipo"?

Cada **tipo** agrupa ferramentas que compartilham o mesmo domínio de responsabilidade no backend:

- **Frameworks de Roteamento** tratam do fluxo de requisições HTTP e organização de código.
- **Validação** garante a conformidade estrutural e semântica dos dados.
- **Autenticação/Autorização** lida com identidade e permissões.
- **Logging** registra comportamento e erros em ambientes de produção.
- **Testes** asseguram qualidade e evitam regressões.
- **Filas** desacoplam processos pesados e tarefas assíncronas.
- **Documentação** padroniza e facilita o consumo de APIs.

Ao classificar as ferramentas por tipo, é possível estruturar o backend de forma modular e consistente, atribuindo responsabilidades claras a cada camada do sistema e promovendo maior manutenibilidade, escalabilidade e testeabilidade.
