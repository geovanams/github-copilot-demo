# Github Copilot

[Acesse o Laboratório](https://github.com/geovanams/github-copilot-demo/blob/main/COPILOT_DEMOS.md)

Repositório usado como base de código para demonstrar os recursos do Github Copilot. (Forked from Azure Samples)

A solução é composta por dois microsserviços: a album API e o album viewer.

![architecture](./assets/architecture.png)

#### Album API (`album-api`)

A [`album-api`](./album-api) é uma API Web do .NET 6 que recupera uma lista de álbuns do Azure Storage usando a API Dapr State Store. Ao executar o aplicativo pela primeira vez, o banco de dados será propagado. Para chamadas subsequentes, a lista de álbuns será recuperada do armazenamento estadual de apoio.

#### Album Viewer (`album-viewer`)

A [`album-viewer`](./album-viewer) é uma aplicação node por meio da qual os álbuns recuperados pela API são exibidos. Para exibir o repositório de álbuns, o microsserviço do album viewer usa a API de invocação do Dapr Service para entrar em contato com o backend do album API.


