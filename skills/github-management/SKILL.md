# GitHub 管理技能 (GitHub Management)

## 概述
全面的 GitHub 仓库管理、协作流程和自动化技能。

## 核心功能

### 1️⃣ 仓库管理

#### 创建仓库
```bash
# 本地创建并推送
git init my-project
cd my-project
git remote add origin https://github.com/username/my-project.git
git branch -M main
git add .
git commit -m "Initial commit"
git push -u origin main
```

#### 仓库设置
- [ ] 设置分支保护规则
- [ ] 配置 CI/CD
- [ ] 添加协作者
- [ ] 设置 Issue 模板
- [ ] 配置 Pull Request 模板
- [ ] 启用 GitHub Actions

### 2️⃣ 分支管理

#### 分支策略
```
main          - 生产环境代码（受保护）
develop       - 开发分支
feature/*     - 功能分支
bugfix/*      - 修复分支
release/*     - 发布分支
hotfix/*      - 紧急修复
```

#### 常用操作
```bash
# 创建功能分支
git checkout -b feature/new-feature

# 同步主分支
git fetch origin
git rebase origin/main

# 合并分支
git checkout main
git merge --no-ff feature/new-feature

# 删除已合并分支
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

### 3️⃣ Pull Request 流程

#### 创建 PR
```bash
# 推送分支
git push origin feature/new-feature

# 然后到 GitHub 创建 PR
# 或使用 gh CLI
gh pr create \
  --title "feat: 添加新功能" \
  --body "## 变更内容\n- 新增 XXX 功能\n- 优化 YYY 性能" \
  --base main \
  --head feature/new-feature
```

#### PR 检查清单
- [ ] 代码通过 lint 检查
- [ ] 所有测试通过
- [ ] 代码审查完成
- [ ] 文档已更新
- [ ] 变更日志已更新

### 4️⃣ Issue 管理

#### 创建 Issue
```bash
# 使用 gh CLI
gh issue create \
  --title "Bug: 登录页面显示异常" \
  --body "## 问题描述\n...\n## 复现步骤\n..." \
  --label "bug,high-priority" \
  --assignee "username"
```

#### Issue 模板
```markdown
---
name: Bug Report
about: 报告一个 bug
title: '[Bug] '
labels: 'bug'
---

## 问题描述
[清晰简洁地描述问题]

## 复现步骤
1. 
2. 
3. 

## 期望行为
[描述期望发生什么]

## 环境信息
- OS: 
- Browser: 
- Version: 
```

### 5️⃣ GitHub Actions

#### 基础工作流
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Run lint
        run: npm run lint

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy
        run: |
          echo "Deploying to production..."
```

#### 常用 Actions
| Action | 用途 |
|--------|------|
| `actions/checkout` | 检出代码 |
| `actions/setup-node` | 设置 Node.js |
| `actions/setup-python` | 设置 Python |
| `actions/cache` | 缓存依赖 |
| `actions/upload-artifact` | 上传构建产物 |
| `actions/download-artifact` | 下载构建产物 |

### 6️⃣ GitHub CLI (gh)

#### 安装
```bash
# macOS
brew install gh

# 验证安装
gh --version
```

#### 常用命令
```bash
# 认证
gh auth login

# 查看仓库
gh repo view

# 创建仓库
gh repo create my-project --public --clone

# 查看 PR
gh pr list
gh pr view <number>

# 创建 PR
gh pr create

# 合并 PR
gh pr merge <number> --merge

# 查看 Issue
gh issue list
gh issue view <number>

# 创建 Issue
gh issue create

# 查看 CI 状态
gh run list
gh run view <run-id>

# 重新运行 CI
gh run rerun <run-id>
```

### 7️⃣ 协作流程

#### Fork & Pull 模式
```bash
# 1. Fork 仓库
gh repo fork original/repo --clone

# 2. 创建分支
git checkout -b feature/my-feature

# 3. 提交更改
git add .
git commit -m "feat: add new feature"

# 4. 推送到自己的仓库
git push origin feature/my-feature

# 5. 创建 PR 到原仓库
gh pr create --repo original/repo
```

#### 代码审查
```bash
# 查看 PR 详情
gh pr view <number> --comments

# 添加评论
gh pr comment <number> --body "LGTM! 但有几点建议..."

# 批准 PR
gh pr review <number> --approve

# 请求更改
gh pr review <number> --request-changes --body "需要修改..."
```

### 8️⃣ 版本发布

#### 创建 Release
```bash
# 打标签
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0

# 创建 Release
gh release create v1.0.0 \
  --title "Release v1.0.0" \
  --notes "## 新功能\n- ...\n## Bug 修复\n- ..." \
  --generate-notes
```

#### 发布工作流
```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Build
        run: npm run build
      
      - name: Create Release
        uses: softprops/action-gh-release@v1
        with:
          files: dist/*
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 9️⃣ 安全与权限

#### 分支保护
```
Settings → Branches → Add branch protection rule

规则设置:
- Require pull request reviews before merging: 2
- Require status checks to pass before merging: ✓
- Require branches to be up to date before merging: ✓
- Include administrators: ✓
```

#### 访问令牌
```bash
# 创建个人访问令牌
# Settings → Developer settings → Personal access tokens

# 使用令牌
git clone https://<TOKEN>@github.com/username/repo.git
```

#### 密钥管理
```yaml
# 在 GitHub Actions 中使用密钥
steps:
  - name: Deploy
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: |
      echo "Using API key: $API_KEY"
```

### 🔟 自动化脚本

#### 批量操作
```bash
#!/bin/bash
# 批量更新所有仓库

repos=$(gh repo list --limit 100 --json name -q '.[].name')

for repo in $repos; do
    echo "Updating $repo..."
    cd $repo
    git pull origin main
    npm update
    git add package-lock.json
    git commit -m "chore: update dependencies"
    git push
    cd ..
done
```

#### PR 自动合并
```yaml
name: Auto-Merge

on:
  pull_request:
    types: [labeled]

jobs:
  auto-merge:
    runs-on: ubuntu-latest
    if: github.event.label.name == 'automerge'
    steps:
      - uses: pascalgn/automerge-action@v0.16.3
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 最佳实践

### Git 提交规范
```
feat: 新功能
fix: Bug 修复
docs: 文档更新
style: 代码格式
refactor: 重构
test: 测试
chore: 构建/工具
```

### PR 命名规范
```
feat: 添加用户登录功能
fix: 修复首页加载错误
docs: 更新 API 文档
refactor: 重构认证模块
```

### 分支命名规范
```
feature/user-authentication
bugfix/login-error
hotfix/critical-security-fix
release/v1.0.0
```

## 常用工作流

### 日常开发
```bash
# 1. 同步主分支
git checkout main
git pull origin main

# 2. 创建功能分支
git checkout -b feature/new-feature

# 3. 开发并提交
git add .
git commit -m "feat: implement new feature"

# 4. 推送并创建 PR
git push -u origin feature/new-feature
gh pr create --fill

# 5. 等待审查和合并
```

### 紧急修复
```bash
# 1. 从 main 创建 hotfix 分支
git checkout main
git pull
git checkout -b hotfix/critical-bug

# 2. 修复并提交
git add .
git commit -m "fix: resolve critical bug"

# 3. 创建紧急 PR
gh pr create --title "fix: critical bug" --label "urgent"

# 4. 加速审查流程
# 通知团队进行快速审查

# 5. 合并后打标签
git tag -a v1.0.1 -m "Hotfix for critical bug"
git push origin v1.0.1
```

## 监控与报告

### 仓库统计
```bash
# 查看仓库活动
gh api repos/owner/repo/traffic/views

# 查看克隆统计
gh api repos/owner/repo/traffic/clones

# 查看 PR 统计
gh pr list --state all --json number,title,createdAt,mergedAt
```

### CI/CD 监控
```bash
# 查看最近的运行
gh run list --limit 10

# 查看特定运行详情
gh run view <run-id> --log

# 下载运行产物
gh run download <run-id>
```

## 故障排除

### 常见问题

| 问题 | 解决方案 |
|------|----------|
| 推送被拒绝 | git pull --rebase 后再推送 |
| 合并冲突 | git mergetool 手动解决 |
| CI 失败 | gh run view 查看日志 |
| PR 无法合并 | 检查分支保护和状态检查 |
| 权限错误 | 检查令牌和协作者权限 |

### 恢复操作
```bash
# 撤销最后一次提交
git reset --soft HEAD~1

# 撤销已推送的提交
git revert <commit-hash>
git push

# 恢复删除的分支
git reflog
git checkout -b recovered-branch <commit-hash>
```

---

**提示**: 熟练使用 `gh` CLI 可以大幅提升 GitHub 管理效率！
