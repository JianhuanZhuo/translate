---
title: Chapter 7. JanusGraph Server
date: 2018/4/6
---


## Chapter 7. JanusGraph Server
JanusGraph uses the [Gremlin Server](http://tinkerpop.apache.org/docs/3.2.6/reference#gremlin-server) engine as the server component to process and answer client queries. When packaged in JanusGraph, Gremlin Server is called JanusGraph Server.

【译文】 JanusGraph 使用 [Gremlin Server](http://tinkerpop.apache.org/docs/3.2.6/reference#gremlin-server) 引擎作为服务器组件来处理和响应客户端的查询。 当打包进 JanusGraph 时，则一般称 Gremlin Server 为JanusGraph Server。

JanusGraph Server must be started manually in order to use it. JanusGraph Server provides a way to remotely execute Gremlin scripts against one or more JanusGraph instances hosted within it. This section will describe how to use the WebSocket configuration, as well as describe how to configure JanusGraph Server to handle HTTP endpoint interactions.

【译文】JanusGraph Server 必须手动启动才能使用。JanusGraph Server 提供了一种对服务器托管的 JanusGraph 实例远程执行 Gremlin 脚本的方法。本节将介绍如何配置 JanusGraph Server 来处理 WebSocket 和 HTTP 交互。

### 7.1. Getting Started
#### 7.1.1. Using the Pre-Packaged Distribution
The JanusGraph release comes pre-configured to run JanusGraph Server out of the box leveraging a sample Cassandra and Elasticsearch configuration to allow users to get started quickly with JanusGraph Server. This configuration defaults to client applications that can connect to JanusGraph Server via WebSocket with a custom subprotocol. There are a number of clients developed in different languages to help support the subprotocol. The most familiar client to use the WebSocket interface is the Gremlin Console. The quick-start bundle is not intended to be representative of a production installation, but does provide a way to perform development with JanusGraph Server, run tests and see how the components are wired together. To use this default configuration:
- Download a copy of the current `janusgraph-$VERSION.zip` file from the [Releases page](https://github.com/JanusGraph/janusgraph/releases)
- Unzip it and enter the `janusgraph-$VERSION` directory
- Run `bin/janusgraph.sh start`. This step will start Gremlin Server with Cassandra/ES forked into a separate process. Note for security reasons Elasticsearch and therefore `janusgraph.sh` must be run under a non-root account.

【译文】JanusGraph 发行版中为 JanusGraph Server 默认配置了 Cassandra 和 Elasticsearch，以便用户快速入门 JanusGraph Server。 客户端程序在此默认配置下可以通过带有自定义子协议的 WebSocket 连接 JanusGraph Server。许多用不同语言开发的客户端都支持子协议。 使用 WebSocket 接口的客户端最熟悉的就是 Gremlin 控制台了。快速入门安装包不代表生产时的安装配置，但它提供了如何使用 JanusGraph Server 进行开发、运行、测试和连接组件的方法。 按如下步骤使用此默认配置：
- 从[下载页](https://github.com/JanusGraph/janusgraph/releases)中下载一份 `janusgraph-$VERSION.zip` 文件
- 解压并切换目录到 `janusgraph-$VERSION`
- 运行命令 `bin/janusgraph.sh start`，这个步骤会启动 Gremlin Server 和 Cassandra/ES 两个进程。处于安全考虑，Elasticsearch 和 `janusgraph.sh` 脚本都必须使用非根账户启动。

``` bash
$ bin/janusgraph.sh start
Forking Cassandra...
Running `nodetool statusthrift`.. OK (returned exit status 0 and printed string "running").
Forking Elasticsearch...
Connecting to Elasticsearch (127.0.0.1:9300)... OK (connected to 127.0.0.1:9300).
Forking Gremlin-Server...
Connecting to Gremlin-Server (127.0.0.1:8182)... OK (connected to 127.0.0.1:8182).
Run gremlin.sh to connect.
```

##### 7.1.1.1. Connecting to Gremlin Server
After running janusgraph.sh, Gremlin Server will be ready to listen for WebSocket connections. The easiest way to test the connection is with Gremlin Console.

【译文】运行 janusgraph.sh 后，Gremlin Server 会监听 WebSocket 连接，测试 Gremlin Server 是否已运行，最简单的方式就是 Gremsole Console 了。

Start [Gremlin Console](http://tinkerpop.apache.org/docs/3.2.6/reference#gremlin-console) with `bin/gremlin.sh` and use the `:remote` and `:>` commands to issue Gremlin to Gremlin Server:

【译文】使用 `bin/gremlin.sh` 启动 [Gremlin Console](http://tinkerpop.apache.org/docs/3.2.6/reference#gremlin-console) 控制台，并使用 `:remote` 和 `:>` 连接 Gremlin Server：

``` bash
$  bin/gremlin.sh
         \,,,/
         (o o)
-----oOOo-(3)-oOOo-----
plugin activated: tinkerpop.server
plugin activated: tinkerpop.hadoop
plugin activated: tinkerpop.utilities
plugin activated: janusgraph.imports
plugin activated: tinkerpop.tinkergraph
gremlin> :remote connect tinkerpop.server conf/remote.yaml
==>Connected - localhost/127.0.0.1:8182
gremlin> :> graph.addVertex("name", "stephen")
==>v[256]
gremlin> :> g.V().values('name')
==>stephen
```

The `:remote` command tells the console to configure a remote connection to Gremlin Server using the `conf/remote.yaml` file to connect. That file points to a Gremlin Server instance running on `localhost`. The `:>` is the "submit" command which sends the Gremlin on that line to the currently active remote.

【译文】`:remote` 命令通知控制台使用 `conf/remote.yaml` 文件来连接 Gremlin Server，该文件指向一个运行在 `localhost` 的 Gremlin Server 实例。`:>` 符号会把命令发送至当前连接的远程服务器。

### 7.2. JanusGraph Server as a WebSocket Endpoint


### 7.3. JanusGraph Server as a HTTP Endpoint
The default configuration described in [Section 7.1, “Getting Started”](http://docs.janusgraph.org/latest/server.html#server-getting-started) is a WebSocket configuration. If you want to alter the default configuration in order to use JanusGraph Server as an HTTP endpoint for your JanusGraph database, follow these steps:

【译文】[Section 7.1, “Getting Started”](http://docs.janusgraph.org/latest/server.html#server-getting-started) 中描述的是默认配置，即 WebSocket 配置。如果想更改默认配置以便 JanusGraph Server 以 HTTP 方式访问，请按照以下步骤操作：

1. Test a local connection to a JanusGraph database first. This step applies whether using the Gremlin Console to test the connection, or whether connecting from a program. Make appropriate changes in a properties file in the `./conf` directory for your environment. For example, edit `./conf/janusgraph-hbase.properties` and make sure the `storage.backend`, `storage.hostname` and `storage.hbase.table` parameters are specified correctly. For more information on configuring JanusGraph for various storage backends, see [Part III, “Storage Backends”](http://docs.janusgraph.org/latest/storage-backends.html). Make sure the properties file contains the following line:

    【译文】首先使用本地连接测试 JanusGraph 数据库。无论使用 Gremlin Console 还是程序来测试，此步骤都适用。根据所处的环境对 `./conf` 目录中的属性文件进行适当的更改。 例如，编辑 `./conf/janusgraph-hbase.properties` 并确保正确指定了 `storage.backend`，`storage.hostname` 和 `storage.hbase.table` 参数。 有关为各种存储后端配置JanusGraph的更多信息，请参阅 [Part III, “Storage Backends”](http://docs.janusgraph.org/latest/storage-backends.html)。确保属性文件包含以下行：

    ``` conf
    gremlin.graph=org.janusgraph.core.JanusGraphFactory
    ```

2. Once a local configuration is tested and you have a working properties file, copy the properties file from the `./conf` directory to the `./conf/gremlin-server` directory.

    【译文】一旦本地配置测试通过且有一个可用的属性文件，将属性文件从 `./conf` 目录复制到 `./conf/gremlin-server/` 目录。

    ``` bash
    cp conf/janusgraph-hbase.properties conf/gremlin-server/http-janusgraph-hbase-server.properties
    ```

3. Copy ./conf/gremlin-server/gremlin-server.yaml to a new file called http-gremlin-server.yaml. Do this in case you need to refer to the original version of the file

    【译文】将 `./conf/gremlin-server/gremlin-server.yaml` 复制到一个名为` http-gremlin-server.yaml` 的新文件中。 如果您需要参考文件的原始版本，请执行此操作

    ``` bash
    cp conf/gremlin-server/gremlin-server.yaml conf/gremlin-server/http-gremlin-server.yaml
    ```

4. Edit the `http-gremlin-server.yaml` file and make the following updates:
    
    【译文】编辑 `http-gremlin-server.yaml` 文件，并做出如下更新：

    1. If you are planning to connect to JanusGraph Server from something other than localhost, update the ip address for host:

        如果你打算除了本地之外也能连接 JanusGraph Server，则需要指定 host 的 ip 地址：

        ```
        host: 10.10.10.100
        ```

    2. Update the channelizer setting to specify the HttpChannelizer:

        通过指定 `HttpChannelizer` 更新 `channelizer` 设置：

        ```
        channelizer: org.apache.tinkerpop.gremlin.server.channel.HttpChannelizer
        ```
    
    3. Update the graphs section to point to your new properties file so the JanusGraph Server can find and connect to your JanusGraph instance:

        【译文】更新属性文件的 graph 部分配置，以便 JanusGraph Server 可以找到并连接到你的 JanusGraph 实例：
        ```
        graphs: {graph: conf/gremlin-server/http-janusgraph-hbase-server.properties}
        ```

5. Start the JanusGraph Server, specifying the yaml file you just configured:

    【译文】指定刚刚配置的 yaml 文件来启动 JanusGraph Server：

    ``` bash
    bin/gremlin-server.sh ./conf/gremlin-server/http-gremlin-server.yaml
    ```

6. The JanusGraph Server should now be running in HTTP mode and available for testing. curl can be used to verify the server is working:

    【译文】现在 JanusGraph Server 应该已经在 HTTP 模式下运行，我们可以使用 curl 工具来测试一下:

    ```
    curl -XPOST -Hcontent-type:application/json -d '{"gremlin":"g.V().count()"}' http://[IP for JanusGraph server host]:8182
    ```



