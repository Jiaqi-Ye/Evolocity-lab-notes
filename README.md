**English** | [中文](README.zh-CN.md)

## Understanding Operad
### What is Operad?

Operad is Evolocity's in-house agent foundation, built so that agents keep learning. When it runs a task, the model writes code into a long-lived Python environment to do the work, which makes it efficient. At the same time, it evaluates agents automatically in sandboxes and uses the results to improve the harness, so agents get better with use without retraining the underlying model.

### How does it save tokens?

Two main ways:

1. **Code-driven work:** the model writes Python programs that do operations in bulk. For example, it checks 100 files and returns only "found 3 errors" instead of having the model read every file. This cuts both input tokens and the number of model calls.
2. **Persistent Python environment:** loaded data and computed results stay in the environment and are reused next time, with no re-reading or recomputing. This mainly saves time, and may also save tokens by not resending the same content.

**Measured:** on ARC-AGI-3, with Operad and Codex on the same model, Operad won all 25 games using 67% fewer input tokens.

### How is it different from other agents?

Codex and Claude Code can also write code and run Python, but they mostly rely on tools that read files, search and run shell commands. Operad has no such tools: all work happens inside the Python environment, and only the important results go back to the model. On top of that, it evaluates and improves its harness automatically, which lowers cost and raises efficiency.

An example from the deck: Operad with the low-cost DeepSeek Flash model spends only 5–7 cents per task and scores higher on SkillsBench than Codex and Claude Code.

### Why does it work across companies?

Operad is a general foundation. Each company customizes the harness for its own tasks, tools and evaluation criteria instead of building a whole system from scratch, and can deploy it on its own cloud.

- **Operad (the general foundation):** provides the Python execution environment, plus the infrastructure for evaluation and evolution: sandboxes, and a controller that runs many agents at once.
- **Harness (the customized part):** the prompt, skills, tools, memory and agent loop configured for a business task. Every change is evaluated and versioned first; it ships if it is better and is rolled back if it is worse.
- **Verifier (the evaluation criteria):** separate from the agent under test; it judges whether the agent meets the bar and gives feedback for improving the harness.

## Company deck

[Towards Enterprise RSI (pitch reel v3)](https://github.com/evolocity-lab/Operad/blob/main/docs/demo/operad-pitch-reel-v3.html)
