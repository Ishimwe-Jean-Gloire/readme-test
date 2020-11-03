<h1 align="center">
  <a href="https://reactnative.dev/">
    ecap-server-core-backendframework-netcore
  </a>
</h1>

<p align="center">
  <strong>Backbone of selise in-house .net core framework</strong><br>
  Build your own ECAP Microservices
</p>

<p align="center">
  <a href="https://github.com/facebook/react-native/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="React Native is released under the MIT license." />
  </a>
  <a href="https://circleci.com/gh/facebook/react-native">
    <img src="https://circleci.com/gh/facebook/react-native.svg?style=shield" alt="Current CircleCI build status." />
  </a>
  <a href="https://reactnative.dev/docs/contributing">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome!" />
  </a>
</p>



## Introduction

Some introduction of backend **ecap-server-core-backendframework-netcore** will be provided here.


## Ecosystem

| Nuget | Status | Description |
|---------|--------|-------------|
| [Selise.Ecap.Framework]          | [![vue-router-status]][vue-router-package] | Framework for CommandHandlers, EventHandlers, Repository etc |
| [Selise.Ecap.WepApi.Netcore]                | [![vuex-status]][vuex-package] | WebApi project dependency |
| [Selise.Ecap.Hosting]             | [![vue-cli-status]][vue-cli-package] | Background service dependency |
| [Selise.Ecap.RowLevelSecurity]          | [![vue-loader-status]][vue-loader-package] | Use to maintain RowLevelSecurity |
| [Selise.Ecap.Bus.Esb] | [![vue-server-renderer-status]][vue-server-renderer-package] | English framework support |

[Selise.Ecap.Framework]: https://github.com/vuejs/vue-router
[Selise.Ecap.WepApi.Netcore]: https://github.com/vuejs/vuex
[Selise.Ecap.Hosting]: https://github.com/vuejs/vue-cli
[Selise.Ecap.RowLevelSecurity]: https://github.com/vuejs/vue-loader
[Selise.Ecap.Bus.Esb]: https://github.com/vuejs/vue/tree/dev/packages/vue-server-renderer

[vue-router-status]: https://img.shields.io/npm/v/vue-router.svg
[vuex-status]: https://img.shields.io/npm/v/vuex.svg
[vue-cli-status]: https://img.shields.io/npm/v/@vue/cli.svg
[vue-loader-status]: https://img.shields.io/npm/v/vue-loader.svg
[vue-server-renderer-status]: https://img.shields.io/npm/v/vue-server-renderer.svg

[vue-router-package]: https://npmjs.com/package/vue-router
[vuex-package]: https://npmjs.com/package/vuex
[vue-cli-package]: https://npmjs.com/package/@vue/cli
[vue-loader-package]: https://npmjs.com/package/vue-loader
[vue-server-renderer-package]: https://npmjs.com/package/vue-server-renderer

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

To update TensorFlow to the latest version, add `--upgrade` flag to the above
commands.

*Nightly binaries are available for testing using the
[tf-nightly](https://pypi.python.org/pypi/tf-nightly) and
[tf-nightly-cpu](https://pypi.python.org/pypi/tf-nightly-cpu) packages on PyPi.*
#### *Try your first TensorFlow program*

```shell
$ python
```

```python
>>> import tensorflow as tf
>>> tf.add(1, 2).numpy()
3
>>> hello = tf.constant('Hello, TensorFlow!')
>>> hello.numpy()
b'Hello, TensorFlow!'
```

For more examples, see the
[TensorFlow tutorials](https://www.tensorflow.org/tutorials/).


## Usage


**Step 1**: Initialize your WebService project with boilerplace code inside the Main method of Program.cs

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

### note: CommonJS usage
In order to gain the TypeScript typings (for intellisense / autocomplete) while using CommonJS imports with `require()` use the following approach:

## Documentation 

swagger documentation url

## Changelog

[Learn about the latest improvements][changelog].
