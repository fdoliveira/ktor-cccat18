# KotlinBR

## ktor-cccat18 - Roteiro Curso KotlinBR

Este repositório contém um guia passo a passo para configurar e executar um projeto Kotlin com Ktor, incluindo a configuração de um banco de dados PostgreSQL usando Docker.

Este conteúdo é um 'fork', do curso Clean Code e Clean Architecture da Branas.io o qual recomendo fortemente.
https://www.branas.io/

O intuito da comunidade é refazer o código do curso feito originalmente em Typescript e NodeJS para Kotlin e Ktor e assim ajudar a comunidade a aprender Kotlin e Ktor.

Cada aula terá uma branch específica, exemplo aula 1 branch ktor-cccat18-1, e o código final estará na branch main.

Branch main será atualizada conforme o andamento do curso.

### O que é Kotlin e Ktor?

[Kotlin](https://kotlinlang.org/) é uma linguagem de programação moderna, concisa e segura, muito usada no desenvolvimento Android e backend.  
[Ktor](https://ktor.io/) é um framework para construir aplicações web e APIs em Kotlin de forma simples e rápida.

---

## 🗂️ Índice
1. [Pré-requisitos](#pré-requisitos)
2. [Criar e rodar o projeto inicial](#criar-e-rodar-o-projeto-inicial)
3. [Configurar o banco de dados](#configurar-o-banco-de-dados)

---

## 📋 Pré-requisitos

Certifique-se de ter as seguintes ferramentas instaladas no seu ambiente:

1. [IntelliJ IDEA Community Edition](https://www.jetbrains.com/pt-br/idea/download#community-edition)


2. Java 17

    #### Instalar Java 17 (via SDKMAN - recomendado)
    
    Para instalar o Java 17 no Linux ou Mac usando o SDKMAN, siga os passos abaixo:
    ```bash
    curl -s "https://get.sdkman.io" | bash
    source "$HOME/.sdkman/bin/sdkman-init.sh"
    sdk install java 17.0.7-tem
    ```
    Para instalar o Java 17 no Windows, veja o vídeo abaixo:
    
    https://www.youtube.com/watch?v=oyYtHrihThk&ab_channel=JagodeVreede


3. [Docker](https://www.docker.com/)

    - [Instalação do Docker no Linux](https://docs.docker.com/engine/install/)
    - [Instalação do Docker no Mac](https://docs.docker.com/desktop/install/mac-install/)
    - [Instalação do Docker no Windows](https://docs.docker.com/desktop/install/windows-install/)


4. Uma IDE para PostgreSQL (ex.: [DBeaver](https://dbeaver.io/) ou [pgAdmin](https://www.pgadmin.org/))

---

## 😎 Criar e rodar o projeto inicial

### 🥳 Passo 1: Criar o projeto e baixar o código
1. Acesse o site oficial do [Ktor](https://ktor.io/).
2. Clique no botão Start para configurar o novo projeto
3. Configure o projeto:

- Project artifact: `com.kotlinbr.ktor-cccat18`
- Plugins:
  - Routing
    1. Routing(https://start.ktor.io/p/routing)
  - Serialization
    1. Content Negotiation(https://start.ktor.io/p/content-negotiation)
    2. Kotlinx Serialization(https://start.ktor.io/p/kotlinx-serialization)
  - Databases
    1. Postgres(https://start.ktor.io/p/postgres)

4. Clique no botão de Download, extraia o projeto nas pasta de sua preferência e abra-o no IntelliJ IDEA.

### 📁 Passo 2: Estrutura de pastas do projeto
Após abrir o projeto, a estrutura de arquivos e pastas será a seguinte:

```
/
├── gradle/
│   └── wrapper/                 # Arquivos do Gradle Wrapper
├── src/
│   ├── main/
│   │   ├── kotlin/              # Código-fonte principal em Kotlin
│   │   │   ├── Application.kt   # Ponto de entrada do aplicativo.
│   │   │   ├── CitySchema.kt    # Configuração do schema do banco de dados.
│   │   │   ├── Database.kt      # Configuração do banco de dados.
│   │   │   ├── Routing.kt       # Configuração de rotas.
│   │   │   └── Serialization.kt # Configuração de serialização.
│   │   └── resources/           # Arquivos de configuração e recursos
│   │       └── application.yaml # Configurações do aplicativo.
│   │       └── logback.xml      # Configuração do log.
│   └── test/
│       └── kotlin/              # Testes do projeto
├── .gitignore                   # Arquivos e pastas ignorados pelo Git
├── build.gradle.kts             # Configuração do Gradle
├── gradle.properties            # Propriedades do Gradle
├── settings.gradle.kts          # Configuração do projeto Gradle
├── docker-compose.yaml          # Configuração do Docker
├── schema.sql                   # Script SQL para inicialização do banco de dados
└── README.md                    # Documentação do projeto
 
```

### ▶️ Passo 3: Rodar o projeto
1. No IntelliJ IDEA, localize a classe `Application.kt`.

2. Execute o método `main` para iniciar o servidor. 

    No terminal, você verá uma saída semelhante a esta:
    
    ```
    2025-04-11 04:55:42.187 [main] INFO  Application - Autoreload is disabled because the development mode is off.
    2025-04-11 04:55:42.269 [main] INFO  Application - Using embedded H2 database for testing; replace this flag to use postgres
    2025-04-11 04:55:42.363 [main] INFO  Application - Application started in 0.283 seconds.
    2025-04-11 04:55:42.452 [DefaultDispatcher-worker-1] INFO  Application - Responding at http://0.0.0.0:8080
    ```

### 🧪 Passo 4. Testar a API

Você pode testar se o servidor está funcionando corretamente com o seguinte comando:

```bash
curl http://localhost:8080
```

---

## 🏦 Configurar o banco de dados

### 📐 Passo 1: Criar o arquivo `schema.sql`
Crie um arquivo chamado `schema.sql` na raiz do projeto com o seguinte conteúdo:

```sqldelight
DROP SCHEMA IF EXISTS ccca CASCADE;

CREATE SCHEMA ccca;

CREATE TABLE ccca.account (
  account_id    UUID    PRIMARY KEY,
  name          TEXT    NOT NULL,
  email         TEXT    NOT NULL,
  cpf           TEXT    NOT NULL,
  car_plate     TEXT,
  is_passenger  BOOLEAN NOT NULL DEFAULT FALSE,
  is_driver     BOOLEAN NOT NULL DEFAULT FALSE,
  password      TEXT    NOT NULL
);
```

### ⚙️ Passo 2: Criar o arquivo `docker-compose.yaml`
Crie um arquivo chamado `docker-compose.yaml` na raiz do projeto com o seguinte conteúdo:

```yaml
services:
  postgres:
    image: postgres:17
    environment:
      POSTGRES_DB: app
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 123456
    ports:
      - "5432:5432"
    volumes:
      - ./schema.sql:/docker-entrypoint-initdb.d/init.sql
```

### 🏃🏻️ Passo 3: Rodar o container
1. No terminal, execute o comando:
   ```bash
   docker-compose up -d
   ```

2. Verifique se o container está rodando com o comando:
   ```bash
    docker ps
    ```

Você deve ver uma saída semelhante a esta:

```
CONTAINER ID   IMAGE         COMMAND                  CREATED              STATUS          PORTS                    NAMES
ba95a74a2be1   postgres:17   "docker-entrypoint.s…"   About a minute ago   Up 16 seconds   0.0.0.0:5432->5432/tcp   ktor-cccat18-postgres-1
```

### 🗄️ Passo 4: Acessar o banco de dados
1. Abra sua IDE de banco de dados (ex.: DBeaver ou pgAdmin).
2. Crie uma nova conexão com as seguintes configurações:
   - Host: `localhost`
   - Porta: `5432`
   - Nome do banco de dados: `app`
   - Usuário: `postgres`
   - Senha: `123456`

3. Teste a conexão e, se tudo estiver correto, clique em "Conectar".
4. Verifique se a tabela `account` foi criada corretamente.
5. Você pode usar o seguinte comando SQL para verificar a tabela:

```sql
SELECT * FROM ccca.account;
```


## ✅ Checklist rápido

- [ ] Java 17 instalado
- [ ] Docker rodando
- [ ] Projeto aberto no IntelliJ
- [ ] Servidor responde em `http://localhost:8080`
- [ ] Banco de dados PostgreSQL rodando
- [ ] Tabela `account` visível na IDE de banco


### 🤝 Contribuição
Contribuições são bem-vindas! Se você encontrar algum erro ou tiver sugestões de melhorias, sinta-se à vontade para abrir uma issue ou enviar um pull request.

### 📄 Licença
Este projeto está licenciado sob a [MIT License](LICENSE).