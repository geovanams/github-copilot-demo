# Github Copilot demo 

## Demo Scenarios

It's used as a code base to demonstrate Github Copilot capabilities.

The solution is composed of two microservices: the album API and the album viewer.

![architecture](./assets/architecture.png)

#### Album API (`album-api`)

The [`album-api`](./album-api) is an .NET 6 minimal Web API that retrieves a list of Albums from Azure Storage using the Dapr State Store API. Upon running the application for the first time the database will be seeded. For subsequent calls, the list of albums will be retrieved from the backing state store.

#### Album Viewer (`album-viewer`)

The [`album-viewer`](./album-viewer) is a node application through which the albums retrieved by the API are surfaced. In order to display the repository of albums, the album viewer microservice uses the Dapr Service invocation API to contact the backend album API.


