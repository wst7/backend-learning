# learing — 前端转后端学习工程

一个**边学边做**的后端学习仓库：既有可运行的动手实验（`backend-labs/`），也有一套驱动教学的 AI Skill（`.pi/skills/backend-learning/`）。面向有经验的前端开发者系统学习后端与系统设计。

## 为什么这么组织

传统学习资料的问题是「看懂了但没搭过」。这个仓库把两者绑在一起：

- **每个理论概念对应一个能真正跑起来的最小实验**，用你熟悉的 Node.js / TypeScript 写，打破框架抽象、直接看协议和底层行为。
- **教学过程由 Skill 驱动**：概念先用前端类比引入 → 指出类比失效点 → 最小可运行代码 → 动手实验 → 苏格拉底式自测。严格按大纲循序渐进，一次只推进一个主题。

## 目录结构

```
.
├── AGENTS.md                      # 全局指令：后端导师模式的触发与规范
├── backend-labs/                  # 动手实验（每个主题一个子目录）
│   └── http-basics/               #   M1-L1：看见 HTTP 报文
│       ├── server.mjs             #   最小 HTTP 服务器，逐行打印请求报文
│       ├── raw-client.mjs         #   裸 TCP 手写 HTTP 请求（不经过 HTTP 库）
│       └── README.md              #   目的 / 步骤 / 预期输出 / 思考题
└── .pi/
    ├── skills/backend-learning/   # 教学 Skill
    │   ├── SKILL.md               #   教学规范与课程生成规则
    │   └── references/
    │       ├── curriculum.md      #   三阶段课程大纲（阶段 → 模块 → 课节）
    │       └── roadmap.md         #   学习路线图 + 三个选型点的轨道
    ├── prompts/                   # /lesson /quiz /lab /review 等指令模板
    └── backend-learning/
        └── PROGRESS.md            # 学习进度（选型、已完成课节、薄弱点）
```

## 快速开始：跑第一个实验

前置：Node.js（内置 `http` / `net` 模块，无需安装依赖）。

```bash
cd backend-labs/http-basics

# 终端 1：启动服务器，逐行打印收到的请求报文
node server.mjs

# 终端 2：用 curl 观察状态码语义
curl -i http://127.0.0.1:3000/            # 200
curl -i http://127.0.0.1:3000/not-found   # 404

# 终端 3：裸 TCP 客户端，手写一份 HTTP 请求，观察 chunked 编码
node raw-client.mjs
```

预期：服务器打印出 `POST /hello HTTP/1.1`、`host:`、`content-length: 5` 和 body `hello`；裸客户端打印完整响应报文，注意 `Transfer-Encoding: chunked` 与结尾的 `0`。

细节与思考题见 [`backend-labs/http-basics/README.md`](backend-labs/http-basics/README.md)。

## 课程大纲

三阶段，从 HTTP/Web 服务本质，到数据存储（PostgreSQL、Redis），再到消息队列与中间件。完整大纲见 [`curriculum.md`](.pi/skills/backend-learning/references/curriculum.md)，路线与选型轨道见 [`roadmap.md`](.pi/skills/backend-learning/references/roadmap.md)。

## 设计原则

1. **类比优先，随即破类比** —— 每个后端概念先映射到前端已知物（如 中间件 ≈ Express 中间件链、Redis ≈ 跨进程的 localStorage），再立刻指出类比失效处，防止误解。
2. **循序渐进，不跳号** —— 课节顺序即依赖顺序；前置未掌握不开新主题。
3. **先概念后动手** —— 每节课 = 目标 → 概念（含类比）→ 最小代码 → 实验 → 小结 + 自测题。
4. **苏格拉底式收尾** —— 先答题再纠正，不直接给答案。
5. **环境零阻力** —— 数据库/Redis 优先 Docker 一键起，避免配置消耗学习动力。

## License

见 [LICENSE](LICENSE)。
