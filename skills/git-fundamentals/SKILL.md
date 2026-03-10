# Git 基础技能 (Git Fundamentals)

## 概述
Git 版本控制基础技能，涵盖从入门到进阶的核心概念和命令。

## 核心概念

### 1. Git 是什么
```
Git = 分布式版本控制系统
作用：追踪代码变化、协作开发、版本管理
```

### 2. 三个区域
```
工作区 (Working Directory) → 你看到的文件
暂存区 (Staging Area)    → git add 后存放
仓库区 (Repository)       → git commit 后存储
```

### 3. 基本流程
```
修改文件 → git add → git commit → git push
```

---

## 基础命令

### 配置命令
```bash
# 首次使用配置
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# 查看配置
git config --list

# 查看特定配置
git config user.name
git config user.email
```

### 初始化仓库
```bash
# 创建新仓库
git init

# 克隆现有仓库
git clone https://github.com/username/repo.git

# 克隆到指定目录
git clone https://github.com/username/repo.git my-folder
```

### 查看状态
```bash
# 查看仓库状态
git status

# 查看提交历史
git log

# 简洁的日志
git log --oneline

# 查看文件差异
git diff

# 查看暂存区差异
git diff --staged
```

---

## 文件操作

### 添加文件
```bash
# 添加单个文件
git add filename.txt

# 添加所有文件
git add .

# 添加所有文件（包括隐藏文件）
git add -A

# 交互式添加
git add -p
```

### 提交更改
```bash
# 提交并写消息
git commit -m "提交说明"

# 提交所有修改的文件
git commit -am "提交说明"

# 修改上次提交
git commit --amend -m "新的提交说明"

# 查看提交详情
git show
git show commit-hash
```

### 删除文件
```bash
# 删除文件并记录
git rm filename.txt

# 删除文件但保留在工作区
git rm --cached filename.txt

# 重命名文件
git mv old-name.txt new-name.txt
```

---

## 分支管理

### 分支操作
```bash
# 查看所有分支
git branch

# 查看包括远程分支
git branch -a

# 创建新分支
git branch feature-name

# 创建并切换到新分支
git checkout -b feature-name

# 新语法（推荐）
git switch -c feature-name

# 切换分支
git checkout branch-name
git switch branch-name

# 删除分支
git branch -d feature-name

# 强制删除分支
git branch -D feature-name
```

### 合并分支
```bash
# 切换到主分支
git checkout main

# 合并功能分支
git merge feature-name

# 查看合并历史
git log --graph --oneline
```

---

## 远程仓库

### 远程操作
```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/username/repo.git

# 重命名远程仓库
git remote rename origin upstream

# 删除远程仓库
git remote remove origin
```

### 推送和拉取
```bash
# 推送到远程
git push origin main

# 首次推送（设置上游）
git push -u origin main

# 强制推送（谨慎使用）
git push -f origin main

# 拉取远程更改
git pull origin main

# 只获取不合并
git fetch origin

# 拉取特定分支
git pull origin feature-branch
```

---

## 撤销操作

### 撤销工作区更改
```bash
# 撤销单个文件
git checkout -- filename.txt
# 或
git restore filename.txt

# 撤销所有文件
git checkout -- .
git restore .
```

### 撤销暂存区
```bash
# 从暂存区移除
git reset HEAD filename.txt
git restore --staged filename.txt
```

### 撤销提交
```bash
# 撤销最后一次提交（保留更改）
git reset --soft HEAD~1

# 撤销提交和暂存（保留工作区）
git reset --mixed HEAD~1
git reset HEAD~1  # 默认

# 完全撤销（删除更改）
git reset --hard HEAD~1

# 撤销远程提交
git revert commit-hash  # 创建新提交撤销
```

---

## 高级操作

### 暂存更改
```bash
# 暂存当前工作
git stash

# 查看暂存列表
git stash list

# 恢复暂存
git stash pop

# 应用暂存但不删除
git stash apply

# 删除暂存
git stash drop

# 清空所有暂存
git stash clear
```

### 查看历史
```bash
# 查看提交历史
git log
git log --oneline
git log --graph
git log --all --graph --oneline

# 查看特定文件历史
git log filename.txt

# 查看某人提交
git log --author="name"

# 查看最近 N 次
git log -n 5
```

### 标签管理
```bash
# 创建标签
git tag v1.0.0

# 创建带注释标签
git tag -a v1.0.0 -m "版本 1.0.0"

# 查看标签
git tag

# 推送标签
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 删除标签
git tag -d v1.0.0
git push origin --delete v1.0.0
```

---

## Git Rebase（重要）

### 基础 Rebase
```bash
# 变基到最新
git checkout feature-branch
git rebase main

# 交互式变基（修改提交历史）
git rebase -i HEAD~3

# 继续变基
git rebase --continue

# 中止变基
git rebase --abort
```

### Rebase vs Merge
```
Merge: 保留完整历史，创建合并节点
Rebase: 线性历史，更清晰

推荐：
- 本地分支用 rebase
- 共享分支用 merge
- 绝不 rebase 已推送的公共分支
```

---

## Git Reflog（安全网）

```bash
# 查看所有操作历史
git reflog

# 恢复误删的分支
git reflog
git checkout -b recovered-branch HEAD@{2}

# 恢复误删的提交
git reflog
git reset --hard HEAD@{1}
```

**重要**: Reflog 保留 90 天内的操作，是恢复误操作的最后手段！

---

## 最佳实践

### 提交信息规范
```
格式：<type>(<scope>): <subject>

类型:
feat:     新功能
fix:      Bug 修复
docs:     文档更新
style:    代码格式
refactor: 重构
test:     测试
chore:    构建/工具

示例:
feat(auth): 添加用户登录功能
fix(api): 修复数据接口错误
docs: 更新 README 文档
```

### 分支命名规范
```
main/master        - 主分支
develop            - 开发分支
feature/xxx        - 功能分支
bugfix/xxx         - Bug 修复
hotfix/xxx         - 紧急修复
release/v1.0.0     - 发布分支
```

### 工作流建议
```
1. 从 main 创建功能分支
2. 在功能分支开发
3. 定期 rebase main 保持同步
4. 完成后提交 PR
5. 审查通过后合并
6. 删除功能分支
```

---

## 常见场景

### 场景 1: 首次贡献
```bash
# 1. Fork 项目
# GitHub 上点击 Fork

# 2. Clone 到本地
git clone https://github.com/your-username/repo.git

# 3. 创建分支
git checkout -b feature/my-feature

# 4. 开发并提交
git add .
git commit -m "feat: add new feature"

# 5. 推送
git push -u origin feature/my-feature

# 6. 创建 Pull Request
# GitHub 上点击 Compare & pull request
```

### 场景 2: 同步上游
```bash
# 添加上游仓库
git remote add upstream https://github.com/original/repo.git

# 获取上游更改
git fetch upstream

# 合并到本地
git checkout main
git merge upstream/main

# 推送到自己的仓库
git push origin main
```

### 场景 3: 解决冲突
```bash
# 1. 拉取最新代码
git pull origin main

# 2. 如果有冲突，Git 会提示
# 3. 打开冲突文件，手动解决
# <<<<<<< HEAD
# 你的更改
# =======
# 其他人的更改
# >>>>>>>

# 4. 解决后标记为已解决
git add filename.txt

# 5. 完成合并
git commit
```

### 场景 4: 清理历史
```bash
# 交互式变基
git rebase -i HEAD~5

# 编辑器中会显示：
pick abc123 提交 1
pick def456 提交 2
pick ghi789 提交 3

# 可以修改为：
pick abc123 提交 1
squash def456 提交 2  # 合并到上一个
fixup ghi789 提交 3   # 合并且丢弃提交信息
```

---

## 实用技巧

### 1. 别名配置
```bash
# 添加到 ~/.gitconfig
[alias]
    co = checkout
    br = branch
    ci = commit
    st = status
    lg = log --graph --oneline
    unstage = reset HEAD --
    last = log -1 HEAD
    who = shortlog -sn
```

### 2. 快速搜索
```bash
# 搜索提交历史
git log --all --grep="关键词"

# 搜索代码
git grep "关键词"

# 搜索某人修改
git log --author="name" --oneline
```

### 3. 统计信息
```bash
# 查看贡献统计
git shortlog -sn

# 查看文件变更统计
git log --stat

# 查看代码行数变化
git log --numstat
```

### 4. 比较分支
```bash
# 比较两个分支
git diff branch1..branch2

# 比较分支文件
git diff branch1..branch2 -- filename.txt
```

---

## 故障排除

### 问题 1: 推送被拒绝
```bash
# 原因：远程有本地没有的提交
# 解决：先拉取再推送
git pull --rebase
git push

# 或强制推送（谨慎）
git push -f
```

### 问题 2: 误删分支
```bash
# 查看 reflog
git reflog

# 找到删除前的 commit
# 恢复分支
git checkout -b recovered-branch <commit-hash>
```

### 问题 3: 提交错分支
```bash
# 1. 撤销提交（保留更改）
git reset --soft HEAD~1

# 2. 切换到正确分支
git checkout correct-branch

# 3. 重新提交
git commit -m "提交说明"
```

### 问题 4: 忘记密码/Token
```bash
# 清除保存的凭据
# macOS
git credential-osxkeychain erase

# Windows
控制面板 → 凭据管理器 → 删除 Git 凭据

# Linux
rm ~/.git-credentials
```

---

## 安全注意事项

### 1. 不要提交敏感信息
```
❌ 密码、API Key、Token
❌ 数据库配置
❌ 私人密钥
❌ 个人隐私信息
```

### 2. 如果已提交敏感信息
```bash
# 1. 立即撤销该密钥
# 2. 使用 BFG 或 filter-branch 清理历史
# 3. 强制推送
git push -f origin main
# 4. 通知所有协作者重新 clone
```

### 3. 使用 .gitignore
```
# 创建 .gitignore 文件
node_modules/
.env
*.log
.DS_Store
dist/
build/
```

---

## 学习资源

### 官方文档
- Git Book: https://git-scm.com/book
- GitHub Docs: https://docs.github.com

### 在线练习
- Learn Git Branching: https://learngitbranching.js.org/
- Git Immersion: https://gitimmersion.com/

### 可视化工具
- GitKraken
- SourceTree
- GitHub Desktop
- VS Code Git 插件

---

## 快速参考卡

```
# 日常操作
git status          # 查看状态
git add .           # 添加文件
git commit -m "msg" # 提交
git push            # 推送
git pull            # 拉取

# 分支操作
git branch          # 查看分支
git checkout -b     # 创建并切换
git merge           # 合并
git branch -d       # 删除

# 撤销操作
git restore         # 恢复文件
git reset           # 重置提交
git revert          # 反向提交

# 查看历史
git log             # 提交历史
git reflog          # 所有操作
git diff            # 查看差异
```

---

**提示**: Git 最强大的功能是 reflog，误操作几乎都能恢复！
