## Disclaimer ⚠

> Esse projeto está em construção.

<p align="center">
<img src="https://github.com/andrepixel/microservice-quarkus/blob/main/Quarkus-microservice-logo.png" border="10"/>
</p>

> Esse projeto visa um estudo profundo de Microservice usando Quarkus com Java, usando Design Patterns, e melhoria continua.

### Arquitetura

<p align="center">
<img src="https://github.com/andrepixel/microservice-quarkus/blob/main/Quarkus_Architecture.png" border="10"/>
</p>

### Problema

  Vamos supor que tivessemos trabalhando em uma empresa que trabalha com tickets para cinemas. Trabalhamos em uma equipe "X" onde fazemos parte de um pequeno fluxo, dentre vários na arquitetura da empresa. Chegou uma demanda, que **Tickets** estão chegando com informações faltantes em um setor da empresa, e está sendo enviado informações demais referente ao **Ticket**, que não são necessários para um setor específico da empresa. Com isso, o nosso time, deve criar uma solução para fazer devidas validações para que o problema não ocorra novamente.

### Solução

  O nosso fluxo tem como premissa, puxar todos ticket que estão em uma fila do **SQS(all_tickets)**, fazer pequenas validações, e salvar eles em um banco de dados, que por ter um fluxo gigante de informações foi escolhido o **DynamoDB**. Feito devidas validações iniciais dos **Tickets**, teremos 3 projetos na outra ponta da arquitetura, que são **Workers**, que irão fazer um tratamento especial para cada **Ticket** correspondente para cada cinema, e enviar para outras filas do **SQS**. Essa arquitetura contém 6 projetos; todos eles são microservice. 
  
  > Estamos simulando um fluxo onde temos o **Primeiro Projeto** como um microservice que não temos conhecimento, que é algo normal em uma empresa, não conhecer tudo da arquitetura. Esse **Primeiro Projeto**, é necessário a sua criação para fazer sentido na representação dos outros.

#### Tecnologias usadas

  * Java
  * Quarkus
  * Docker
  * Kubernetes
  * GitHub Actions
  * AWS

#### [Primeiro Projeto](https://github.com/andrepixel/microservice_spring_project_1)

 Esse projeto consiste em fazer toda a regra de negócio, do qual o time não tem conhecimento. Aqui, simulamos um **Worker** que cria todo o **Ticket** e envia para uma fila do **SQS(all_tickets)**.

--------------------------------------------------------------------------------------------------------------------

#### Segundo Projeto

  > Work in progress...

  Esse projeto é um **Worker** que consiste em fazer uma validação inicial da estrutura do **Ticket**, que vai ser buscado de uma fila do **SQS(all_tickets)** e fazer uma separação de **Ticket** de cada cinema, e enviar para a **API**, em sua respectiva rota.

---

#### Terceiro Projeto 

  > Work in progress...

   Esse projeto é uma **API** que consiste em validar a estrutura enviada pelos **Workers** nas pontas, e enviar a estrutura do dado(Ticket) para os devidos **documents** no **DynamoDB**.

---

#### Quarto Projeto 

  > Work in progress...

  Esse projeto é um **Worker** que consiste em fazer a requisição para a **API** fazer validações, e caso esteja tudo certo, fazer uma outra requisição para a **API** que o **Ticket** foi validado e enviado para a fila do **SQS** correspondente.
  
---

#### Quinto Projeto

  > Work in progress...

  Esse projeto é um **Worker** que consiste em fazer a requisição para a **API** fazer validações, e caso esteja tudo certo, fazer uma outra requisição para a **API** que o **Ticket** foi validado e enviado para a fila do **SQS** correspondente.

---

#### Sexto Projeto

  > Work in progress...

  Esse projeto é um **Worker** que consiste em fazer a requisição para a **API** fazer validações, e caso esteja tudo certo, fazer uma outra requisição para a **API** que o **Ticket** foi validado e enviado para a fila do **SQS** correspondente.

---
