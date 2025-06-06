# anp agent opensdk

anp agent opensdk 致力于为 Agent 开发者提供一个快速上手、易于集成的 SDK，帮助你自己的智能体快速集成基于 ANP 协议的互联能力，扩展开发者的服务范围。

## 项目目标

- 为 Agent 开发者提供一套开箱即用的 SDK，降低集成门槛。
- 通过自动演示脚本，帮助用户直观了解 SDK 的关键能力和工作流程。
- 提供详细的集成步骤说明，助力开发者将自有 Agent 快速接入 ANP 网络。
- 为智能体提供互联的开箱即用基本能力：
    1. 创建一个DID身份，身份与一个域名绑定，可以将DID文档和描述发布到公网域名，作为基本信任源
    2. 可以与其他 Agent 进行点对点身份认证与互信验证
    3. 可以与其他 Agent 进行点对点post消息/接收对方post消息、发布和调用api
    4. 在无法点对点联系的情况下（例如内网），可以与其他Agent在共同确认的sse公网服务建立“消息群”
    5. sse公网服务可以接收post，sse推送到目标Agent，从而完成Agent间的基本消息传递

## 自动演示脚本

项目内置自动演示脚本，旨在帮助用户快速了解 SDK 的核心功能和工作流程。启动脚本后，您可以自动体验 SDK 的完整流程，关键步骤会在控制台输出，便于理解各环节。

演示脚本当前涵盖以下功能：
1. 通过工具自动创建一个 DID 身份，并在用户目录下生成相应的配置。
2. 在 Agent 代码中加载 DID 身份。
3. 启动本地 ANPSDK 服务，该服务负责帮助 Agent 发布 DID-doc、API 以及消息接收端口。
4. 同一 ANPSDK 服务可以承载多个 Agent，并自动路由对应的消息/API 请求。
5. 启动 ANPSDK 服务的 Agent 可以通过 API (GET/POST)、消息 (POST) 和消息群 (POST+SSE) 进行互相通信。
6. 对于开发者，可以通过装饰器或注册方式在 ANPSDK 服务上发布对外 API，注册消息监听/消息群监听处理器，或随时向任何 DID 用户发送消息。

未来演示脚本将涵盖以下功能：
1. 向 ANP 目录服务发布 Agent 信息。
2. 发现其他 Agent 并拉取其描述信息。
3. 发起探索与互操作请求。
4. 在内网和移动场景下，通过 ANP 的 SSE 公网服务与其他智能体交互。
5. 演示简化版 ANPSDK，在托管 DID-doc 的情况下，无需启动 HTTP Server，只通过公网 SSE 对外监听。

> 启动演示脚本：
>
> ```bash
> python anp_sdk_demo.py -h

    usage: anp_sdk_demo.py [-h] [-p] [-f] [-n name host port host_dir agent_type]

    ANP SDK 演示程序

    options:
    -h, --help            show this help message and exit
    -s                    启用步骤模式，每个步骤都会暂停等待用户确认———适合作为学习与调试
    -f                    快速模式，跳过所有等待用户确认的步骤————适合作为回归测试
    -n name host port host_dir agent_type
                            创建新用户，需要提供：用户名 主机名 端口号 主机路径 用户类型
> ```
>
python anp_sdk_demo.py -n cool_anper localhost 9527 wba user

创建一个名为cool_anper的用户，主机名为localhost，端口号为9527，主机路径为wba，用户类型为user
其地址为did:wba:localhost%3A9527%3A:wba:user:8位随机数

python anp_sdk_demo.py -n cool_anp_agent localhost 9527 wba agent

创建一个名为cool_anp_agent的用户，主机名为localhost，端口号为9527，主机路径为wba，用户类型为agent
其地址为did:wba:localhost%3A9527%3A:wba:agent:unique_id（8位随机数）

did及其他信息存储在 /anp_open_sdk/anp_users/user_unique_id/目录下
agent类型会额外创建一个/anp_open_sdk/anp_users/user_unique_id/agent目录,用于配合开发者进行agent的各种配置

重复用户名会创建为用户名+日期+当日序号




## Agent 集成步骤说明

1. **基于 Agent 核心能力**：在您的智能体项目内引入SDK。
2. **配置 anp agent opensdk**：创建did身份，简单配置sdk。
3. **实现 ANP 服务注册**：参考示例代码，将您的代码注册到 ANP SDK的接口（如身份、发现、认证、探索、互操作等）。
4. **注册与发布 Agent**：通过 SDK 提供的注册接口，将你的 Agent 发布到 ANP 网络。
5. **测试与调试**：使用自动演示脚本或手动调用接口，验证集成效果。

详细集成文档请参考 [doc/architecture](doc/architecture/) 及示例代码。

## 运行过程体验

支持多进程或多主机下的 ANPSDK 互操作体验：

- 各 ANPSDK 可在不同进程或主机上独立运行，通过 ANP 协议交换彼此agent信息。
- 支持身份认证、互信验证，保障通信安全。
- 可发起探索、互操作请求，体验 Agent 开放互联协作。

> 你可以在多台主机分别运行 anp_sdk_demo.py，体验真实网络环境下的 Agent 发现与互操作。



## 快速开始

1. 克隆项目并安装依赖
2. 配置 .env 文件
3. 运行 anp_sdk_demo.py 体验自动演示
4. 按照集成步骤将 SDK 集成到你的 Agent 项目

## 参考文档

- [ANP 协议规范](https://github.com/agent-network-protocol/AgentNetworkProtocol)
- 示例代码与自动演示脚本

欢迎反馈建议，共同完善 anp agent opensdk！
本项目采用 Apache License 2.0 进行授权，详情请查看 LICENSE 文件。