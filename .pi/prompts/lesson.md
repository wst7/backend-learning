---
description: 生成下一节课（概念+类比+代码+实验）
argument-hint: "[主题，默认按大纲下一节]"
---
用 backend-learning skill 给我上一节课：${1:-按 curriculum.md 大纲的下一节}

要求：先读取 skill 目录下 references/curriculum.md 和 <项目根>/.pi/backend-learning/PROGRESS.md 确认我当前进度，只上大纲中的下一个未完成**课节（L#）**，一个课节一节，不整模块灌输（$1 为空时按编号顺序；$1 形如 "M8-L2" 或 "Lua 脚本" 时上指定课节）。按 SKILL.md 的课程规范输出：目标 → 概念（必须有前端类比 + 类比失效点）→ 最小可运行 TypeScript/Node 代码 → 动手实验布置（标 [实验] 的课节必须实际跑通）→ 2-3 道自测题（等我回答，不要直接给答案）。上完后更新 <项目根>/.pi/backend-learning/PROGRESS.md（按课节编号记录）。
