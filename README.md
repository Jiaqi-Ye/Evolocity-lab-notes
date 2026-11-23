## Operad 理解
### Operad 是什么？

Operad 是 Evolocity 自研的 Agent 底座，让 Agent 能持续学习。执行任务时，AI 往一个常驻的 Python 环境里写代码来完成工作，所以效率高。同时，它在沙箱里自动评测 Agent，并据此改进 Harness，让 Agent 越用越好，不需要重新训练底层模型。

### 它如何节省 Token？

主要有两个方面：

1. **代码驱动：** AI 写 Python 程序批量完成操作。比如检查 100 个文件，只返回"发现 3 个错误"，不让模型去读全部文件。这样输入 Token 和模型调用次数都会减少。
2. **持久化 Python 环境：** 已加载的数据和算好的结果会留在环境里，下次直接用，不用重复读取和计算。主要省的是时间，也可能少传一些重复内容，从而省 Token。

**实测：** 在 ARC-AGI-3 上，Operad 和 Codex 用同一个模型，25 个游戏都全部通关，Operad 的输入 Token 少 67%。

### 它和其他 Agent 有什么不同？

Codex 和 Claude Code 也能写代码、跑 Python，但它们主要还是靠读文件、搜索、敲命令这类工具。Operad 没有这些工具，所有工作都在 Python 环境里完成，只把重要结果交给模型。另外，它会自动评测并改进 Harness，所以成本更低、效率更高。

PPT 里的例子：Operad 配便宜的 DeepSeek Flash 模型，每个任务只花 5–7 美分，在 SkillsBench 上的得分高于 Codex 和 Claude Code。

### 为什么能应用于不同公司？

Operad 是一个通用底座。不同公司只要按自己的任务、工具和评测标准定制 Harness，不用从零开发整个系统，而且可以部署在公司自己的云上。

- **Operad（通用底座）：** 提供 Python 执行环境，也提供评测和进化的基础设施：沙箱，以及能同时调度大量 Agent 的控制器。
- **Harness（定制部分）：** 按业务任务配置 Prompt、Skills、Tools、Memory 和 Agent Loop。每次改动都会先评测、记录版本，变好就上线，变差就回滚。
- **Verifier（评测标准）：** 独立于被测的 Agent，判断它的表现是否达标，并为改进 Harness 提供反馈。

## 公司 PPT：Towards Enterprise RSI（pitch reel v3）

原件在公司私有仓库，不放在这里：`evolocity-lab/Operad` → `docs/demo/operad-pitch-reel-v3.html`（用公司账号打开）。

以下是我的摘要，共 9 页：

1. **封面**：Evolocity，目标是企业级 RSI（递归自我改进）。
2. **定位**：一个会持续学习的核心，用来构建和测试 Agent（build → test → refine）。不同业务的 Agent（代码维护、发布风险、事故根因等）共享已学到的东西；用客户自己的模型、工具和 Harness，可部署在客户的 VPC 或机房，关键步骤有人工审批。
3. **Harness 进化**：Skills、Memory、Tools、Plugins、Agent loop 经常改，模型很少改。循环是"运行 → 记录轨迹 → 总结 → 变聪明"，每次改动都在沙箱里评测、有版本号，变好就上线，变差就回滚。
4. **ARC-AGI-3**：和 Codex 用同一个模型比，Operad 全部通关，输入 Token 少约三分之二，总时间和操作步数也更少。
5. **便宜模型也能拿高分**：Operad 配 DeepSeek Flash，每个任务几美分，在 Terminal-Bench 和 SkillsBench 上和主流 Agent + 大模型的成绩相当或更高（注意：任务集和预算不同，不是严格同条件对比）。
6. **私有评测**：在客户自己的任务上，比较各种 Harness × 模型的组合；每次运行用独立沙箱，Verifier 和被测 Agent 分开，记录分数、成本、Token、时间和完整轨迹。
7. **评测基础设施**：沙箱可以用客户自己的云；一个控制器能并行跑大量 Agent、断点续跑；接入现有 benchmark 和客户自己的测试；进化策略可插拔（固定进化搜索、单 Agent 自我迭代、多 Agent 共享记忆）。
8. **进行中的工作**：Terminal-Bench 上的自我改进、GPU kernel 的 Harness 进化，以及 **Playtest agent**（player agent 用代码玩游戏，reviewer agent 按 rubric 打分）——我在 FunPlus 做的就是这个。
9. **私有评测的数据来源**：游戏代码仓库、世界模型数据、3D 资产、游戏和合成视频。
