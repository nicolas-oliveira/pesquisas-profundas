<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# Guia Completo: Setup de Back-end Node.js com Nest.js + Prisma + Docker + Nginx + PostgreSQL

## Visão Geral da Arquitetura

Esta stack utiliza:

- **Nest.js** (framework back-end em TypeScript)
- **Prisma** (ORM moderno com type-safety)
- **PostgreSQL** (banco de dados relacional)
- **Docker \& Docker Compose** (containerização de todos os serviços)
- **Nginx** (proxy reverso para HTTPS e balanceamento)

Ao final deste guia, você terá um ambiente dockerizado pronto para desenvolvimento e produção, com:

1. Serviços isolados em containers
2. Proxy reverso e TLS via Nginx
3. Conexão persistente com PostgreSQL
4. Migrations e client Prisma integrados

## 1. Estrutura de Pastas

No diretório raiz do projeto, crie:

```
project-root/
├── app/                      # Código-fonte Nest.js
│   ├── src/
│   ├── prisma/
│   │   └── schema.prisma
│   ├── .env
│   ├── Dockerfile
│   └── package.json
├── docker-compose.yml
└── nginx/
    ├── Dockerfile
    └── nginx.conf
```


## 2. Configuração do Nest.js + Prisma

### 2.1 Inicializar projeto Nest.js com TypeScript

```bash
npm init -y
npm install @nestjs/cli
npx nest new app --package-manager npm --strict
```


### 2.2 Instalar Prisma

```bash
cd app
npm install prisma @prisma/client
npx prisma init --datasource-provider postgresql
```

- Arquivo `app/prisma/schema.prisma` deve apontar para a variável `DATABASE_URL` do `.env`[^1].


### 2.3 Definir o schema Prisma

Em `app/prisma/schema.prisma`:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String?
}
```


### 2.4 Variáveis de ambiente

Em `app/.env`:

```
DATABASE_URL=postgresql://postgres:example@db:5432/mydb?schema=public
```


## 3. Dockerfile da Aplicação Nest.js

Em `app/Dockerfile`:

```dockerfile
# Stage 1: build
FROM node:18-alpine AS builder
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: runtime
FROM node:18-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=builder /usr/src/app/dist ./dist
COPY prisma ./prisma
EXPOSE 3000
CMD ["node", "dist/main"]
```


## 4. Dockerfile do Nginx Proxy

Em `nginx/Dockerfile`:

```dockerfile
FROM nginx:stable-alpine
COPY nginx.conf /etc/nginx/nginx.conf
```

Em `nginx/nginx.conf`:

```nginx
events {}
http {
  upstream nestjs {
    server app:3000;
  }

  server {
    listen       80;
    server_name  _;

    location / {
      proxy_pass         http://nestjs;
      proxy_set_header   Host $host;
      proxy_set_header   X-Real-IP $remote_addr;
      proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }
}
```


## 5. Docker Compose

Em `docker-compose.yml`:

```yaml
version: "3.8"
services:
  app:
    build:
      context: ./app
    container_name: nest_app
    env_file: ./app/.env
    volumes:
      - ./app:/usr/src/app
    depends_on:
      - db
    networks:
      - backend

  db:
    image: postgres:15-alpine
    container_name: postgres_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: example
      POSTGRES_DB: mydb
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - backend

  nginx:
    build:
      context: ./nginx
    container_name: nginx_proxy
    ports:
      - "80:80"
    depends_on:
      - app
    networks:
      - backend

volumes:
  pgdata:

networks:
  backend:
```


## 6. Fluxo de Migrations e Prisma Client

1. **Criar migration**:

```bash
docker compose run app npx prisma migrate dev --name init
```

2. **Gerar client** (caso alterações no schema):

```bash
docker compose run app npx prisma generate
```

3. **Iniciar containers**:

```bash
docker compose up --build -d
```


## 7. Testando a Aplicação

- Acesse `http://localhost` no navegador: Nginx encaminhará para o Nest.js.
- Endpoints REST do Nest.js estarão em `http://localhost/users` (conforme controller criado).


## 8. Observações Finais

- Para adicionar HTTPS, inclua certificados e altere `nginx.conf` para `listen 443 ssl`.
- Em produção, utilize `.env.prod` com credenciais seguras e `docker compose -f docker-compose.yml -f docker-compose.prod.yml up`.
- Ajuste `volumes` conforme necessidade de hot-reload ou ambientes imutáveis.

Pronto! Sua API Nest.js com Prisma, PostgreSQL e Nginx em Docker está configurada e operacional.

<div style="text-align: center">⁂</div>

[^1]: https://www.prisma.io/docs/guides/docker

[^2]: https://www.youtube.com/watch?v=iD3TJJBk_Us

[^3]: https://www.tomray.dev/nestjs-docker-compose-postgres

[^4]: https://stackoverflow.com/questions/74854061/nestjs-doesnt-handle-request-from-nginx-proxy-pass-using-docker-compose

[^5]: https://www.prisma.io/blog/nestjs-prisma-rest-api-7D056s1BmOL0

[^6]: https://solomou.dev/blog/2024-05-13-nestjs-with-docker-compose-for-development

[^7]: https://www.digitalocean.com/community/tutorials/how-to-secure-a-containerized-node-js-application-with-nginx-let-s-encrypt-and-docker-compose

[^8]: https://www.youtube.com/watch?v=6G_EO6wRh2Y

[^9]: https://www.youtube.com/watch?v=o0fMQtmciFY

[^10]: https://www.bithost.in/blog/tech-2/docker-for-a-nest-js-app-with-postgresql-and-nginx-reverse-proxy-37

[^11]: https://dev.to/manuchehr/dockerize-nestjs-app-with-postgres-redis-prisma-orm-1130

[^12]: https://github.com/rogerioadris/nestjs-docker/blob/master/docker-compose.yml

[^13]: https://dev.to/dinhkhai0201/load-balancing-with-docker-compose-nginx-nestjs-220p

[^14]: https://notiz.dev/blog/dockerizing-nestjs-with-prisma-and-postgresql/

[^15]: https://dev.to/chukwutosin_/step-by-step-guide-setting-up-a-nestjs-application-with-docker-and-postgresql-5hei

[^16]: https://blog.logrocket.com/how-to-run-node-js-server-nginx/

[^17]: https://www.youtube.com/watch?v=fqr0mhQrASU

[^18]: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-nestjs-application-with-nginx-on-ubuntu

[^19]: https://www.freecodecamp.org/news/build-a-crud-rest-api-with-nestjs-docker-swagger-prisma/

[^20]: https://wanago.io/2023/01/16/api-nestjs-docker-compose/

