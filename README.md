<h1 align="center">
  <p>
    ecap-server-core-backendframework-netcore
  </p>
</h1>

<p align="center">
  <strong>Backbone of selise in-house .net core framework</strong><br>
  Build your own ECAP Microservices
</p>

<p align="center">
  <a href="https://github.com/ittahad/readme-test/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/license-SELISE-blue"/>
  </a>
  <a href="https://circleci.com/backend-framework">
    <img src="https://img.shields.io/badge/azure-passing-brightgreen"/>
  </a>
  <a href="https://circleci.com/backend-framework">
    <img src="https://img.shields.io/badge/aws-5%20passed%2C%201%20failed-red"/>
  </a>
  <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template">Swagger</a>
    ·
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Report Bug</a>
    ·
    <a href="https://github.com/ittahad/readme-test/issues/new">Request Feature</a>
</p>



## Table of Contents

* [Introduction](#introduction)
* [Ecosystem](#ecosystem)
* [Prerequisite](#prerequisite)
* [Usage](#usage)
* [Documentation](#documentation)
* [ChangeLog](#changelog)
* [Authors](#authors)
* [License](#license)

## Introduction

Some introduction of backend **ecap-server-core-backendframework-netcore** will be provided here.


## Ecosystem

| Nuget | Status | Description |
|---------|--------|-------------|
| [Selise.Ecap.Framework]          | [![Selise.Ecap.Framework-Status]][Selise.Ecap.Framework-Package] | Framework for CommandHandlers, EventHandlers, Repository etc |
| [Selise.Ecap.WepApi.Netcore]                | [![Selise.Ecap.WepApi.Netcore-Status]][Selise.Ecap.WepApi.Netcore-Package] | WebApi project dependency |
| [Selise.Ecap.Hosting]             | [![Selise.Ecap.Hosting-Status]][Selise.Ecap.Hosting-Package] | Background service dependency |
| [Selise.Ecap.RowLevelSecurity]          | [![Selise.Ecap.RowLevelSecurity-Status]][Selise.Ecap.RowLevelSecurity-Package] | Use to maintain RowLevelSecurity |
| [Selise.Ecap.Bus.Esb] | [![Selise.Ecap.Bus.Esb-Status]][Selise.Ecap.Bus.Esb-Package] | English framework support |

[Selise.Ecap.Framework]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.WepApi.Netcore]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.Hosting]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.RowLevelSecurity]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.Bus.Esb]: https://img.shields.io/badge/nuget-3.0.0.1-blue

[Selise.Ecap.Framework-Status]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.WepApi.Netcore-Status]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.Hosting-Status]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.RowLevelSecurity-Status]: https://img.shields.io/badge/nuget-3.0.0.1-blue
[Selise.Ecap.Bus.Esb-Status]: https://img.shields.io/badge/nuget-3.0.0.1-blue

[Selise.Ecap.Framework-Package]: https://www.google.com
[Selise.Ecap.WepApi.Netcore-Package]: https://www.google.com
[Selise.Ecap.Hosting-Package]: https://www.google.com
[Selise.Ecap.RowLevelSecurity-Package]: https://www.google.com
[Selise.Ecap.Bus.Esb-Package]: https://www.google.com

## Prerequisite

To install the first nuget go to the WebService project of you solution and go to the Nuget Manager. Install the following Nuget

```
$ Install nuget Selise.Ecap.WebApi.NetCore
```

The go to the Background service project of you solution and go to the Nuget Manager. Install the following Nuget

```
$ Install nuget Selise.Ecap.Hosting
```

Finally, go to the Services project of you solution and go to the Nuget Manager. Install the following Nuget

```
$ Install nuget Selise.Ecap.Framework
```

After installing the dependencies above go to the ```Usage``` section


## Usage


**Step 1**: Initialize your WebService project with boilerplate code inside the Main method of Program.cs

```
var ecapWebApiPipelineBuilderOptions = new EcapWebApiPipelineBuilderOptions
            {
                UseFileLogging = true,
                UseConsoleLogging = true,
                CommandLineArguments = args,
                UseAuditLoggerMiddleware = true,
                AddApplicationServices = AddApplicationServices,
                UseJwtBearerAuthentication = true,
                AddRequiredQueues = AddRequiredQueues,
                AddRequiredExchanges = AddRequiredExchanges
            };

            ecapWebApiPipelineBuilderOptions.AddRoutes = (appSettings, routeBuilder) =>
            {
                routeBuilder.MapRoute("Default", $"{appSettings.ServiceName}" + "/{controller}/{action}/{id?}");
            };

            var webHostBuilder = EcapWebApiPipelineBuilder.BuildEcapWebApiPipeline(ecapWebApiPipelineBuilderOptions);

            await webHostBuilder.Build().RunAsync();
```

**Step 2**: Add these methods inside the WebService Program.cs file

```
private static IEnumerable<string> AddRequiredExchanges(IAppSettings appSettings)
        {
            return EventExchanges.GetExchangeNames();
        }

        private static IEnumerable<string> AddRequiredQueues(IAppSettings appSettings)
        {
            return new[] { ... };
        }

        private static void AddApplicationServices(IServiceCollection serviceCollection, IAppSettings appSettings)
        {
            serviceCollection.RegisterCollection(typeof(ICommandHandler<,>), new[] { typeof(...).Assembly });
        }
```

**Step 3**: Initialize the Background service in a similar fasion

```
 public static async Task Main(string[] args)
        {
            var rabbitMqHostBuilderOptions = new RabbitMqHostBuilderOptions
            {
                UseFileLogging = true,
                UseConsoleLogging = true,
                CommandLineArguments = args,
                AddApplicationServices = AddApplicationServices,
                AddAmpqMessageConsumerOptions = (appSettings) =>
                {
                    var ampqMessageConsumerOptions = new AmpqMessageConsumerOptions();

                    ampqMessageConsumerOptions.ConsumerSubscriptions.ListenOn(..., 5);

                    return ampqMessageConsumerOptions;
                }
            };

            var hostBuilder = RabbitMqHostBuilder.BuildRabbitMqHost(rabbitMqHostBuilderOptions);

            await hostBuilder.Build().RunAsync();
        }

        private static void AddApplicationServices(IServiceCollection serviceCollection, IAppSettings appSettings)
        {
            serviceCollection.RegisterCollection(typeof(ICommandHandler<,>), new[] { typeof(...).Assembly });
        }
```

## Documentation 

``` Swagger Documentation ``` <a href="https://www.google.com"> https://api-documentation.selise.biz/ </a>

## Changelog

Detailed changes for each release are documented in the [release notes](https://github.com/ittahad/readme-test/wiki).

## Authors

- [Abul Kasim](https://twitter.com/vuejs)
- [Ahsan Habib](https://medium.com/the-vue-point)
- [Rezwan Rafiq](https://medium.com/the-vue-point)

## License

[Secure Link Service Ltd](https://www.selise.ch)

Copyright (c) 2012-present, SELISE
