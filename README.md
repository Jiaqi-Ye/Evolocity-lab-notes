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

## Company deck: Towards Enterprise RSI (pitch reel v3)

The original lives in the company's private repository and is not copied here: `evolocity-lab/Operad` → `docs/demo/operad-pitch-reel-v3.html` (open it with the company account).

My summary of the 9 slides:

1. **Cover:** Evolocity, aiming at enterprise RSI (recursive self-improvement).
2. **Positioning:** a continuously learning core for building and testing agents (build → test → refine). Agents for different jobs (code maintenance, release risk, incident root cause and more) share what has been learned; it uses the customer's own models, tools and harnesses, can be deployed in the customer's VPC or on-prem, and keeps human approval on key steps.
3. **Harness evolution:** skills, memory, tools, plugins and the agent loop change often; the model rarely does. The loop is "run → trace → consolidate → smarter"; every change is evaluated in a sandbox and versioned, then promoted if better or rolled back if worse.
4. **ARC-AGI-3:** against Codex on the same model, Operad wins every game with about two thirds fewer input tokens, and also less total time and fewer actions.
5. **High scores on a cheap model:** Operad with DeepSeek Flash, a few cents per task, matches or beats mainstream agents with large models on Terminal-Bench and SkillsBench (note: task sets and budgets differ, so it is not a strict like-for-like comparison).
6. **Private evaluation:** on the customer's own tasks, compare every harness × model combination; each run gets its own sandbox, the verifier is separate from the agent under test, and score, cost, tokens, time and the full trace are recorded.
7. **Evaluation infrastructure:** sandboxes can run on the customer's own cloud; one controller runs many agents in parallel and resumes where it stopped; it plugs into existing benchmarks and the customer's own tests; the evolution strategy is pluggable (fixed evolutionary search, a single self-iterating agent, or many agents with shared memory).
8. **Work in progress:** self-improvement on Terminal-Bench, harness evolution for GPU kernels, and the **playtest agent** (a player agent plays the game through code, a reviewer agent scores it against rubrics), which is what I work on for FunPlus.
9. **Data for private evaluation:** game code repositories, world-model data, 3D assets, and gameplay and synthetic video.
