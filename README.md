Marketplace-Api

# Projeto de Microserviços: Auth-Service e Debt-Service

## Sumário

- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Arquitetura](#arquitetura)
- [Banco de Dados](#banco-de-dados)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Como Executar](#como-executar)
- [Endpoints Disponíveis](#endpoints-disponíveis)

## Tecnologias Utilizadas

- **Linguagem de Programação**: Go (Golang)
- **Framework Web**: Fiber
- **Banco de Dados**: MongoDB
- **Contêinerização**: Docker
- **Orquestração de Contêineres**: Docker Compose

## Arquitetura

### Microserviços

1. **Auth (Serviço de Autenticação)**
   - **Responsabilidade**: Gerenciamento de usuários, incluindo cadastro, login e logout.
   - **Endpoints**:
     - `"/", addUser`: Para cadastro de novos usuários.
     - `"/login", loginUser`: Para autenticação de usuários.
     - `"/logout", logout`: Para invalidar a sessão do usuário.
     - `"/", getAll`: Para listar todos os usuário.
     - `"/:id", getById`: Para listar todos os usuário por id.
     - `"/:id", updateUser`: Para atualizar informações do usuário.
     - `"/:id", deleteUser`: Para deletar informações do usuário.

2. **Debt (Serviço de Dívidas)**
   - **Responsabilidade**: Gerenciamento das dívidas dos usuários, incluindo a listagem de dívidas e cálculo do score de crédito.
   - **Endpoints**:
     - `"/", addDebt`: Para adicionar novas dívidas (restrito a administradores).
     - `"/", listDebts`:Para listar as dívidas do usuário autenticado.
     - `"/score", getScore`: Para calcular e retornar o score de crédito do usuário.

### Comunicação entre Microserviços

- **Autenticação**: Cada serviço tem seus próprios endpoints protegidos por autenticação. Isso pode ser implementado usando JWT (JSON Web Tokens), onde o token é gerado pelo `Auth-Service` durante o login e deve ser enviado nos cabeçalhos das requisições subsequentes para os serviços protegidos.

## Banco de Dados

- **MongoDB**: Utilizado como banco de dados principal para armazenar informações de usuários e dívidas. Cada microserviço pode ter suas próprias coleções dentro do MongoDB.
  - **Auth-Service**: Coleção para armazenar informações de usuários.
  - **Debt-Service**: Coleção para armazenar informações de dívidas.

## Estrutura do Projeto
````
AUTH/
internal/
├── Dockerfile
├── docker-compose.yml
├── go.mod
├── go.sum
├── main.go
├── db
│   └── connection.go
|   └── auth.go
|   └── customer.go
├── models
│   └── customer.go
|   └── auth.go
├── service
│   └── router.go
|   └── auth.go
|   └── customer.go
|    └── validAdmin.go

DEBT/
internal/
├── Dockerfile
├── docker-compose.yml
├── go.mod
├── go.sum
├── main.go
├── db
│   ├── connection.go
│   └── debt.go
├── middlewares
│   └── auth.go
├── models
│   ├── debt.go
├── service
│   ├── checkUserAdmin.go
│   └── debt.go
|    ├── router.go
│   └── score.go
````
### Como executar o projeto

- **marketplace-api**: Este repositório contém os serviços de autenticação e dívidas, dentro dele so tem um arquivo docker-compose.yml
para rodar os serviços.

## Como Executar

### Clone os Repositórioa (ou navegue até o diretório contendo docker-compose.yml):

```bash
git clone https://github.com/ragazoni/auth.git
git clone https://github.com/ragazoni/debt.git
git clone https://github.com/ragazoni/marketplace-api.git
cd marketplace-api
```

## Construir e iniciar os serviços
Precisa ter instalado na maquina docker, docker-compose.
execute comando abaixo para construir e iniciar os serviços, todas as configurações 
estão dentro do yml neste projeto.

```bash
docker-compose up --build
```

## Acessar serviços

auth: http://localhost:9001 
debts: http://localhost:3000

- **Fiber**: Subi o servidor do fiber localmente na maquina, no arquivo main.go de cada serviço
tem as configurações e a porta utilizada de cada serviço, pode altera-las se assim preferir.

```bash
go run main.go
```


 




