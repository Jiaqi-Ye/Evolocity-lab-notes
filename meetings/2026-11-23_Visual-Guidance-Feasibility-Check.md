# FunPlus × Operad 会议摘要

2026年11月23日 · 主题：Visual Guidance、Playtest 与 Self-Improving Harness

**核心结论：** 当前优先开展 Feasibility Check，验证游戏引擎是否支持视觉建议中的动作，以及是否能获取用于程序化评测的数据。

## 一、Milestone 1.1 与 1.2 的区别

| 对比项 | Milestone 1.1 | Milestone 1.2 |
| --- | --- | --- |
| 对应内容 | Visual Principle | Visual Guidance |
| 目标 | 满足基础视觉要求 | 进一步优化视觉表现 |
| 判断标准 | 相对明确、客观 | 相对主观、难量化 |
| 通俗理解 | 达到 60 分 | 从 60 分提升到 90 分 |

当前个人任务主要对应 1.2 的 Visual Guidance，涉及动效、节奏感、游戏手感等。

## 二、无人工标注情况下的评测

- 1.1 和 1.2 暂无对应的新人工标注；旧实验仍部分依赖 v0.2.4 的人工结果，可能存在噪声。
- 此前部分实验仅对旧 build 重新评分，尚需针对 1.1 实际运行 loop。
- 研究问题：在缺少人工标注和反馈的情况下，Agent 能否自行探索并进化出有效的评测方法？

## 三、从视觉评测到代码级评测

初步思路：Visual Guidance / 视频参考 → 截图与视觉观察 → 提取规律 → 代码或引擎层验证 → 可复用的 Evaluation Harness。

例如，验证抛物线运动时，可以探索通过位置、速度或物理参数进行计算，而非每次都反复截图。截图可作为初期视觉反馈；长期目标是提高验证效率。

## 四、Meta-Harness 与 Operad

会议讨论了 Meta-Harness 论文的简单自进化机制：读取历史经验 → Coding Agent 提出修改 → 生成新 Harness → 在 Sandbox 执行 → 评估 → 保存 Version History → 继续迭代。

每轮记录 Proposed Code Change、Reasoning Trace 和 Evolution Score。Operad 本身可以被理解为 Harness of Harnesses：Cell 是可优化的 Harness，Controller 负责生成代码、执行和评估。

长期目标是在 Operad 上测试不同的自进化 Recipes，保留有效的方法。Game Playtest 是其中一个应用场景。现阶段优先采用简单的 loop，并控制 Sandbox 成本。

## 五、人员分工与协作

- **Jiaqi：** Lumber Truck（M12），检查引擎对视觉动作的支持和数据获取能力。
- **Qurui：** 漂流冲冲冲，检查另一款游戏引擎的对应能力。
- **共同任务：** 比较两个引擎的结果，寻找可借鉴、可复用的验证方法；可通过 Slack Huddle 协作。Operad 相关问题可向亦菲请教。

## 六、当前任务：Feasibility Check

1. **提取动作要求：** 阅读 Visual Guidance，从 PA Template 模板库选择一两个典型动作，例如抛物线、缩放或待机动画。
2. **验证 Lumber Truck 引擎：** 确认底层引擎是否能够实现这些动作，以及是否可以获取位置、速度、动画参数等用于验证的数据。
3. **根据结果决定下一步：** 若不支持，暂不继续纠结代码级实现，考虑其他验证方式；若支持，再探索如何将视觉要求转化为可计算的评测规则，并与 Qurui 比较结果。

**当前范围：** 不需要立即实现完整的 Self-Improving Loop，也不需要优化整个游戏或完成全部 Visual Guidance 评测体系。

*来源：会议转录（转录在末尾截断）。*
