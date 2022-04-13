# 提案介绍
该方案将 Cuelib 内置到 Stack 仓库，使 Cuelib 可以跨 Stack 引用，以解决 submodule 开发和维护难的问题，将 Cuelib 和 Stack 在目录结构上进一步解耦，为将来实现 Stack 之间的代码复用做铺垫。

以往 submodule 模式的开发过程比较麻烦，需要涉及两个串行步骤：

* 更新 Cuelib 仓库：修改 Cuelib -> 推送到 Cuelib 仓库 -> 提 PR -> 评审 -> 合并
* 更新 Stack Cuelib Submodule 版本：在 Stack 根目录执行 `git submodule update --remote` -> 推送到 Git 仓库 -> 提 PR -> 评审 -> 合并

Submodule 的开发方式主要的问题是 Stack Submodule 目录容易出现更新不及时，导致 Stack 功能失效。

新的方案只需要进行一次上述流程，且不会出现 Stack 和 Cuelib 版本不一致的问题，从贡献的角度来说也降低了难度。

# 如何开发
## 安装依赖
```
git clone git@github.com:lyzhang1999/stack.git
hof mod vendor cue && dagger project update
```

## 执行 Stack Plan
```
cd gin-next
dagger do test -p ./plans
```

# Release Stack
Stack 发布包时需要将 `cue.mod` 和 `cue/mods` 一并复制到发行包下。

```
.
└── gin-next
    ├── code
    ├── cue.mod
    ├── cue.mods
    └── plans
```

详情：https://github.com/lyzhang1999/stack-package-layout

# 现有 cuelib 仓库处理问题

两个选择：

* 废弃
* 从 Stack Cueilb 目录自动更新