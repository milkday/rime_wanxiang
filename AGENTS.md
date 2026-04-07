# AGENTS.md

## 项目定位

这是一个部署在 `/Users/zzf/Library/Rime` 的 Rime 配置仓库。

- 该路径是 Rime 直接使用的配置目录，不能随意改名或迁移到别的路径后再假设运行正常。
- 这是用户自己的维护仓库，基于上游项目 `amzxyz/rime_wanxiang` 做个性化调整。
- 当前只使用单分支模型：`wanxiang`。

## Git 远程约定

- `origin`：用户自己的 fork，用于日常 `push`
  - `https://github.com/milkday/rime_wanxiang`
- `upstream`：原作者仓库，用于同步上游提交
  - `https://github.com/amzxyz/rime_wanxiang.git`

AI 在修改 Git 配置前，应先确认当前远程关系仍然符合上面的约定：

```bash
git remote -v
git branch -vv
```

除非用户明确要求，否则不要改动分支模型，不要新增复杂工作流，不要把单分支维护改造成多分支体系。

## 分支策略

- 只维护一个分支：`wanxiang`
- 本地 `wanxiang` 应跟踪 `origin/wanxiang`
- 不主动创建 `main`、`master`、`develop`、`feature/*` 等额外分支，除非用户明确提出

目标是保持简单，能不动就不动。

## 标准 Git 操作流程

### 1. 推送自己的修改

当用户在本地完成修改后，默认推送到自己的 fork：

```bash
git add <files>
git commit -m "<message>"
git push origin wanxiang
```

如果本地跟踪关系正常，也可以直接：

```bash
git push
```

### 2. 同步上游更新

当需要获取原作者最新提交时，使用以下流程：

```bash
git fetch upstream
git merge upstream/wanxiang
git push origin wanxiang
```

说明：

- `fetch` 从原作者仓库获取更新
- `merge upstream/wanxiang` 将上游提交合并到当前本地 `wanxiang`
- `push origin wanxiang` 把合并后的结果推送到用户自己的 fork

除非用户明确要求，否则不要默认改用 `rebase`，因为当前目标是稳定、直接、低心智负担。

## AI 修改代码时的边界

AI 在这个仓库中工作时，应优先遵守以下原则：

- 优先做最小修改，不做无关重构
- 不因为“看起来更规范”就批量改文件名、目录结构或格式
- 不随意调整现有 Git 远程、分支、标签策略
- 不主动引入复杂自动化脚本来包装简单的 Git 操作
- 不把 Rime 运行时生成的本地文件提交进仓库

## Rime 目录特殊注意事项

这是 Rime 实际使用的目录，不只是开发副本，因此要特别注意：

- 不要删除用户的本地运行数据，除非用户明确要求
- 不要随意清空 `build/`、`*.userdb/`、`user.yaml`、`installation.yaml`
- 即使这些文件通常不纳入 Git，也可能对用户当前输入法状态有实际影响
- 修改配置后，如果需要用户验证，应提醒其重新部署 Rime，而不是假设配置会自动生效

## `.gitignore` 约定

以下内容属于本地运行产物，默认不应提交：

- `build/`
- `*.userdb/`
- `user.yaml`
- `installation.yaml`
- 以及仓库现有 `.gitignore` 中已经列出的临时文件

AI 修改 `.gitignore` 时，应保持这个方向，不要把用户真实维护的配置文件误加入忽略列表。

## 提交前检查

在执行提交前，AI 应至少检查：

```bash
git status --short --branch
```

重点确认：

- 是否只包含与当前任务相关的修改
- 是否误包含了 Rime 运行时产物
- 是否误改了无关文件

如果发现工作区里有用户未说明的现有改动，应避免覆盖，并基于现状谨慎继续。

## 推荐的 AI 行为

如果用户让 AI “查看项目并继续做事”，AI 应优先按下面顺序理解上下文：

1. 先读本文件 `AGENTS.md`
2. 再看当前任务相关文件
3. 必要时检查 `git remote -v`、`git branch -vv`、`git status --short --branch`
4. 仅在任务确实需要时再调整 Git 配置

## 禁止事项

除非用户明确要求，否则 AI 不应执行以下操作：

- `git reset --hard`
- `git clean -fd`
- 删除本地 Rime 用户数据
- 改造为多分支开发模型
- 重写提交历史
- 修改远程仓库指向，使 `origin` 不再指向用户 fork

## 面向未来的维护原则

这个仓库的目标不是做复杂的 Git 实验，而是：

- 保持与上游 `amzxyz/rime_wanxiang` 的可同步性
- 保留用户自己的定制修改
- 让日常维护流程足够简单，用户和 AI 都不需要额外记忆复杂规则

如果后续任务与这些原则冲突，应优先选择更简单、更稳妥、侵入性更低的方案。
