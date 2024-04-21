## Disclaimer ⚠

> Esse projeto está em construção.

<p align="center">
<img src="https://github.com/andrepixel/microservice-quarkus/blob/main/Quarkus-Architecture.png" border="10"/>
</p>

> Esse projeto visa um estudo profundo de Microservice usando Java, Quarkus, Design Patterns, Containers e CI/CD.

### Arquitetura

<p align="center">
<img src="https://github.com/andrepixel/microservice-quarkus/blob/main/Quarkus_Architecture.jpg" border="10"/>
</p>

### Problema

  Vamos supor que estivessemos trabalhando em uma empresa que trabalha com gerenciamento de tickets para diversos cinemas. Trabalhamos em uma equipe "X" onde fazemos parte de um pequeno fluxo, dentre vários na arquitetura da empresa. Chegou uma demanda, que temos que gerenciar **Tickets** para mensurarmos a qualidade de processamentos dos nossos processos e verificar se estamos tendo algum problema no processamento do mesmo. Nosso **Product Owner** nos avisou, que alguns **Tickets** estão chegando com informações faltantes, e precisamos validar e retirar eles do fluxo de **Tickets**. **Tickets** que estão tudo certo, devem ser enviados para um **Message Broker** para que eles possam ser validados para o setor de **Business** da empresa.

### Solução

  O nosso fluxo tem como premissa, puxar todos **Tickets** que estão em uma fila do **SQS (all-tickets)** através de um **Worker**. O **Worker** fará uma pré validação de cada um, e salvar eles em um banco de dados, que por ter um fluxo gigante de informações, e os **Tickets** não tem relacionamento entre eles, foi escolhido o **DynamoDB**, para atender essa demanda. Feito devidas validações iniciais dos **Tickets**, teremos 3 projetos na outra ponta da arquitetura, que são **Workers**, que irão buscar os **Tickets** no **DynamoDB**, fazer um tratamento especial para cada **Ticket** correspondente para cada cinema, e enviar para outras filas do **SQS (constellation-ticket, crimsonfleet-ticket e ryujin-ticket)**. Essa arquitetura contém 6 projetos; todos eles são microservice. 

#### Tecnologias usadas

  * Java
  * Quarkus
  * Docker
  * GitHub Actions
  * AWS

#### [Primeiro Projeto](https://github.com/andrepixel/microservice-quarkus-1)

 Esse projeto consiste em fazer toda a regra de negócio anterior, do qual o time não tem conhecimento. Aqui, simulamos um **Worker** que cria todo o **Ticket** e envia para uma fila do **SQS (all-tickets)**. 

 > Estamos simulando um fluxo onde temos o **Primeiro Projeto** como um microservice que não temos conhecimento, que é algo normal em uma empresa, não conhecer tudo da arquitetura, principalmente se estamos trabalhando em uma empresa grande com arquitetura de microservice. Esse **Primeiro Projeto**, é necessário a sua criação para fazer sentido na representação dos outros.

--------------------------------------------------------------------------------------------------------------------

#### Segundo Projeto

  > Work in progress...

  Esse projeto é um **Worker** que consiste em fazer uma validação inicial da estrutura dos **Tickets**, que está alocado no **SQS (all-tickets)** e fazer uma separação de **Ticket** de cada cinema, e enviar para uma **API**, em sua respectiva rota, para que seja salvo consequetemente em suas tabelas.

---

#### Terceiro Projeto 

  > Work in progress...

   Esse projeto é uma **API** que consiste em validar a estrutura enviada pelos **Workers** nas pontas, e enviar a estrutura do dado **Ticket** para os devidos tabelas no **DynamoDB**.

---

#### Quarto Projeto 

  > Work in progress...

  Esse projeto é um **Worker** que consiste em fazer uma chamada de requisição **GET** para a **API** para buscar **Tickets** do cinema **Constellation** fazer validações nesses **Tickets**, e se o **Ticket** estiver tudo certo, ele envia para uma fila do **SQS (constellation-ticket)** correspondente, caso tenha algo errado com o **Ticket** o **Worker** faz uma outra requisição para a **API**, agora do tipo **PUT** que o **Ticket** foi validado, e adiciona no campo de erro o porque ele não foi enviado para a fila do **SQS**.
  
---

#### Quinto Projeto

  > Work in progress...

  Esse projeto é um **Worker** que consiste em fazer uma chamada de requisição **GET** para a **API** para buscar **Tickets** do cinema **CrimsonFleet** fazer validações nesses **Tickets**, e se o **Ticket** estiver tudo certo, ele envia para uma fila do **SQS (crimsonfleet-ticket)** correspondente, caso tenha algo errado com o **Ticket** o **Worker** faz uma outra requisição para a **API**, agora do tipo **PUT** que o **Ticket** foi validado, e adiciona no campo de erro o porque ele não foi enviado para a fila do **SQS**.

---

#### Sexto Projeto

  > Work in progress...

 Esse projeto é um **Worker** que consiste em fazer uma chamada de requisição **GET** para a **API** para buscar **Tickets** do cinema **Ryujin** fazer validações nesses **Tickets**, e se o **Ticket** estiver tudo certo, ele envia para uma fila do **SQS (ryujin-ticket)** correspondente, caso tenha algo errado com o **Ticket** o **Worker** faz uma outra requisição para a **API**, agora do tipo **PUT** que o **Ticket** foi validado, e adiciona no campo de erro o porque ele não foi enviado para a fila do **SQS**.

---
