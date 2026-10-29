# 后端学习课程大纲（前端转后端）

三级结构：**阶段 → 模块 → 课节**。一节课（课节）= 30-45 分钟 = 一次 `/lesson` 的交付单元。每个课节是单一主题、可独立验收。

学习规则：
- 按阶段顺序推进；模块内按课节编号顺序推进；跨模块依赖在"前置"标出
- 每课节 1-2 道随堂自测，每模块结束 `/quiz`（3 题），连续两题答错的课节回炉重学
- `/lesson` 消费粒度 = 课节，不是模块
- 每个模块有"产出"（可运行的小项目，对标 Codecademy 每 2 单元一项目的密度），做完才进下一模块

图例：`[P]` 可选路径课节（按兴趣跳过），`[实验]` 需要动手搭环境。

---

## 阶段一：入门 —— 跑起来一个真正的后端

### M1 HTTP 与 Web 服务本质（6 课节）
- L1 HTTP/1.1 报文：method/header/status code，浏览器 fetch 背后真正发生了什么
- L2 TCP 连接：三次握手、keep-alive、队头阻塞（对照 WebSocket 握手理解）[实验]
- L3 从零用 Node `http` 模块写一个服务器，再对比 Fastify 封装了什么 [实验]
- L4 REST 约定：资源建模、状态码语义、幂等性
- L5 Cookie/Session vs JWT：服务端视角的认证状态管理
- L6 HTTP/2 与 HTTPS：TLS 握手、多路复用 [P]
- **产出**：一个无框架的 REST 服务，curl 验证所有状态码语义

### M2 Node.js 后端运行时（5 课节）
- L1 事件循环在服务端的表现：一次阻塞让 QPS 掉多少 [实验]
- L2 进程 vs 线程：cluster 多进程模型（类比 Web Worker，但讲清无共享内存的代价）
- L3 stream：处理大文件/大响应背压（类比前端 ReadableStream）[实验]
- L4 错误处理与优雅退出：uncaughtException、SIGTERM、in-flight 请求
- L5 Node 内存模型：V8 堆、GC、内存泄漏排查入门（heapdump）[P]
- **产出**：压测脚本对比阻塞/非阻塞版本的 QPS 差异报告

### M3 分层架构与工程规范（4 课节）
- L1 路由/controller/service/repository 四层为什么存在，前端 MCP/DAO 类比
- L2 依赖注入与配置管理：十二要素应用
- L3 API 版本化、错误响应规范、分页设计
- L4 接口文档：OpenAPI 自动生成
- **产出**：把 M1 的服务重构为四层结构 + 自动生成的 OpenAPI 文档

### M4 PostgreSQL 入门（6 课节）
- L1 关系型模型 vs 文档模型：PG 的 JSONB 给了前端开发者什么 [实验]
- L2 SELECT 深入：JOIN/GROUP BY/窗口函数
- L3 写操作与约束：外键、唯一约束、CHECK，数据库如何兜底业务规则
- L4 事务与 ACID：脏读/不可重复读/幻读，隔离级别实测 [实验]
- L5 Node + pg 驱动：写一个带事务的转账接口 [实验]
- L6 数据库设计实战：从需求到 ER 图到建表语句
- **产出**：用户系统的表结构 + 带事务的注册/登录 API

**阶段一验收**：独立完成"用户系统 API"（注册/登录/CRUD/分页/错误规范），docker compose 起 PG，压测工具打一遍。

---

## 阶段二：进阶 —— 做出能扛事的系统

### M5 PostgreSQL 进阶（5 课节）
- L1 B+ 树索引原理：为什么索引像"书的目录"，回表是什么 [P]
- L2 索引失效场景实测：函数包裹、前导通配符、隐式类型转换 [实验]
- L3 EXPLAIN 读法：Seq Scan → Index Scan 的决策逻辑 [实验]
- L4 锁与并发：行锁/间隙锁/死锁案例，乐观锁 vs 悲观锁
- L5 连接池（pgbouncer）与 N+1 查询治理
- **产出**：对用户系统慢查询逐条 EXPLAIN 优化，前后对比耗时

### M6 Redis 基础与应用（7 课节）
- L1 Redis 是什么：内存模型、为什么比 PG 快、安装与基本命令 [实验]
- L2 String/计数器：INCR 原子性，限流器雏形 [实验]
- L3 Hash/List/Set/ZSet：每个结构对应什么业务场景（排行榜用 ZSet、时间线用 ZSET/LPUSH）
- L4 过期与淘汰：lazy/active 过期、7 种淘汰策略、maxmemory [实验]
- L5 缓存三大故障：穿透/击穿/雪崩，各自解法（布隆过滤器、互斥锁、TTL 抖动）
- L6 四种缓存策略：cache-aside/write-through/write-behind/read-aside 的一致性取舍
- L7 Pipeline 与 RESP 协议：循环 get 的性能杀手，RTT 是什么 [实验]
- **产出**：给用户系统加热点数据缓存，注入故障演示穿透及防护效果

### M7 Redis 内部机制（5 课节）
- L1 单线程事件循环：为什么快、KEYS * 为什么是生产事故 [实验]
- L2 数据结构内部编码：listpack/skiplist/hashtable，编码升级触发条件 [P]
- L3 内存剖析：MEMORY USAGE、碎片率、大 key/热 key 排查 [实验]
- L4 持久化：RDB 的 fork/COW、AOF fsync 三档、选型决策矩阵 [实验]
- L5 客户端与连接池：ioredis 配置、超时与重连策略
- **产出**：一个内存剖析报告（找出你系统里最大的 key 并给出优化方案）

### M8 Redis 分布式能力（6 课节）
- L1 SET NX PX 分布式锁：锁值 UUID、GC 停顿问题、fencing token [实验]
- L2 Lua 脚本：原子复合操作，EVAL/EVALSHA，用 Lua 修限流器竞态 [实验]
- L3 Redlock 争议：Martin Kleppmann vs antirez 论战到底在争什么
- L4 主从复制：全量/部分同步、复制延迟、异步复制丢数据窗口 [P]
- L5 Sentinel 与 Cluster：16384 slot、MOVED/ASK、什么时候需要 Cluster [P]
- L6 实战：用 Redis 实现延迟队列（ZSET）或 BullMQ 任务队列 [实验]
- **产出**：带分布式锁的库存扣减服务，并发压测证明不超卖

### M9 消息队列（5 课节）
- L1 同步调用 vs 异步解耦：为什么订单服务不该直接调库存服务
- L2 RabbitMQ/Kafka 核心概念：exchange/queue、topic/partition、消费组
- L3 投递保障：at-least-once + 幂等消费、重复消费怎么防 [实验]
- L4 重试与死信：失败处理的完整链路
- L5 实战：订单-库存-通知三服务解耦 [实验]
- **产出**：三服务异步下单流水线，含重试和死信队列演示

### M10 工程化：中间件、可观测性与质量（7 课节）
- L1 Nginx 反向代理：负载均衡算法、七层转发（前端 devServer proxy 的完全体）[实验]
- L2 后端测试：单测/集成测试分层，Testcontainers 起真 PG/Redis 测集成 [实验]
- L3 CI/CD 与质量门禁：GitHub Actions 跑测试才能合并
- L4 日志：结构化日志、pino/winston、集中收集思路
- L5 指标与监控：Prometheus 四个黄金信号、RED/USE 方法
- L6 链路追踪：OpenTelemetry 基本概念，一个请求跨 3 个服务怎么追 [P]
- L7 进程守护与部署：Docker 多阶段构建、systemd/compose 生产姿势
- **产出**：M9 的服务带上测试 + CI，绿色流水线截图，/metrics 暴露指标

**阶段二验收**：给阶段一的用户系统加"热点内容缓存 + 秒杀接口（Redis 锁 + MQ 异步下单）"，演示故障注入（杀 Redis 看穿透表现）。

---

## 阶段三：深入 —— 系统设计与分布式

### M11 系统设计方法论（4 课节）
- L1 估算：QPS/存储/带宽 back-of-envelope 怎么算（对标 Grokking/ByteByteGo 开局框架）
- L2 从零到百万用户：单机 → 缓存 → 读写分离 → 拆分的演进路径
- L3 负载均衡与水平扩展：一致性哈希，为什么有状态服务难扩
- L4 缓存架构：本地缓存 + Redis 两级、失效广播
- **产出**：对你自己的用户系统写一份容量估算文档（现在和 10 倍流量）

### M12 分布式核心问题（5 课节）
- L1 CAP/PAUSE 定理的实际含义：你在真实系统里选的是什么
- L2 分布式 ID：雪花算法及其时钟回拨问题
- L3 分布式事务：2PC/TCC/本地消息表，为什么最终一致性是主流
- L4 共识与协调：Raft 直觉、etcd/ZooKeeper 在架构里的角色 [P]
- L5 消息队列进阶：Kafka 副本机制、exactly-once 的代价 [P]
- **产出**：画出你所有已完成产出的系统全景图，标出每处的一致性选择

### M13 经典设计题（每题一课节，共 6 课节）
按"需求 → 估算 → 高层设计 → 深入组件 → 权衡"模板做（评分四维：估算准确性/组件完整性/权衡论证/故障追问）：
- L1 短链服务（哈希 + 缓存）
- L2 Feed 流/时间线（推拉模式）
- L3 秒杀系统（限流 + 预扣 + 异步下单）
- L4 分布式限流器（滑动窗口 + Redis Lua）
- L5 排行榜（ZSet 完整方案）
- L6 聊天系统（在线状态、消息有序性、WebSocket 网关）
- **产出**：6 题全部书面画图 + 口述，录音回听自查

### M14 大规模真实架构（4 课节）
- L1 读写分离与分库分表：何时做、中间件（ShardingSphere）怎么挡在中间
- L2 微服务拆分的代价：分布式事务、运维复杂度，为什么不是银弹
- L3 案例拆解①：经典论文精读——MapReduce/Memcached at Facebook（MIT 6.824 读法：先读论文再拆架构）[P]
- L4 案例拆解②：工程博客精读（选 2-3 篇：Twitter snowflake、Uber 架构）[P]
- **产出**：选一家公司，书面复原其架构演进史（初期→现在）

**阶段三验收**：完整口述设计 2 道设计题（秒杀 + Feed 流），书面画出架构图并按四维 rubric 自评，回答追问（"这个组件挂了怎么办"）。

---

## Capstone（阶段三后，1-2 周）

**多服务内容平台**：Fastify 网关 + PG 主库 + Redis 缓存/锁 + MQ 解耦 + Nginx + 测试/CI + 可观测性三件套。要求：写出容量估算、故障演练脚本（杀缓存/MQ/DB 各一次）、事后复盘报告。

---

## 选修模块（按需插入任意阶段）
- OAuth2 与 RBAC（插 M3 后）
- gRPC 与 protobuf（插 M9 后）
- K8s 基础（插 M10 后）
- Go 入门（后端主流语言对照，插 M14 后）

## 总量
14 模块 ≈ 75 课节 + 14 个模块产出 + 3 次阶段验收 + 1 个 capstone。按每周 3-4 课节，约 5-6 个月。
