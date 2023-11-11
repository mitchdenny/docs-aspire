---
title: .NET Aspire components overview
description: Explore the fundamental concepts of .NET Aspire components and learn how to integrate them into your apps.
ms.date: 11/11/2023
ms.topic: conceptual
ms.prod: dotnet
---

# .NET Aspire components overview

.NET Aspire components are a curated suite of NuGet packages specifically selected to facilitate the integration of cloud-native applications with prominent services and platforms, including but not limited to Redis and PostgreSQL. Each component furnishes essential cloud-native functionalities through either automatic provisioning or standardized configuration patterns. The .NET Aspire components are categorized into two main groups:

- **.NET Aspire client libraries:** This category comprises a selection of NuGet packages designed to enhance the process of connecting applications to widely-used services such as PostgreSQL databases, Redis caches, Azure Blob Storage, and Azure Key Vault, among others.

- **.NET Aspire platform libraries:** These NuGet packages are intended to streamline application integration within specific execution environments. For instance, the Kubernetes library offers features like telemetry enrichment and support for health probes.

## Available components

The following table lists the .NET Aspire components currently available for use:

| Component | NuGet | Description |
|--|--|--|
| [Azure Blob Storage](storage/azure-storage-blobs-component.md) | [Aspire.Azure.Storage.Blobs](https://www.nuget.org/packages/Aspire.Azure.Storage.Blobs) | Provides a client library for accessing Azure Blob Storage. |
| [Azure Cosmos DB Entity Framework Core](database/azure-cosmos-db-entity-framework-component.md) | [Aspire.Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Aspire.Microsoft.EntityFrameworkCore.Cosmos) | Provides a client library for accessing Azure Cosmos DB databases with Entity Framework Core. |
| [Azure Cosmos DB](database/azure-cosmos-db-component.md) | [Aspire.Microsoft.Azure.Cosmos](https://www.nuget.org/packages/Aspire.Microsoft.Azure.Cosmos) | Provides a client library for accessing Azure Cosmos DB databases. |
| [Azure Key Vault](security/azure-security-key-vault-component.md) | [Aspire.Azure.Security.KeyVault](https://www.nuget.org/packages/Aspire.Azure.Security.KeyVault) | Provides a client library for accessing Azure Key Vault. |
| [Azure Service Bus](messaging/azure-service-bus-component.md) | [Aspire.Azure.Messaging.ServiceBus](https://www.nuget.org/packages/Aspire.Azure.Messaging.ServiceBus) | Provides a client library for accessing Azure Service Bus. |
| [Azure Storage Queues](storage/azure-storage-queues-component.md) | [Aspire.Azure.Storage.Queues](https://www.nuget.org/packages/Aspire.Azure.Storage.Queues) | Provides a client library for accessing Azure Storage Queues. |
| [Azure Table Storage](storage//azure-storage-tables-component.md) | [Aspire.Azure.Data.Tables](https://www.nuget.org/packages/Aspire.Azure.Data.Tables) | Provides a client library for accessing the Azure Table service. |
| [PostgreSQL Entity Framework Core](database/postgresql-entity-framework-component.md) | [Aspire.Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Aspire.Npgsql.EntityFrameworkCore.PostgreSQL) | Provides a client library for accessing PostgreSQL databases using Entity Framework Core. |
| [PostgreSQL](database/postgresql-component.md) | [Aspire.Npgsql](https://www.nuget.org/packages/Aspire.Npgsql) | Provides a client library for accessing PostgreSQL databases. |
| [RabbitMQ](messaging/rabbitmq-client-component.md) | [Aspire.RabbitMQ.Client](https://www.nuget.org/packages/Aspire.RabbitMQ.Client) | Provides a client library for accessing RabbitMQ. |
| [Redis Distributed Caching](caching/stackexchange-redis-distributed-caching-component.md) | [Aspire.StackExchange.Redis.DistributedCaching](https://www.nuget.org/packages/Aspire.StackExchange.Redis.DistributedCaching) | Provides a client library for accessing Redis caches for distributed caching. |
| [Redis Output Caching](caching/stackexchange-redis-output-caching-component.md) | [Aspire.StackExchange.Redis.OutputCaching](https://www.nuget.org/packages/Aspire.StackExchange.Redis.OutputCaching) | Provides a client library for accessing Redis caches for output caching. |
| [Redis](caching/stackexchange-redis-component.md) | [Aspire.StackExchange.Redis](https://www.nuget.org/packages/Aspire.StackExchange.Redis) | Provides a client library for accessing Redis caches. |
| [SQL Server Entity Framework Core](database/sql-server-entity-framework-component.md) | [Aspire.Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Aspire.Microsoft.EntityFrameworkCore.SqlServer) | Provides a client library for accessing SQL Server databases using Entity Framework Core. |
| [SQL Server](database/sql-server-component.md) | [Aspire.Microsoft.Data.SqlClient](https://www.nuget.org/packages/Aspire.Microsoft.Data.SqlClient) | Provides a client library for accessing SQL Server databases. |

## Explore a sample component workflow

.NET Aspire components streamline the process of consuming popular services and platforms. For example, you could use the .NET Aspire PostgreSQL component to connect to and utilize a PostgreSQL database. The database could be hosted on-prem or in a cloud service such as Azure, AWS, or GCP. The following steps demonstrate how to integrate this component into your app:

1. Install the [Aspire.Npgsql](https://www.nuget.org/packages/Aspire.Npgsql) NuGet package.

    # [.NET CLI](#tab/dotnet-cli)

    ```dotnetcli
    dotnet add package Aspire.Npgsql --prerelease
    ```

    # [PackageReference](#tab/package-reference)

    ```xml
    <PackageReference Include="Aspire.Npgsql"
                      Version="[SelectVersion]" />
    ```

    ---

    For more information, see [dotnet add package](../core/tools/dotnet-add-package.md) or [Manage package dependencies in .NET applications](../core/tools/dependencies.md).

1. In the _Program.cs_ file of your project, call the `AddNpgsqlDataSource` extension method to register a `NpgsqlDataSource` for use via the dependency injection container. The method expects a connection name parameter.

    ```csharp
    builder.AddNpgsqlDataSource("PostgreSqlConnection");
    ```

    > [!NOTE]
    > Components that are designed to connect to Azure services also support passwordless authentication and authorization using Azure RBAC, which is the recommended approach for production apps.

1. Add a connection string with a matching name in your _appsettings.json_ file.

    ```json
    {
      "ConnectionStrings": {
        "PostgreSqlConnection": "Host=myserver;Database=test"
      }
    }
    ```

1. Inject the `NpgsqlDataSource` object into your controllers or service endpoints to run commands against the database:

    ```csharp
    public class ExampleService(NpgsqlDataSource npgContext)
    {
    }
    ```

After four steps, you now have a fully configured PostgreSQL database component integrated into your app. This component also configured health checks, logging, tracing, metrics, retries, and other useful capabilities for you behind the scenes. .NET Aspire components provide various options to configure each of these features.

## Configure .NET Aspire components

.NET Aspire components implement a consistent configuration experiences via <xref:Microsoft.Extensions.Configuration.IConfiguration> and <xref:Microsoft.Extensions.Options.IOptions%601>. Configuration is schematized and part of a component's contract, ensuring backward compatibility across versions of the component. You can setup every .NET Aspire component through either JSON configuration files or directly through code using delegates. JSON files must follow a standardized naming convention based on the Component name.

For example, add the following code to the _appsettings.json_ file to configure the PostgreSQL component:

```json
{
  "Aspire": {
    "Npgsql": {
      "HealthChecks": false,
      "Tracing": false
    }
  }
}
```

Alternatively, you can configure the component directly in your code using a delegate:

```csharp
builder.AddNpgsqlDataSource(
    "PostgreSqlConnection",
    static settings => settings.HealthChecks = false);
```

## Dependency injection

.NET Aspire components automatically register essential services with the .NET dependency container using the proper scope. This allows key component classes and services to be injected throughout your code. For example, the .NET Aspire PostgreSQL component makes available the `NpgsqlDataSource` to inject into your application layers and run commands against a database:

```csharp
public class ExampleService(NpgsqlDataSource npgContext)
{
}
```

For more information, see [.NET dependency injection](../core/extensions/dependency-injection.md).

### Keyed services

.NET Aspire components also support keyed dependency injection. In this scenario, the service name for keyed dependency injection will be the same as the connection name:

```csharp
builder.AddKeyedNpgsqlDataSource(
    "PostgreSqlConnection",
    static settings => settings.HealthChecks = false);
```

You can then retrieve the registered service using the `FromKeyedServices` attribute:

```csharp
public class ExampleService(
    [FromKeyedServices("PostgreSqlConnection")] NpgsqlDataSource npgContext)
{
}
```

For more information, see <xref:Microsoft.Extensions.DependencyInjection.FromKeyedServicesAttribute>.

## Cloud-native features

Cloud-native applications surface many unique requirements and concerns. The core features of .NET Aspire orchestration and components are designed to handle many cloud-native concerns for you with minimal configurations. Some of the key features include:

- [Orchestration](app-host-overview.md): A lightweight, extensible, and cross-platform app host for .NET Aspire apps. The app host provides a consistent configuration and dependency injection experience for .NET Aspire components.
- [Service discovery](service-discovery/overview.md): A technique for locating services within a distributed application. Service discovery is a key component of microservice architectures.
- [Service defaults](service-defaults.md): A set of default configurations intended for sharing amongst resources within .NET Aspire apps. These defaults are designed to work well in most scenarios and can be customized as needed.

Some .NET Aspire components also include more capabilities for specific services or platforms, which can be found in the component specific reference docs.

### Observability and telemetry

.NET Aspire components automatically set up Logging, Tracing, and Metrics configurations, which are sometimes known as _the pillars of observability_.

- **[Logging](/dotnet/core/diagnostics/logging-tracing)**: A technique where code is instrumented to produce logs of interesting events that occurred while the program was running. A baseline set of log events are enabled for .NET Aspire components by default and more extensive logging can be enabled on-demand to diagnose particular problems.

- **[Tracing](/dotnet/core/diagnostics/distributed-tracing)**: A specialized form of logging that helps you localize failures and performance issues within applications distributed across multiple machines or processes. This technique tracks requests through an application to correlate work done by different application components and separate it from other work the application may be doing for concurrent requests.

- **[Metrics](/dotnet/core/diagnostics/metrics)**: Numerical measurements recorded over time to monitor application performance and health. Metrics are often used to generate alerts when potential problems are detected. Metrics have low performance overhead and many services configure them as always-on telemetry.

Together, these types of telemetry allow you to gain insights into your application's behavior and performance using various monitoring and analysis tools. Depending on the backing service, some components may only support some of these features. For example, some components support logging and tracing, but not metrics. Telemetry features can also be disabled. For more information, see [.NET Aspire service defaults](service-defaults.md).

### Health checks

.NET Aspire components enable health checks for services by default. Health checks are HTTP endpoints exposed by an app to provide basic availability and state information. These endpoints can be configured to report information used for various scenarios:

- Influence decisions made by container orchestrators, load balancers, API gateways, and other management services. For instance, if the health check for a containerized app fails, it might be skipped by a load balancer routing traffic.
- Verify that underlying dependencies are available, such as a database or cache, and return an appropriate status message.
- Trigger alerts or notifications when an app isn't responding as expected.

For example, the .NET Aspire PostgreSQL component automatically adds a health check at the `/health` URL path to verify the following:

- A database connection could be established
- A database query could be executed successfully

If either of these operations fail, the health check also fails. For more information, see [Health checks in .NET Aspire](health-checks.md).

### Resiliency

.NET Aspire components enable resiliency configurations automatically where appropriate. Resiliency is the ability of your system to react to failure and still remain functional. Resiliency extends beyond preventing failures to include recovering and reconstructing your cloud-native environment back to a healthy state. Examples of resiliency configurations include:

- **Connection retries**: You can configure some .NET Aspire components to retry requests that initially fail. For example, failed database queries can be retried multiple times if the first request fails. This creates tolerance in environments where service dependencies may be briefly unresponsive or unavailable when the system state changes.

- **Timeouts**: You can configure how long an .NET Aspire component waits for a request to finish before it times out. Timeout configurations can be useful for handling dependencies with variable response times.

For more information, see [Build resilient HTTP apps](../core/resilience/http-resilience.md).