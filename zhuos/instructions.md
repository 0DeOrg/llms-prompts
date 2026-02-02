# 项目提示词：monitor-websocket 架构指令集

### **1. 项目定位与背景**
- **项目名称**：`monitor-websocket`
- **核心目标**：作为一个完全独立、轻量级的 WebSocket 服务，实现监控指标的实时推送（基于 `WEBSOCKET_PROTOCOL.md`）。
- **技术栈**：Spring Boot 2.7.13, Spring Cloud Finchley.SR4 (OpenFeign), Gson, Lombok, SLF4J/Logback。

### **2. 核心架构原则（设计意图）**
- **枚举驱动 (Enum-Driven)**：
    - 所有协议常量（`ChannelType`, `MessageType`, `MetricType`）必须通过枚举管理。
    - **意图**：确保强类型安全，避免硬编码拼写错误，维持前后端协议高度一致。
- **Bean 驱动与开闭原则 (Bean-Driven & OCP)**：
    - 业务频道必须实现 `MonitorChannel` 接口并注册为独立的 Spring Bean。
    - `ChannelManager` 通过依赖注入动态发现所有频道实现。
    - **意图**：新增业务监控项时，仅需添加新 Bean，无需修改核心转发逻辑。
- **代理模式 (Proxy Pattern)**：
    - 所有外部资源访问（Prometheus HTTP API, 跨模块 Feign 调用）必须通过 `ResourceProvider` 接口及其代理类 `DataResourceProxy`。
    - **意图**：集中管理外部依赖，实现日志审计、异常拦截、结果适配（Adapter）及降级保护。
- **会话存活保护 (Session Protection)**：
    - 基于 Ping-Pong 心跳机制，由 `ChannelManager` 维护活跃时间戳，定时任务自动清理僵尸连接。
    - **意图**：防止网络异常导致的资源泄露，保障服务端高可用。

### **3. 核心组件职责**
- **MonitorWebSocketHandler**：协议入口，负责报文解析、分发及活跃时间上报。
- **ChannelManager**：中枢大脑，管理会话生命周期、订阅关系映射及存活检测。
- **MonitorChannel**：业务契约，定义数据快照（Snapshot）获取与模拟数据生成的标准。
- **DataResourceProxy**：访问网关，统一封装对 Prometheus 和外部微服务的调用逻辑。
- **RedisLuaProxyBean**：Redis 执行代理，采用 `SCRIPT LOAD` + `EVALSHA` 机制，优化 Lua 脚本执行性能。
- **MqConsumer**：RabbitMQ 消息消费者，负责接入行情数据，通过手动 Ack 确保消息可靠性。

### **4. 开发约束与规范**
1. **代码注释**：所有新类和关键方法必须包含“设计意图”说明。
2. **隔离性**：禁止引入对主项目 `api` 或 `domain` 模块的强依赖，保持子项目的独立运行能力。
3. **容错性**：从外部获取数据时，必须提供降级逻辑（如返回内存中的模拟数据）。
4. **Redis 执行**：涉及复杂 Redis 逻辑时，优先使用 Lua 脚本并由 `RedisLuaProxyBean` 调用，确保操作的原子性与高效性。
5. **消息可靠性**：MQ 消费必须使用 `MANUAL` 确认模式。业务逻辑处理成功后执行 `basicAck`；处理失败必须执行 `basicNack` 并设置 `requeue=true` 以确保消息不丢失。
6. **扩展路径**：新增监控频道时，在 `ChannelType` 添加枚举 -> 实现 `MonitorChannel` 接口 -> 标注 `@Component`。
