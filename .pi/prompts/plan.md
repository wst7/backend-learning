---
description: 生成完整课程大纲（入门/进阶/深入三阶段）
argument-hint: "[可选：额外要求，如偏重 Go]"
---
用 backend-learning skill 为我生成一份完整的后端学习课程规划。${@:-}

我是前端开发，目标是掌握后端、系统设计、Redis、消息队列、PostgreSQL。输出三阶段（入门/进阶/深入）课程表：每阶段列出模块、每模块的课题清单、预计课时、前置依赖。生成后写入 <项目根>/.pi/backend-learning/PROGRESS.md 的计划部分，并给出我建议的第一节课。
