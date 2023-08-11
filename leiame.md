# api em node js com express, prisma e postgresql (via docker)
source:
https://www.youtube.com/watch?v=gnq8ZY85UUM


yarn init -y
yarn add express cors dotenv bcrypt yup prisma @prisma/client
yarn prisma init
yarn add -D sucrase
yarn add -D nodemon
npx eslint --init -D
## configurar o nodemon
criar um arquivo na raiz de nome nodemon.json e inserir o seguinte conteúdo:
{
    "execMap": {
        "js":"node -r sucrase/register"
    }
}

## configurar o prisma
### editar o arquivo .env
DATABASE_URL="postgresql://pguser:pgpassword@localhost:5435/mydb?schema=public"
// tipo-de-banco://usuário:senha@localhost:porta/nome-do-banco?schema=public

### definir a estrutura das tabelas no arquivo schema.prisma (formato final do arquivo)
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  password  String
  phone     String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

### realizar a migration
yarn prisma migrate dev --name init


## docker - criar e rodar imagem do postgres
### criar na raiz um arquivo de nome docker-compose.yml com o seguinte conteúdo:

### para executar o container:
- iniciar o docker desktop  
- no terminal: docker-compose up (não usar o power shell, no mac o comando é "docker-compose up")

### para executar o container em segundo plano (liberar o terminal):
docker-compose up -d

### para ver os containers em uso:
docker ps

