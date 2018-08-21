
# Chapter 8. ConfiguredGraphFactory
The JanusGraph Server can be configured to use the `ConfiguredGraphFactory`. The `ConfiguredGraphFactory` is an access point to your graphs, similar to the `JanusGraphFactory`. These graph factories provide methods for dynamically managing the graphs hosted on the server.

【译文】JanusGraph 服务器可配置为 `ConfiguredGraphFactory`。 `ConfiguredGraphFactory` 是图的一个访问入口，类似于`JanusGraphFactory`。 这些图工厂提供了动态管理服务器上图实例的方法。

## 8.1. Overview
`JanusGraphFactory` is a class that provides an access point to your graphs by providing a `Configuration` object each time you access the graph.

【译文】`JanusGraphFactory` 是一个类，每次用户访问图时，它通过提供 `Configuration` 对象来为图构造访问入口。

ConfiguredGraphFactory provides an access point to your graphs for which you have previously created configurations using the ConfigurationManagementGraph. It also offers an access point to manage graph configurations.

【译文】`ConfiguredGraphFactory` 为您以前使用 `ConfigurationManagementGraph` 创建配置的图提供了一个访问入口。它还提供了另一个访问入口来管理图及其配置。

ConfigurationManagementGraph allows you to manage graph configurations.

【译文】`ConfigurationManagementGraph` 允许您管理图配置。

JanusGraphManager is an internal server component that tracks graph references, provided your graphs are configured to use it.

【译文】 `JanusGraphManager` 是一个内部服务器组件，用于跟踪图实例引用，只要您的图配置为使用它。

## 8.2. ConfiguredGraphFactory versus JanusGraphFactory
However, there is an important distinction between these two graph factories:

【译文】但是，两种图工厂有着明显的区别：

1. The `ConfiguredGraphFactory` can only be used if you have configured your server to use the `ConfigurationManagementGraph` APIs at server start.
    
    【译文】 `ConfiguredGraphFactory` 只有在服务器启动时配置为使用 `ConfigurationManagementGraph` API 才能使用

The benefits of using the `ConfiguredGraphFactory` are that:

【译文】使用 `ConfiguredGraphFactory` 有如下好处：

1. You only need to supply a String to access your graphs, as opposed to the `JanusGraphFactory`-- which requires you to specify information about the backend you wish to use when accessing a graph-- every time you open a graph.

    【译文】 你只需要一个字符串就可以访问你的图实例，而不是像 `JanusGraphFactory` 那样每次打开时都要求指定图的后端信息。

2. If your `ConfigurationManagementGraph` is configured with a distributed storage backend then your graph configurations are available to all JanusGraph nodes in your cluster.

    【译文】 如果分布式存储后台配置了 `ConfigurationManagementGraph`，那么这个配置对集群上的所有节点都有效。

## 8.3. How Does the ConfiguredGraphFactory Work?
The `ConfiguredGraphFactory` provides an access point to graphs under two scenarios:

【译文】 `ConfiguredGraphFactory` 对图实例提供了两种场景下的访问入口：

1. You have already created a configuration for your specific graph object using the `ConfigurationManagementGraph#createConfiguration`. In this scenario, your graph is opened using the previously created configuration for this graph.

    【译文】 你已经使用 `ConfigurationManagementGraph#createConfiguration` 方法为某图对象创建了一个配置。在此种场景下，你的图已经使用之前定义的配置打开了。

2. You have already created a template configuration using the `ConfigurationManagementGraph#createTemplateConfiguration`. In this scenario, we create a configuration for the graph you are creating by copying over all attributes stored in your template configuration and appending the relevant graphName attribute, and we then open the graph according to that specific configuration.

    【译文】

## 8.6. Dropping a Graph
`ConfiguredGraphFactory.drop("graphName")` will drop the graph database, deleting all data in storage and indexing backends. The graph can be open or closed (will be closed as part of the drop operation). Furthermore, this will also remove any existing graph configuration in the `ConfigurationManagementGraph`.

【译文】`ConfiguredGraphFactory.drop("graphName")` 会删除图数据库，包括删除存储和索引所有数据。另外，`ConfigurationManagementGraph` 中的配置信息也会被删除。