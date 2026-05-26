# CloudLab11 - CI/CD with GitHub Actions

Este projeto demonstra uma pipeline de CI/CD que utiliza o GitHub Actions, Docker, Terraform e autenticação AWS com OIDC.
Inclui processos automatizados de build, testes, deploy e testes locais de workflows com `act`.

## Estrutura do Repositório

```text
A:.
│   .dockerignore
│   .gitignore
│   .java-version
│   .lombok.config
│   docker-compose.kafka.yml
│   docker-compose.yml
│   pom.xml
│   rapidosSubmits.txt
│   README.md
│   
├───.github
│   └───workflows
│           aws-test.yml
│           ci.yml
│           deploy.yml
│           hello.yml
│           image.yml
│           release.yml
│           reusable-image.yml
│           terraform.yml
│           
├───.idea
│       .gitignore
│       compiler.xml
│       encodings.xml
│       jarRepositories.xml
│       misc.xml
│       vcs.xml
│       workspace.xml
│       
├───.mvn
│       jvm.config
│       
├───api-gateway
│   │   Dockerfile
│   │   pom.xml
│   │   
│   ├───src
│   │   ├───main
│   │   │   ├───java
│   │   │   │   └───pt
│   │   │   │       └───ulusofona
│   │   │   │           └───apigateway
│   │   │   │               │   ApiGatewayApplication.java
│   │   │   │               │   
│   │   │   │               └───config
│   │   │   │                       GatewayConfig.java
│   │   │   │                       
│   │   │   └───resources
│   │   │           application.yml
│   │   │           
│   │   └───test
│   │       └───java
│   │           └───pt
│   │               └───ulusofona
│   │                   └───apigateway
│   │                       │   ApiGatewayApplicationTest.java
│   │                       │   ApiGatewayParameterizedTest.java
│   │                       │   
│   │                       └───config
│   │                               GatewayConfigTest.java
│   │                               
│   └───target
│       ├───classes
│       │   │   application.yml
│       │   │   
│       │   └───pt
│       │       └───ulusofona
│       │           └───apigateway
│       │               │   ApiGatewayApplication.class
│       │               │   
│       │               └───config
│       │                       GatewayConfig.class
│       │                       
│       ├───generated-sources
│       │   └───annotations
│       ├───generated-test-sources
│       │   └───test-annotations
│       ├───maven-status
│       │   └───maven-compiler-plugin
│       │       ├───compile
│       │       │   └───default-compile
│       │       │           createdFiles.lst
│       │       │           inputFiles.lst
│       │       │           
│       │       └───testCompile
│       │           └───default-testCompile
│       │                   createdFiles.lst
│       │                   inputFiles.lst
│       │                   
│       └───test-classes
│           └───pt
│               └───ulusofona
│                   └───apigateway
│                       │   ApiGatewayApplicationTest.class
│                       │   ApiGatewayParameterizedTest.class
│                       │   
│                       └───config
│                               GatewayConfigTest.class
│                               
├───order-service
│   │   Dockerfile
│   │   pom.xml
│   │   
│   ├───src
│   │   ├───main
│   │   │   ├───java
│   │   │   │   └───pt
│   │   │   │       └───ulusofona
│   │   │   │           └───orderservice
│   │   │   │               │   OrderServiceApplication.java
│   │   │   │               │   
│   │   │   │               ├───client
│   │   │   │               │       ProductResponse.java
│   │   │   │               │       ProductServiceClient.java
│   │   │   │               │       UserResponse.java
│   │   │   │               │       UserServiceClient.java
│   │   │   │               │       
│   │   │   │               ├───config
│   │   │   │               │       KafkaConfig.java
│   │   │   │               │       
│   │   │   │               ├───controller
│   │   │   │               │       GlobalExceptionHandler.java
│   │   │   │               │       OrderController.java
│   │   │   │               │       
│   │   │   │               ├───dto
│   │   │   │               │       OrderItemRequest.java
│   │   │   │               │       OrderItemResponse.java
│   │   │   │               │       OrderRequest.java
│   │   │   │               │       OrderResponse.java
│   │   │   │               │       
│   │   │   │               ├───event
│   │   │   │               │       OrderCreatedEvent.java
│   │   │   │               │       OrderItemEvent.java
│   │   │   │               │       OrderStatusChangedEvent.java
│   │   │   │               │       
│   │   │   │               ├───model
│   │   │   │               │       Order.java
│   │   │   │               │       OrderItem.java
│   │   │   │               │       OrderStatus.java
│   │   │   │               │       
│   │   │   │               ├───repository
│   │   │   │               │       OrderRepository.java
│   │   │   │               │       
│   │   │   │               ├───service
│   │   │   │               │       OrderService.java
│   │   │   │               │       
│   │   │   │               └───sqs
│   │   │   │                       OrderProductEventsSqsConfiguration.java
│   │   │   │                       OrderProductEventsSqsProperties.java
│   │   │   │                       ProductCreatedSqsPayload.java
│   │   │   │                       ProductEventSqsPollingConsumer.java
│   │   │   │                       
│   │   │   └───resources
│   │   │           application.yml
│   │   │           
│   │   └───test
│   │       ├───java
│   │       │   └───pt
│   │       │       └───ulusofona
│   │       │           └───orderservice
│   │       │               │   OrderServiceApplicationTest.java
│   │       │               │   
│   │       │               ├───controller
│   │       │               │       GlobalExceptionHandlerTest.java
│   │       │               │       OrderControllerParameterizedTest.java
│   │       │               │       OrderControllerTest.java
│   │       │               │       
│   │       │               ├───dto
│   │       │               │       OrderItemRequestTest.java
│   │       │               │       OrderItemResponseTest.java
│   │       │               │       OrderRequestTest.java
│   │       │               │       OrderResponseTest.java
│   │       │               │       
│   │       │               ├───model
│   │       │               │       OrderStatusTest.java
│   │       │               │       OrderTest.java
│   │       │               │       
│   │       │               ├───service
│   │       │               │       OrderServiceParameterizedTest.java
│   │       │               │       OrderServiceTest.java
│   │       │               │       
│   │       │               └───sqs
│   │       │                       ProductEventSqsPollingConsumerTest.java
│   │       │                       
│   │       └───resources
│   │               application-test.yml
│   │               
│   └───target
│       ├───classes
│       │   │   application.yml
│       │   │   
│       │   └───pt
│       │       └───ulusofona
│       │           └───orderservice
│       │               │   OrderServiceApplication.class
│       │               │   
│       │               ├───client
│       │               │       ProductResponse.class
│       │               │       ProductServiceClient.class
│       │               │       UserResponse.class
│       │               │       UserServiceClient.class
│       │               │       
│       │               ├───config
│       │               │       KafkaConfig.class
│       │               │       
│       │               ├───controller
│       │               │       GlobalExceptionHandler.class
│       │               │       OrderController.class
│       │               │       
│       │               ├───dto
│       │               │       OrderItemRequest.class
│       │               │       OrderItemResponse.class
│       │               │       OrderRequest.class
│       │               │       OrderResponse.class
│       │               │       
│       │               ├───event
│       │               │       OrderCreatedEvent.class
│       │               │       OrderItemEvent.class
│       │               │       OrderStatusChangedEvent.class
│       │               │       
│       │               ├───model
│       │               │       Order.class
│       │               │       OrderItem.class
│       │               │       OrderStatus.class
│       │               │       
│       │               ├───repository
│       │               │       OrderRepository.class
│       │               │       
│       │               ├───service
│       │               │       OrderService.class
│       │               │       
│       │               └───sqs
│       │                       OrderProductEventsSqsConfiguration.class
│       │                       OrderProductEventsSqsProperties.class
│       │                       ProductCreatedSqsPayload.class
│       │                       ProductEventSqsPollingConsumer.class
│       │                       
│       ├───generated-sources
│       │   └───annotations
│       ├───generated-test-sources
│       │   └───test-annotations
│       ├───maven-status
│       │   └───maven-compiler-plugin
│       │       ├───compile
│       │       │   └───default-compile
│       │       │           createdFiles.lst
│       │       │           inputFiles.lst
│       │       │           
│       │       └───testCompile
│       │           └───default-testCompile
│       │                   createdFiles.lst
│       │                   inputFiles.lst
│       │                   
│       └───test-classes
│           │   application-test.yml
│           │   
│           └───pt
│               └───ulusofona
│                   └───orderservice
│                       │   OrderServiceApplicationTest.class
│                       │   
│                       ├───controller
│                       │       GlobalExceptionHandlerTest.class
│                       │       OrderControllerParameterizedTest.class
│                       │       OrderControllerTest.class
│                       │       
│                       ├───dto
│                       │       OrderItemRequestTest.class
│                       │       OrderItemResponseTest.class
│                       │       OrderRequestTest.class
│                       │       OrderResponseTest.class
│                       │       
│                       ├───model
│                       │       OrderStatusTest.class
│                       │       OrderTest.class
│                       │       
│                       ├───service
│                       │       OrderServiceParameterizedTest.class
│                       │       OrderServiceTest.class
│                       │       
│                       └───sqs
│                               ProductEventSqsPollingConsumerTest.class
│                               
├───product-service
│   │   Dockerfile
│   │   pom.xml
│   │   
│   ├───src
│   │   ├───main
│   │   │   ├───java
│   │   │   │   └───pt
│   │   │   │       └───ulusofona
│   │   │   │           └───productservice
│   │   │   │               │   ProductServiceApplication.java
│   │   │   │               │   
│   │   │   │               ├───controller
│   │   │   │               │       GlobalExceptionHandler.java
│   │   │   │               │       ProductController.java
│   │   │   │               │       
│   │   │   │               ├───dto
│   │   │   │               │       ProductRequest.java
│   │   │   │               │       ProductResponse.java
│   │   │   │               │       
│   │   │   │               ├───event
│   │   │   │               │       OrderCreatedEvent.java
│   │   │   │               │       OrderItemEvent.java
│   │   │   │               │       ProductCreatedSqsEvent.java
│   │   │   │               │       
│   │   │   │               ├───model
│   │   │   │               │       Product.java
│   │   │   │               │       
│   │   │   │               ├───repository
│   │   │   │               │       ProductRepository.java
│   │   │   │               │       
│   │   │   │               ├───service
│   │   │   │               │       OrderEventConsumer.java
│   │   │   │               │       ProductService.java
│   │   │   │               │       
│   │   │   │               └───sqs
│   │   │   │                       ProductEventSqsPublisher.java
│   │   │   │                       ProductSqsConfiguration.java
│   │   │   │                       ProductSqsProperties.java
│   │   │   │                       
│   │   │   └───resources
│   │   │           application.yml
│   │   │           
│   │   └───test
│   │       ├───java
│   │       │   └───pt
│   │       │       └───ulusofona
│   │       │           └───productservice
│   │       │               │   ProductServiceApplicationTest.java
│   │       │               │   
│   │       │               ├───controller
│   │       │               │       GlobalExceptionHandlerTest.java
│   │       │               │       ProductControllerParameterizedTest.java
│   │       │               │       ProductControllerTest.java
│   │       │               │       
│   │       │               ├───dto
│   │       │               │       ProductRequestTest.java
│   │       │               │       ProductResponseTest.java
│   │       │               │       
│   │       │               ├───model
│   │       │               │       ProductTest.java
│   │       │               │       
│   │       │               ├───service
│   │       │               │       OrderEventConsumerTest.java
│   │       │               │       ProductServiceParameterizedTest.java
│   │       │               │       ProductServiceTest.java
│   │       │               │       
│   │       │               └───sqs
│   │       │                       ProductEventSqsPublisherTest.java
│   │       │                       
│   │       └───resources
│   │               application-test.yml
│   │               
│   └───target
│       ├───classes
│       │   │   application.yml
│       │   │   
│       │   └───pt
│       │       └───ulusofona
│       │           └───productservice
│       │               │   ProductServiceApplication.class
│       │               │   
│       │               ├───controller
│       │               │       GlobalExceptionHandler.class
│       │               │       ProductController.class
│       │               │       
│       │               ├───dto
│       │               │       ProductRequest.class
│       │               │       ProductResponse.class
│       │               │       
│       │               ├───event
│       │               │       OrderCreatedEvent.class
│       │               │       OrderItemEvent.class
│       │               │       ProductCreatedSqsEvent.class
│       │               │       
│       │               ├───model
│       │               │       Product.class
│       │               │       
│       │               ├───repository
│       │               │       ProductRepository.class
│       │               │       
│       │               ├───service
│       │               │       OrderEventConsumer.class
│       │               │       ProductService.class
│       │               │       
│       │               └───sqs
│       │                       ProductEventSqsPublisher.class
│       │                       ProductSqsConfiguration.class
│       │                       ProductSqsProperties.class
│       │                       
│       ├───generated-sources
│       │   └───annotations
│       ├───generated-test-sources
│       │   └───test-annotations
│       ├───maven-status
│       │   └───maven-compiler-plugin
│       │       ├───compile
│       │       │   └───default-compile
│       │       │           createdFiles.lst
│       │       │           inputFiles.lst
│       │       │           
│       │       └───testCompile
│       │           └───default-testCompile
│       │                   createdFiles.lst
│       │                   inputFiles.lst
│       │                   
│       └───test-classes
│           │   application-test.yml
│           │   
│           └───pt
│               └───ulusofona
│                   └───productservice
│                       │   ProductServiceApplicationTest.class
│                       │   
│                       ├───controller
│                       │       GlobalExceptionHandlerTest.class
│                       │       ProductControllerParameterizedTest.class
│                       │       ProductControllerTest.class
│                       │       
│                       ├───dto
│                       │       ProductRequestTest.class
│                       │       ProductResponseTest.class
│                       │       
│                       ├───model
│                       │       ProductTest.class
│                       │       
│                       ├───service
│                       │       OrderEventConsumerTest.class
│                       │       ProductServiceParameterizedTest.class
│                       │       ProductServiceTest.class
│                       │       
│                       └───sqs
│                               ProductEventSqsPublisherTest.class
│                               
├───terraform
│   │   .terraform.lock.hcl
│   │   main.tf
│   │   outputs.tf
│   │   README.md
│   │   terraform.tfstate
│   │   terraform.tfstate.backup
│   │   tfplan
│   │   variables.tf
│   │   
│   └───.terraform
│       └───providers
│           └───registry.terraform.io
│               └───hashicorp
│                   └───aws
│                       └───6.44.0
│                           └───windows_amd64
│                                   LICENSE.txt
│                                   terraform-provider-aws_v6.44.0_x5.exe
│                                   
└───user-service
│   Dockerfile
│   pom.xml
│   
├───src
│   ├───main
│   │   ├───java
│   │   │   └───pt
│   │   │       └───ulusofona
│   │   │           └───userservice
│   │   │               │   UserServiceApplication.java
│   │   │               │   
│   │   │               ├───controller
│   │   │               │       GlobalExceptionHandler.java
│   │   │               │       UserController.java
│   │   │               │       
│   │   │               ├───dto
│   │   │               │       UserRequest.java
│   │   │               │       UserResponse.java
│   │   │               │       
│   │   │               ├───model
│   │   │               │       User.java
│   │   │               │       
│   │   │               ├───repository
│   │   │               │       UserRepository.java
│   │   │               │       
│   │   │               └───service
│   │   │                       UserService.java
│   │   │                       
│   │   └───resources
│   │           application.yml
│   │           
│   └───test
│       ├───java
│       │   └───pt
│       │       └───ulusofona
│       │           └───userservice
│       │               │   UserServiceApplicationTest.java
│       │               │   
│       │               ├───controller
│       │               │       GlobalExceptionHandlerTest.java
│       │               │       UserControllerParameterizedTest.java
│       │               │       UserControllerTest.java
│       │               │       
│       │               ├───dto
│       │               │       UserRequestTest.java
│       │               │       UserResponseTest.java
│       │               │       
│       │               ├───model
│       │               │       UserTest.java
│       │               │       
│       │               └───service
│       │                       UserServiceParameterizedTest.java
│       │                       UserServiceTest.java
│       │                       
│       └───resources
│               application-test.yml
│               
└───target
├───classes
│   │   application.yml
│   │   
│   └───pt
│       └───ulusofona
│           └───userservice
│               │   UserServiceApplication.class
│               │   
│               ├───controller
│               │       GlobalExceptionHandler.class
│               │       UserController.class
│               │       
│               ├───dto
│               │       UserRequest.class
│               │       UserResponse.class
│               │       
│               ├───model
│               │       User.class
│               │       
│               ├───repository
│               │       UserRepository.class
│               │       
│               └───service
│                       UserService.class
│                       
├───generated-sources
│   └───annotations
├───generated-test-sources
│   └───test-annotations
├───maven-status
│   └───maven-compiler-plugin
│       ├───compile
│       │   └───default-compile
│       │           createdFiles.lst
│       │           inputFiles.lst
│       │           
│       └───testCompile
│           └───default-testCompile
│                   createdFiles.lst
│                   inputFiles.lst
│                   
└───test-classes
│   application-test.yml
│   
└───pt
└───ulusofona
└───userservice
│   UserServiceApplicationTest.class
│   
├───controller
│       GlobalExceptionHandlerTest.class
│       UserControllerParameterizedTest.class
│       UserControllerTest.class
│       
├───dto
│       UserRequestTest.class
│       UserResponseTest.class
│       
├───model
│       UserTest.class
│       
└───service
UserServiceParameterizedTest.class
UserServiceTest.class
```

### 1. Hello Actions (hello.yml)

- Workflow de demonstração
- Executa em push e manualmente
- Mostra contexto do GitHub (repo, branch, commit, autor)
- Lista ficheiros do projeto

Trigger em:
- push
- Manual

---

### 2. CI Pipeline (ci.yml)

- Compila aplicação Java (product-service)
- Executa testes unitários com Maven
- Instala dependências necessárias (Maven + JDK 21)
- Faz upload de relatórios de testes
- Gera resumo no GitHub Actions (`GITHUB_STEP_SUMMARY`)
- Envia notificação para Slack

Trigger em:
- push para `main`
- pull requests

---

### 3. AWS OIDC Test (aws-test.yml)

- Autenticação segura na AWS via OIDC
- Usa role definida em secret `AWS_ROLE_TO_ASSUME`
- Executa: aws sts get-caller-identity

Trigger em:
- Manual

---

### 4. Terraform Pipeline (terraform.yml)

- Executa infraestrutura como código (IAC) na pasta terraform/
- Inclui:
- - terraform fmt -check
- - terraform init
- - terraform validate
- - terraform plan
- Publica plano em comentários de Pull Request
- Executa terraform apply automaticamente em main

Trigger em:
- push para `main`, quando o diretorio terraform for modificado
- pull requests, quando o diretorio terraform for modificado

---

### 5. Build All Services (image.yml)

- Usa matrix build
- Compila e faz build de múltiplos serviços:
- - user-service
- - product-service
- - order-service
- Funcionalidades:
- - Faz login no Docker Hub
- - Build paralelo dos serviços
- - Push automático de imagens Docker
- Tagging:
- - latest
- - SHA do commit

Trigger em:
- push para `main`
- Manual

--

### 6. Reusable Image Workflow (reusable-image.yml)

- Workflow reutilizável
- Recebe input: nome do serviço
- Centraliza lógica de build e push Docker
- Evita duplicação de código

Trigger em:
- Quando for chamado

### 7. Release Workflow (release.yml)

- Trigger baseado em tags (v*)
- Reutiliza/chama reusable-image.yml
- Automatiza release de imagens Docker

Trigger em:
- Quando feito uma tag

---

### 8. Deploy Workflow (deploy.yml)

- Workflow responsável por simular o processo de Continuous Deployment (CD)
- Executa automaticamente sempre que há um push para a branch main
- O deploy é dividido em duas fases: build e deploy
- O job de deploy depende do sucesso do build (needs: build)
- O deploy está associado ao GitHub Environment production

Trigger em:
- push para `main`, precisa ser aprovado manualmente por um reviewer

---

## GitHub Secrets Necessários

- AWS_ROLE_TO_ASSUME -> Role IAM para AWS OIDC
- DOCKERHUB_USERNAME -> Username Docker Hub
- DOCKERHUB_TOKEN -> Token Docker Hub
- SLACK_WEBHOOK -> Webhook do Slack

## Docker Hub
As imagens são publicadas automaticamente no Docker Hub

Exemplo: docker.io/<username>/product-service

Tags utilizadas:
- latest
- SHA do commit

## Terraform (Infrastructure as Code)

Executado automaticamente pelo workflow terraform.yml.

Etapas:
- terraform fmt -check
- terraform init
- terraform validate
- terraform plan

Comportamento:
- Pull Request → executa plan e comenta no PR
- Push para main → executa apply automaticamente

## AWS OIDC (Resumo)

Este projeto utiliza autenticação OIDC com AWS:
- GitHub Actions solicita token temporário
- AWS valida identidade do repositório
- Assume uma IAM Role (gha-deployer)
- Não são usadas credenciais permanentes