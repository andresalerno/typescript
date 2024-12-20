
## Estrutura de Projeto TS (backend/frontend) + Mobile

```typescript

project/
├── backend/               # Backend Node.js com TypeScript
│   ├── src/
│   │   ├── config/        # Configurações (Docker, banco de dados, etc.)
│   │   ├── domain/        # Lógica de domínio (entidades, regras de negócio)
│   │   ├── infrastructure/ # Integrações externas (MongoDB, Neo4j, Redis)
│   │   ├── application/   # Casos de uso e serviços (coordenadores da lógica)
│   │   ├── interfaces/    # Interfaces (APIs REST/GraphQL e adaptadores)
│   │   ├── shared/        # Utilitários e helpers comuns
│   │   └── main.ts        # Ponto de entrada do backend
│   ├── Dockerfile         # Dockerfile para o backend
│   └── package.json
├── frontend/              # Frontend React com TypeScript/JavaScript
│   ├── src/
│   │   ├── components/    # Componentes reutilizáveis
│   │   ├── pages/         # Páginas da aplicação (web e/ou mobile)
│   │   ├── hooks/         # Hooks customizados
│   │   ├── services/      # Chamadas a APIs e lógica do frontend
│   │   ├── styles/        # Estilização global e específica
│   │   ├── utils/         # Funções utilitárias
│   │   └── App.tsx        # Ponto de entrada do frontend
│   ├── Dockerfile         # Dockerfile para o frontend
│   └── package.json
├── mobile/                # Aplicação mobile React Native
│   ├── src/
│   │   ├── components/    # Componentes reutilizáveis
│   │   ├── screens/       # Telas da aplicação
│   │   ├── navigation/    # Configuração de rotas
│   │   ├── services/      # Chamadas a APIs e lógica do mobile
│   │   ├── styles/        # Estilização global e específica
│   │   ├── utils/         # Funções utilitárias
│   │   └── App.tsx        # Ponto de entrada do mobile
│   ├── Dockerfile         # Dockerfile para o mobile
│   └── package.json
├── docker-compose.yml     # Orquestração de serviços com Docker
└── README.md

```

## Backend

- Config (Configurações)
    - Configurações para Docker, Banco de Dados, Variáveis de ambiente, etc
- Domain (Domínio)
    - Contém entidades (modelos de negócio) e regras de negócio. Exemplo: User, Order e etc
- Infrastructure (Infraestrutura)
    - Integrações com bancos de dados (MongoDB, Neo4j) e cache (Redis)
    - Repositório para acesso a dados
    - Conexões e drivers de banco
- Application (Aplicação)
    - Casos de uso e lógica da aplicação
    - Exemplo: CreateUser, GetOrder e etc
- Interfaces
    - APIs (Rest/GraphQL)
    - Adaptação entre as camadas de aplicação e o mundo externo

## Frontend

- Components
    - Componetes reutilizáveis, como botões, inputs e etc
- Pages
    - Páginas completas, conectadas a rotas
- Hooks
    - Hooks customizados para lógica reutilizável
- Services
    - Lógica de chamadas à API (Axios ou Fetch)
- Utils
    - Funções utilitárias (formatação de datas e validações)

### Mobile (React Native com TS/JS)

- Components
    - Componentes reutilizáveis, otimizados para mobile
- Screens
    - Telas principais da aplicação
- Navigation
    - Gerenciamento de rotas
- Services
    - Conexão com APIs


## Benefícios da estrutura sugerida

- Flexibilidade: alterar ou adicionar bancos de dados exige mudanças mínimas, pois cada integração está isolada na camada da infraestrtura
- Testabilidade: interfaces desacopladas permitem criar mocks para testes unitários e integrar APIs no Postman
- Manutenção: a separação de camadas e o uso de SOLID tornam a aplicação fácil de escalar e manter
- Integração eficiente: bancos relacionais e não relacionais trabalham em conjunto sem sobrecarregar o sistema

## Bancos de Dados

### Relacionais

- Usado para gerenciar dados tabulares estruturados, como:
    - informações sobre bicicletas (nome, reço, descrição etc)
    - Usuários (nome, e-mail, endereço, etc)
    - Pedidos (quem comprou o quê, quando e onde)

- [PostgreSQL](https://www.postgresql.org/download/)
    - Recursos avançados (ex.: tipos de dados geográficos com PostGIS)
    - Suporte robusto para consultas complexas
- [MYSQL](https://dev.mysql.com/downloads/installer/)
    - Boa performance para leitura e escrita
    - Simples de configurar e escalar

### Não relacionais

#### Banco de dados para Arquivos e/ou Imagens

- Usado para armazenar informações menos estruturadas como:
    - descrições extensas
    - logs
    - metadados
    - flexibilidade com dados que podem mudar frequentemente

- [MongoDB](https://www.mongodb.com/try/download/community)
    - Perfeito para documentos (ex.: armazenar descrições de biciletas com metadados)
    - Suporte nativo para JSON, ideal para integrar com APIs

- [MinIO](https://min.io/open-source/download?platform=windows)
    - O MinIO é um Storage de Objetos, uma arquitetura de armazenamento de dados, que consegue lidar com uma abundância de informações não estruturadas, sem hierarquia.

#### Banco de dados para Cache

- Usado para melhorar a performance de consultas frequentes
    - Cache de produtos mais vistos
    - Armazenamento temporário de resultados de consultas

- [Redis](https://redis.io/docs/latest/get-started/)
    - Excelente para cache em tempo real
    - Oferece suprote para filas e contagem de visualizações

#### Baco de dados para relacionamentos complexos

- usado para modelar conexões complexas como:
    - relacionamentos entre usuários (ex.: vendedores e compradores)
    - bicicletas relacionadas ou complementares

- [Neo4j](https://neo4j.com/product/neo4j-graph-database/)
    - ideal para modelar e consultar grafos
    - Exemplo: "Quais bicicletas vendidas por usuários na minha área são similares a X?"

## RabbitMQ

O **RabbitMQ** pode ser usado como uma ferramenta essencial para implementar comunicação assíncrona no marketplace de biciletas. Ele é um message broker, ideal para sistemas que precisam processar e gerenciar tarefas em segundo plano ou comuniar diferentes serviços de forma desacoplada.

O RabbitMQ é um message broker que pode ser integrado em projetos como o descrito acima para lidar com comunicação assíncrona, filas de mensagens e processamento em background. Ele pode ser utilizado, por exemplo, para orquestrar tarefas demoradas, permitir comunicação entre microserviços, ou implementar padrões de pub/sub.

Aqui está uma abordagem para integrá-lo em seu projeto:

- Backend:
    - Infrastructure:
        - A integração principal do RabbitMQ ficaria dentro da pasta infrastructure, já que ele é uma ferramenta externa.
        - Crie um módulo para configurar o RabbitMQ e gerenciar as conexões e canais.
    - Application:
        - Use RabbitMQ para delegar tarefas assíncronas, como envio de e-mails, geração de relatórios, ou atualizações em massa.
        - Publique eventos relevantes que outras partes do sistema possam consumir, como "Usuário criado" ou "Pedido processado".

- Frontend:
    - Services:
        - O frontend raramente se comunica diretamente com o RabbitMQ. Em vez disso, ele pode consumir APIs ou WebSockets que o backend expõe e que utilizam RabbitMQ para obter notificações em tempo real ou atualizações de estado.

- Mobile:
    - Services:
        - Assim como no frontend, a camada mobile consumirá APIs que, por sua vez, podem ser integradas ao RabbitMQ para tratar eventos ou notificações.

Segue abaixo alguns exemplos:

### Processamento de imagens (fotos e imagens de satélite)

- Cenário
    - Quando um vendedor faz o upload de fotos das biciletas ou imagens de satélite, pode ser necessário redimensionar, otimizar ou processar essas imagens

Como usar o RabbitMQ:

#### Processamento de Imagens (fotos e imagens de satélite)

- Quando uma imagem é enviada para o sistema, um evento é publicado em uma fila do RabbitMQ com os detalhes da imagem (ex.: URL da imagem no S3)

- Um serviço especializado processa a imagem (redimensiona, otimiza) e atualiza o banco de dados com o status

#### Envio de notifcações (emails, push ou SMS)

- Cenário
    - O marketplace precisa notificar os usuários
        - quando um pedido é confirmado
        - quando um produto é marcado como favorito e está em promoção
        - quando o vendedor recebe uma nova avaliação




