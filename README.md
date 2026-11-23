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
