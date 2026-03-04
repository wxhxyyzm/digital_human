# 分支策略（简单）

目的：保证 `main` 仅用于发布，可追溯且稳定；日常开发在 `dev` 与 feature 分支上进行。

主要分支
- `main`：仅用于发布（可打 tag）。任何合入 `main` 的代码应为已测试、可发布的代码。
- `dev`：日常开发分支，所有 feature 分支应基于 `dev` 开发并向 `dev` 发起合并请求（PR）。

开发工作流
1. 从 `dev` 创建 feature 分支：
   - `git checkout dev`
   - `git pull origin dev`
   - `git checkout -b feature/your-name-brief`

2. 在 feature 分支提交（小步、描述清楚）并推送：
   - `git add <file>`
   - `git commit -m "feat: 描述功能"`
   - `git push -u origin feature/your-name-brief`

3. 在远程创建 Pull Request，目标分支为 `dev`。通过 CI / 代码审核后合并。

4. 合并后：
   - 本地切回 `dev` 并拉取最新：`git checkout dev && git pull origin dev`
   - 可删除远程 feature 分支：`git push origin --delete feature/your-name-brief`

发布流程（把 `dev` 的稳定内容发布到 `main`）
1. 在准备发布时，切到 `dev` 并确保最新：`git checkout dev && git pull origin dev`
2. 切回 `main` 并合并 `dev`（使用合并或 rebase，根据团队偏好）：
   - `git checkout main`
   - `git pull origin main`
   - `git merge --no-ff dev`   # 产生合并提交，便于追踪发布点
   - `git push origin main`
3. 在 `main` 上打版本标签（可选）：
   - `git tag -a v1.2.0 -m "Release v1.2.0"`
   - `git push origin v1.2.0`

建议与注意事项
- 保持小而频繁的提交，便于回滚与代码审查。
- 代码审查通过后再合并到 `dev`，CI 必须通过再合并到 `main`。
- 如果需要在 `main` 上做紧急修复（hotfix），可以基于 `main` 新建 `hotfix/*` 分支，修复并合并回 `main` 与 `dev`。

常用命令速查
```bash
# 创建并推送 dev（如果尚未存在）
git checkout main
git pull origin main
git checkout -b dev
git push -u origin dev

# 新建 feature
git checkout dev
git pull origin dev
git checkout -b feature/xxx

# 合并 feature 到 dev（通过 PR 在远端合并，或本地合并后推送）
git checkout dev
git merge --no-ff feature/xxx
git push origin dev
```

以上为简明分支策略示例，可根据团队规模与 CI 要求调整。
