# Tech challenge


- RM: 350505
- Grupo 53

## Repository:

### Documentation

- https://github.com/lfneves/tech-challenge-doc

### Projects

- https://github.com/lfneves/tech-challenge-order

- https://github.com/lfneves/tech-challenge-payment

- https://github.com/lfneves/tech-challenge-status


---
<br>
<br>

# Summary

- [Tech challenge](#tech-challenge)
  - [Repository:](#repository)
    - [Documentation](#documentation)
    - [Projects](#projects)
- [Summary](#summary)
- [1. Project Architecture](#1-project-architecture)
  - [1.1. Projects Automated Deployment/Test](#11-projects-automated-deploymenttest)
    - [1.1.1. Github Actions Workflow](#111-github-actions-workflow)
  - [1.2. Projects Coverage](#12-projects-coverage)
  - [1.3. Jacoco report](#13-jacoco-report)
  - [1.4. order-ws](#14-order-ws)
  - [1.5. payment-ws](#15-payment-ws)
  - [1.6. status-ws](#16-status-ws)
- [2. Projects structures:](#2-projects-structures)
  - [2.1. Terraform AWS EKS Cluster Deployment](#21-terraform-aws-eks-cluster-deployment)
    - [2.1.1. AWS Infra Terraform EKS](#211-aws-infra-terraform-eks)
    - [2.1.2. Prerequisites](#212-prerequisites)
    - [2.1.3. Repository](#213-repository)
  - [2.2. Terraform AWS RDS Database Deployment](#22-terraform-aws-rds-database-deployment)
    - [2.2.1. AWS Infra Terraform RDS](#221-aws-infra-terraform-rds)
  - [2.3. Justification for Choosing PostgreSQL for a Restaurant System](#23-justification-for-choosing-postgresql-for-a-restaurant-system)
  - [2.4. Database Diagram](#24-database-diagram)
    - [2.4.1. Application mvp](#241-application-mvp)
      - [2.4.1.1. This is a Spring Boot WebFlux application using Kotlin.](#2411-this-is-a-spring-boot-webflux-application-using-kotlin)
  - [2.5. 💡 Requirements](#25--requirements)
  - [2.6. Getting Started](#26-getting-started)
  - [2.7. Project Structure](#27-project-structure)
    - [2.7.1. Prerequisites](#271-prerequisites)
  - [2.8. Installation](#28-installation)
    - [2.8.1. Docker](#281-docker)
    - [2.8.2. Kubernetes (k8s)](#282-kubernetes-k8s)
      - [2.8.2.1. To initiate Kubernetes applications, execute the commands found within the "k8s" folder.](#2821-to-initiate-kubernetes-applications-execute-the-commands-found-within-the-k8s-folder)
      - [2.8.2.2. o access the application URL, use the following command:](#2822-o-access-the-application-url-use-the-following-command)
      - [2.8.2.3. Example output:](#2823-example-output)
      - [2.8.2.4. Metric Server](#2824-metric-server)
    - [2.8.3. Kubernetes (k8s) - Install with Helm](#283-kubernetes-k8s---install-with-helm)
      - [2.8.3.1. Helm uninstall](#2831-helm-uninstall)
  - [2.9. Deploy Github-actions](#29-deploy-github-actions)
    - [2.9.1. Integration Mercado Pago](#291-integration-mercado-pago)
    - [2.9.2. This project uses CommandLineRunner](#292-this-project-uses-commandlinerunner)
  - [2.10. The best way to use it as a suggestion is by using Postman](#210-the-best-way-to-use-it-as-a-suggestion-is-by-using-postman)
      - [2.10.0.1. A collection is available preconfigured in the project root](#21001-a-collection-is-available-preconfigured-in-the-project-root)
      - [2.10.0.2. Create new user example:](#21002-create-new-user-example)
      - [2.10.0.3. Login - Use the username (cpf) and password, then copy the token and use it in authenticated endpoints.](#21003-login---use-the-username-cpf-and-password-then-copy-the-token-and-use-it-in-authenticated-endpoints)
    - [2.10.1. 💡To make it easier use environment variables](#2101-to-make-it-easier-use-environment-variables)
      - [2.10.1.1. Place the command in the test tab on /api/auth/login-token](#21011-place-the-command-in-the-test-tab-on-apiauthlogin-token)
      - [2.10.1.2. Example:](#21012-example)
  - [2.11. ](#211-)
    - [2.11.1. This project also uses OpenAPI Specification (Swagger).](#2111-this-project-also-uses-openapi-specification-swagger)
      - [2.11.1.1. To access swagger use the URL:](#21111-to-access-swagger-use-the-url)
  - [2.12. Roadmap](#212-roadmap)
  - [2.13. License](#213-license)


# 1. Project Architecture

- Three microservices were developed: order-ws, payment-ws, and status-ws, which communicate via AWS SNS and SQS. 

- When an order is created, it is saved in a PostgreSQL database and a message is sent to the ORDER_TOPIC, which then delivers the message to both the STATUS_QUEUE and PAYMENT_QUEUE. 

- Upon receipt, these queues store the messages in MongoDB. Whenever a payment is made or there is a change in the order status, messages are sent to the queues to process the updated status.

<br>

![Alt text](images/software.png)


----
<br>

## 1.1. Projects Automated Deployment/Test

### 1.1.1. Github Actions Workflow

![Alt text](images/workflow.png)


- order-ws

![Alt text](images/deploy/order_github_actions.png)

- payment-ws

![Alt text](images/deploy/payment_github_actions.png)

- status-ws

![Alt text](images/deploy/status_github_actions.png)


## 1.2. Projects Coverage

![Alt text](images/sonar-coverage.png)


## 1.3. Jacoco report

- NOTE: The variance in test coverage between Jacoco and Sonar can be attributed to variations in behavior exhibited by certain filters and integration tests, leading to different or unexpected outcomes.


- The test files are located in the respective folders

## 1.4. order-ws

- Report file download folder and open in browser: [Link order-ws report](fase_4_test_report_jacoco/order-ws-reports/jacoco/test/html/index.html)

![Alt text](images/order_jacoco_report.png)


## 1.5. payment-ws

- Report file download folder and open in browser: [Link payment-ws report](fase_4_test_report_jacoco/payment-ws-reports/jacoco/test/html/index.html)

![Alt text](images/payment_jacoco_report.png)


## 1.6. status-ws

- Report file download folder and open in browser: [Link status-ws report](fase_4_test_report_jacoco/status-ws-reports/jacoco/test/html/index.html)

![Alt text](images/status_jacoco_report.png)

---

# 2. Projects structures:

- order-ws folders

```
└── src
   ├── main
   |  ├── kotlin
   |  |  └── com
   |  |     └── mvp
   |  |        └── order
   |  |           ├── application
   |  |           |  └── v1
   |  |           ├── domain
   |  |           |  ├── configuration
   |  |           |  ├── model
   |  |           |  |  ├── exception
   |  |           |  |  ├── order
   |  |           |  |  |  └── enums
   |  |           |  |  ├── product
   |  |           |  |  └── user
   |  |           |  └── service
   |  |           |     ├── message
   |  |           |     ├── order
   |  |           |     ├── product
   |  |           |     └── user
   |  |           ├── infrastruture
   |  |           |  ├── entity
   |  |           |  |  ├── order
   |  |           |  |  ├── product
   |  |           |  |  └── user
   |  |           |  └── repository
   |  |           |     ├── order
   |  |           |     ├── product
   |  |           |     └── user
   |  |           └── utils
   |  |              └── constants
   |  └── resources
   |     └── database
   └── test
      ├── kotlin
      |  └── com
      |     └── mvp
      |        └── order
      |           ├── application
      |           |  ├── bdd
      |           |  ├── integration
      |           |  |  ├── order
      |           |  |  |  ├── controller
      |           |  |  |  └── service
      |           |  |  └── user
      |           |  ├── message
      |           |  └── unit
      |           |     ├── order
      |           |     |  └── model
      |           |     ├── product
      |           |     └── user
      |           └── helpers
      └── resources
         ├── features
         ├── schemas
         └── sql

```


- payment-ws folders

```
└── src
   ├── main
   |  ├── kotlin
   |  |  └── com
   |  |     └── mvp
   |  |        └── payment
   |  |           ├── application
   |  |           |  └── v1
   |  |           ├── domain
   |  |           |  ├── configuration
   |  |           |  ├── model
   |  |           |  |  ├── exception
   |  |           |  |  ├── payment
   |  |           |  |  |  ├── enums
   |  |           |  |  |  ├── listener
   |  |           |  |  |  └── store
   |  |           |  |  └── status
   |  |           |  └── service
   |  |           |     ├── listener
   |  |           |     ├── message
   |  |           |     └── payment
   |  |           ├── infrastruture
   |  |           |  ├── entity
   |  |           |  └── repository
   |  |           └── utils
   |  └── resources
   └── test
      ├── kotlin
      |  └── com
      |     └── mvp
      |        └── payment
      |           ├── application
      |           |  ├── bdd
      |           |  ├── integration
      |           |  ├── message
      |           |  ├── model
      |           |  ├── repository
      |           |  └── unit
      |           └── mongodb
      └── resources
         └── features

```

- status-ws folders

```
└── src
   ├── main
   |  ├── kotlin
   |  |  └── com
   |  |     └── mvp
   |  |        └── status
   |  |           ├── application
   |  |           |  └── v1
   |  |           ├── domain
   |  |           |  ├── configuration
   |  |           |  ├── model
   |  |           |  |  ├── exception
   |  |           |  |  ├── payment
   |  |           |  |  |  ├── enums
   |  |           |  |  |  └── listener
   |  |           |  |  └── status
   |  |           |  └── service
   |  |           |     ├── listener
   |  |           |     ├── message
   |  |           |     └── status
   |  |           ├── infrastruture
   |  |           |  ├── entity
   |  |           |  └── repository
   |  |           └── utils
   |  └── resources
   └── test
      ├── kotlin
      |  └── com
      |     └── mvp
      |        └── status
      |           ├── application
      |           |  ├── bdd
      |           |  ├── integration
      |           |  ├── message
      |           |  ├── model
      |           |  └── unit
      |           └── mongodb
      └── resources
         └── features

```
---

## 2.1. Terraform AWS EKS Cluster Deployment

Repositorys:

- https://github.com/lfneves/mvp (old)
- https://github.com/lfneves/infra-rds-terraform
- https://github.com/lfneves/infra-eks-terraform
- https://github.com/lfneves/infra-vpc-terraform

### 2.1.1. AWS Infra Terraform EKS

This project uses Terraform to automate the deployment of an Amazon Elastic Kubernetes Service (EKS) cluster on AWS. Amazon EKS is a managed Kubernetes service that simplifies the deployment, scaling, and operation of containerized applications using Kubernetes.

```
infra-eks-terraform
├── LICENSE
├── README.md
└── infra-eks-terraform
   ├── eks-cluster.tf
   ├── outputs.tf
   ├── providers.tf
   ├── variables.tf
   ├── vpc.tf
   └── workstation-external-ip.tf

```

### 2.1.2. Prerequisites

Before getting started, make sure you have the following prerequisites installed on your machine:

- [Terraform](https://www.terraform.io/) (you can use `terraform --version` to check)
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) for interacting with the cluster
- [kubectl-aws-iam-authenticator](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) for authenticating with the EKS cluster
- Internet access

### 2.1.3. Repository

1. Clone this repository:

   ```bash
   git clone https://github.com/lfneves/infra-eks-terraform.git

   cd infra-eks-terraform
   ```

2. Automatically create a `delivery-eks-terraform.tfstate` file and deploy bucket on `delivery-terraform-s3` and provide the necessary variables:

   ```hcl
   region            = "us-east-1"
   cluster_name      = "delivery-cluster"
   node_instance_type = "t2.small"
   node_max_count     = 1
   node_min_count     = 1
   ```

---

## 2.2. Terraform AWS RDS Database Deployment

### 2.2.1. AWS Infra Terraform RDS

https://github.com/lfneves/infra-rds-terraform

Terraform AWS RDS PostgreSQL Deployment
This project uses Terraform to automate the deployment of a single Amazon RDS instance with PostgreSQL on AWS. Amazon RDS (Relational Database Service) is a managed relational database service that makes it easy to deploy, operate, and scale databases.

```
infra-rds-terraform
├── LICENSE
├── README.md
└── infra-rds-terraform
   ├── main.tf
   ├── outputs.tf
   ├── table_schema.sql
   ├── variables.tf
   └── versions.tf

```


## 2.3. Justification for Choosing PostgreSQL for a Restaurant System

- **Robust Performance**: PostgreSQL is known for delivering solid performance, even in high transaction volume environments. This is crucial for a restaurant system where it's essential to process orders quickly and efficiently, ensuring a seamless customer experience.

- **Reliability and Stability**: PostgreSQL is renowned for its stability and reliability. Restaurant systems need a database that can handle continuous and critical workloads, minimizing the risk of unexpected failures that could disrupt business operations.

- **Flexible Data Model**: PostgreSQL supports a variety of data types and offers support for advanced features such as foreign keys, indexes, and stored procedures. This allows for flexible data modeling, covering everything from menus and orders to employee and customer information.

- **Advanced Query Capabilities**: PostgreSQL has a powerful SQL query engine that allows for efficient execution of complex queries. This is crucial for generating reports and data analysis that can help restaurant owners and managers make informed decisions.

- **Active Community and Support**: PostgreSQL has an active community of developers and users worldwide. This means you'll have access to ongoing technical support, updates, and security fixes. Additionally, many resources and plugins are available to customize the system to meet the restaurant's specific needs.

- **Cost-Effective**: PostgreSQL is an open-source database, which means it's a cost-effective option compared to many commercial database management systems. This can be especially advantageous for restaurants with budget constraints.

- **Integration and Scalability**: PostgreSQL is highly compatible with many programming languages and can be easily integrated with other parts of the restaurant system, such as online ordering apps, inventory management systems, and more. Furthermore, it is scalable, allowing the database to grow as the restaurant expands its operations.

In summary, PostgreSQL offers a solid combination of performance, reliability, flexibility, and cost-effectiveness, making it a sensible choice for a restaurant system that requires a robust and dependable database to meet critical business needs.


## 2.4. Database Diagram

![Alt text](images/database-diagram.png "Database Diagram")

---

### 2.4.1. Application mvp

https://github.com/lfneves/mvp

#### 2.4.1.1. This is a [Spring Boot WebFlux](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html) application using [Kotlin](https://kotlinlang.org/).

Spring WebFlux utilizes the [Reactor](https://projectreactor.io/) library, which is an implementation of Reactive Streams specs for building non-blocking applications.

This project:
- Uses [Reactor Netty as the default implementation](https://github.com/reactor/reactor-netty) for testing purposes. To change to Apache Tomcat as the default Web container for Spring WebFlux, follow these steps.
- Utilizes functional endpoints.
- Employs the [PostgreSQL](https://www.postgresql.org/) database.


## 2.5. 💡 Requirements

- Java 17 or later - [SDKMAN - Recommendation](https://sdkman.io/install)
- Gradle 7.6.1 or later - [Gradle build tool Installation](https://gradle.org/install/)
- Docker 24.0.2 or later - [How to install Docker](https://docs.docker.com/engine/install/)
- Docker Compose 1.29.2 or later - [Reference guide](https://docs.docker.com/compose/install/)
- Minikube v1.31.2 or later - [Get Started with Minikube](https://minikube.sigs.k8s.io/docs/start/)
- Helm v3.10.1 or later - [Installing Helm](https://helm.sh/docs/intro/install/)
- The project runs on port 8099 (http://localhost:8099).

<!-- GETTING STARTED -->
## 2.6. Getting Started

```sh
# Get the latest version

git clone https://github.com/lfneves/mvp.git
```

## 2.7. Project Structure


```
main
├── kotlin
|  └── com
|     └── mvp
|        └── delivery
|           ├── DeliveryApplication.kt
|           ├── application
|           ├── domain
|           ├── infrastruture
|           └── utils
└── resources
   ├── application.yml
   └── database
      ├── 1_create_tables.sql
      └── 2_inserts_category.sql
```

### 2.7.1. Prerequisites
Check versions:
* Java 17+
  ```sh
  java --version
  ```

* Docker
  ```sh
  docker -v
  ```

* Docker Compose
  ```sh
  docker-compose --version
  ```

## 2.8. Installation
This is an example of how to use the software and how to install it.


### 2.8.1. Docker

In the main project directory:

  
  Docker build and start applications:
  ```sh 
  $ docker-compose up --build
  ```
  
   Or use:
   
  ```
  $ docker-compose up -d --build
  ```
  

  To recreate the application in case of problems, use the command:
    
  ```
  $ docker-compose down
  ```
  
---


### 2.8.2. Kubernetes (k8s)

#### 2.8.2.1. To initiate Kubernetes applications, execute the commands found within the "k8s" folder.

```
$ kubectl apply -f delivery/k8s/postgres/.
```

```
$ kubectl apply -f delivery/k8s/application/.
```


#### 2.8.2.2. o access the application URL, use the following command:

```
$ minikube service delivery --url
```

#### 2.8.2.3. Example output:
```
http://192.168.49.2:32000

```

Inside the "k8s" folder, you will discover ".yaml" files utilized to deploy databases and applications within Kubernetes.

```
/delivery/k8s
├── application
|  ├── 1-deployment.yaml
|  ├── 2-service-load-balancer.yaml
|  ├── 3-hpa.yaml
|  ├── 4-ingress.yaml
|
└── postgres
   ├── 1-db-persistent-volume.yaml
   ├── 2-db-volume-claim.yaml
   ├── 3-db-configmap.yaml
   ├── 4-db-secret.yaml
   ├── 5-db-deployment.yaml
   └── 6-db-service.yaml
```

#### 2.8.2.4. Metric Server 

```
$ minikube addons enable metrics-server
```

To monitor the Horizontal Pod Autoscaler, employ the following command:
```
$ kubectl get hpa
```
![Alt text](images/hpa_example_img.png "Horizontal Pod Autoscaler")

---

###  2.8.3. Kubernetes (k8s) - Install with Helm 

[BETA] Because this hasn't been implemented following best practices.

```
$ helm install deliveryhelm deliveryhelm/
```

#### 2.8.3.1. Helm uninstall

```
$ helm uninstall deliveryhelm deliveryhelm/
```

---

## 2.9. Deploy Github-actions


![Alt text](images/mvp_ci_cd.png "Deploy Github-actions")


---
###  2.9.1. Integration Mercado Pago 

For the webhook checkout process, generate a QR code.

For testing full process with Mercado Pago webhook, use hookdeck.com with CLI to change the order status in the localhost application.

Apllication path **/api/v1/mp-order/qr-code-checkout** creates a checkout with Mercado Pago.


Example:

```json
{
    "in_store_order_id": "75ca8fe9-3b1a-4053-8f3e-49a62e91f8e8",
    "qr_data": "00020101021243650016COM.MERCADOLIBRE02013063675ca8fe9-3b1a-4053-8f3e-49a62e91f8e85204000053039865802BR5908delivery6009SAO PAULO62070503***63042BFA"
}
```

---
### 2.9.2. This project uses [CommandLineRunner](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/CommandLineRunner.html)
- CommandLineRunner is used to create a default user, products and categories on start application startup.
- Default login :

**/api/auth/login-token**
```json
{
  "username": "99999999999",
  "password": "123"
}
```
---

## 2.10. The best way to use it as a suggestion is by using [Postman](https://www.postman.com/downloads/)
#### 2.10.0.1. A collection is available preconfigured in the project root
[MVP - Pos tech delivery application.postman_collection.json]()

- This project uses user and session control for access
- Endpoints without control access  _**"/api/auth/*"**_, **_"/api/v1/users/signup"_**

#### 2.10.0.2. Create new user example:
http://localhost:8099/api/v1/users/signup

Body:
```json
{
    "name": "Admin",
    "email": "admin@email.com",
    "cpf": "99999999999",
    "password": "admin",
    "address": {
        "street": "rua 1",
        "city": "sp",
        "state": "sp",
        "postalCode": "1234"
    }
}
```

<br/>

#### 2.10.0.3. Login - Use the username (cpf) and password, then copy the token and use it in authenticated endpoints.

http://localhost:8099/api/auth/login-token
```json
{
  "username": "99999999999",
  "password": "admin"
}
```
Response:
```json
{
"token": "eyJhbGciOiJIUzI1NiJ9.eyJpZENsaWVudCI6IjAiLCJ1c2VybmFtZSI6IjEyMzQ1Njc4OTEyIiwic3ViIjoiMTIzNDU2Nzg5MTIiLCJpYXQiOjE2ODgwOTI1NTAsImF1ZCI6Im5vLWFwcGxpY2F0aW9uLW5hbWUiLCJleHAiOjE2ODgwOTQwMDB9.HagYPqukwOML3OYad8sRjlnE0Gsy-5tGUSC72S-xyfU"
}
```

### 2.10.1. 💡To make it easier use environment variables 
#### 2.10.1.1. Place the command in the test tab on /api/auth/login-token
```sh
pm.environment.set("token", pm.response.json().token);
```

#### 2.10.1.2. Example:

![Alt text](images/postman_01.png "Postman token environment")
<br>
2.11. ![Alt text](images/postman_02.png "Postman token using")
---

### 2.11.1. This project also uses OpenAPI Specification [(Swagger)](https://swagger.io/docs/specification/about/).

#### 2.11.1.1. To access swagger use the URL:
http://localhost:8099/swagger-ui.html
or
http://localhost:8099/webjars/swagger-ui/index.html

![Alt text](images/swagger_01.png "Swagger home")

![Alt text](images/swagger_02.png "Swagger Endpoints")

---

<!-- ROADMAP -->
## 2.12. Roadmap

- [x] Improve README.md
- [X] Update order add paid status and adjusting service 
- [x] Implementation Helm 
- ### Improvements
- [x] Refactor admin services and repository to new package
- [x] Fix create order exceptions
- [x] Mercado Pago Qr code checkout
- [x] Refactor scripts database

---
<!-- LICENSE -->
## 2.13. License

Distributed under the MIT License. See LICENSE.txt for more information.

