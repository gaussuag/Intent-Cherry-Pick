# Intent-Cherry-Pick
这是一个融合 git cherry-pick 与 Code Agent 能力的代码合并工作流。它保留传统 cherry-pick 对 source commit 的忠实复现能力，同时引入 Code Agent 对代码上下文、调用链、分支差异和改动意图的理解，在遇到冲突或目标分支实现不同的时候，像人类解决冲突一样进行最小手工融合。它的目标不是重新设计或优化代码，而是在保护 source commit 实现事实的同时，保留 target 分支已有逻辑，完成可审查、可追溯、可留档的代码合入。
