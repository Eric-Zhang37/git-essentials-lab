# Git Essentials Lab 单行命令参考

本页只列出应在终端中逐行输入的命令。默认使用 Variant A。

- 先在 launcher 中执行每题的 `start`。
- 然后进入 `start` 输出的 `Workspace:` 路径。
- `check` 必须调用 launcher 中的 `lab.py`；请把 `<LAUNCHER>` 换成 launcher 的实际路径。
- `<WORKSPACE>`、`<YOUR-FORK>`、`<PR-NUMBER>` 和 `<COPIED-COMMIT-ID>` 必须换成实际值。
- “编辑文件”不是 Git 命令，需要在编辑器中完成指定内容后再继续输入下一条命令。
- 每条命令单独输入，不要把整页一次性粘贴到终端。

## 公共初始化（launcher 中）

```text
git remote -v
git remote add upstream https://github.com/oh-gnues/git-essentials-lab.git
git fetch origin --tags
git fetch upstream --tags "+refs/heads/*:refs/remotes/upstream/*"
py -3 lab.py doctor
py -3 lab.py refs
```

如果 `upstream` 已存在，不要再次执行 `git remote add upstream ...`。

## Exercise 01

在 launcher 中：

```text
py -3 lab.py start 01
```

进入新工作区后：

```text
git status
git switch -c feature/search
```

编辑 `src/library/Catalog.java`：使用 `Locale.ROOT` 将查询和标题统一为小写，同时保留 substring matching。

```text
git diff
git add src/library/Catalog.java
git commit -m "feat: make catalog search case-insensitive"
git switch result/ex01
git merge --ff-only feature/search
git log --oneline --graph --decorate -5
py -3 "<LAUNCHER>\lab.py" check 01 --repo .
git push -u origin result/ex01
```

## Exercise 02

在 launcher 中：

```text
py -3 lab.py start 02
```

进入新工作区后：

```text
git remote -v
git branch -vv
```

创建 `docs/my-run-guide.md`，写明 Python、Git、JDK 前置条件、精确命令 `python3 run.py demo`，并说明 demo 的作用。

```text
git status
git add docs/my-run-guide.md
git commit -m "docs: add library demo run guide"
py -3 "<LAUNCHER>\lab.py" check 02 --repo .
git push -u origin result/ex02
git branch -vv
```

## Exercise 03

在 launcher 中：

```text
py -3 lab.py start 03
```

进入新工作区后：

```text
git status
git branch -vv
git log --oneline --graph --decorate HEAD practice/practice
```

回到 launcher，模拟 teammate 更新：

```text
py -3 lab.py advance 03 --repo "<WORKSPACE>"
```

回到练习工作区：

```text
git fetch practice
git log --oneline --graph --decorate HEAD practice/practice
git pull --ff-only practice practice
git log --oneline --graph --decorate HEAD practice/practice
py -3 "<LAUNCHER>\lab.py" check 03 --repo .
```

此题不推送。

## Exercise 04

在 launcher 中：

```text
py -3 lab.py start 04
```

进入新工作区后：

```text
git log --oneline --graph --decorate HEAD lab-v1/ex04/faculty
git push -u origin result/ex04
git switch -c feature/ex04
git merge lab-v1/ex04/faculty
```

冲突是预期结果。编辑 `src/library/LoanPolicy.java`，删除冲突标记，并保留：student=3、faculty=5、loanDays=14、非负 fee=100/day。

```text
git status
git add src/library/LoanPolicy.java
git commit --no-edit
git log --oneline --graph --decorate -8
py -3 "<LAUNCHER>\lab.py" check 04 --repo .
git push -u origin feature/ex04
```

用 GitHub CLI 在自己的 fork 内创建并合并 PR：

```text
gh pr create --repo <YOUR-FORK> --base result/ex04 --head feature/ex04 --title "Exercise 04: integrate borrowing policies" --body "Resolved the policy conflict by retaining student=3 and faculty=5. Exercise 04 check passes."
gh pr merge <PR-NUMBER> --repo <YOUR-FORK> --merge
git fetch origin
git switch result/ex04
git merge --ff-only origin/result/ex04
py -3 "<LAUNCHER>\lab.py" check 04 --repo .
```

## Exercise 05

在 launcher 中：

```text
py -3 lab.py start 05
```

进入新工作区后：

```text
git diff -- src/library/LoanReceipt.java
git restore src/library/LoanReceipt.java
git status
git show lab-v1/ex05/bad-policy
git log --oneline --graph --decorate -6
git revert --no-edit lab-v1/ex05/bad-policy
git log -1 --format=full
py -3 "<LAUNCHER>\lab.py" check 05 --repo .
git push -u origin result/ex05
```

## Exercise 06

在 launcher 中：

```text
py -3 lab.py start 06
```

进入新工作区后：

```text
git status
git diff
git stash push -u -m "wip receipt and search notes"
git status
git stash list
```

编辑 `src/library/LoanPolicy.java`，把 loan period 从 14 改为 21。

```text
git diff
git add src/library/LoanPolicy.java
git commit -m "fix: extend loan period to 21 days"
git stash pop
git status
git diff
git stash list
py -3 "<LAUNCHER>\lab.py" check 06 --repo .
```

最终的 receipt 修改必须保持未提交，note 必须保持未跟踪。此题不推送。

## Exercise 07

在 launcher 中：

```text
py -3 lab.py start 07
```

进入新工作区后：

```text
git log --oneline --graph --decorate HEAD lab-v1/ex07/start
git status
git ls-remote --heads origin refs/heads/result/ex07
git add docs/receipt.md
git commit --amend -m "feat: improve loan receipt"
git log -1 --format=fuller
py -3 "<LAUNCHER>\lab.py" check 07 --repo .
git push -u origin result/ex07
```

## Exercise 08

在 launcher 中：

```text
py -3 lab.py start 08
```

进入新工作区后：

```text
git show --stat --oneline HEAD
git reset lab-v1/ex08/start
git status
git add src/library/LoanReceipt.java
git commit -m "feat: improve loan receipt"
git add docs/receipt.md
git commit -m "docs: explain loan receipt"
git log --oneline --graph --decorate -5
py -3 "<LAUNCHER>\lab.py" check 08 --repo .
```

此题不推送。

## Exercise 09

在 launcher 中：

```text
py -3 lab.py start 09
```

进入新工作区后：

```text
git log --reverse --oneline lab-v1/ex09/start..lab-v1/ex09/donor
git show cffed2379fcd1963bfc0fec3adbfa68aa28b31a2
git cherry-pick cffed2379fcd1963bfc0fec3adbfa68aa28b31a2
git status
git log --oneline --graph --decorate -5
py -3 "<LAUNCHER>\lab.py" check 09 --repo .
git push -u origin result/ex09
```

## Exercise 10

在 launcher 中：

```text
py -3 lab.py start 10
```

进入新工作区后：

```text
git log --oneline --graph --decorate HEAD lab-v1/ex10/upstream
git rebase -i --onto lab-v1/ex10/upstream lab-v1/ex10/start
```

在 rebase todo 编辑器中，将两行改成以下动作，保留编辑器显示的真实 commit ID：

```text
reword <FIRST-COMMIT-ID> wip receipt
fixup <SECOND-COMMIT-ID> wip receipt docs
```

保存后，在 commit message 编辑器中将消息改为：

```text
feat: improve loan receipt
```

rebase 完成后继续：

```text
git status
git log --oneline --graph --decorate -5
git log -1 --format=fuller
py -3 "<LAUNCHER>\lab.py" check 10 --repo .
git push -u origin result/ex10
```

## Exercise 11

在 launcher 中：

```text
py -3 lab.py start 11
```

复制 `start` 输出的完整 `Initial remote tip`。进入新工作区后：

```text
git log --oneline --graph --decorate HEAD practice/practice
git ls-remote practice refs/heads/practice
git commit --amend -m "feat: improve loan receipt"
git push practice result/ex11:refs/heads/practice
```

普通 push 被 non-fast-forward 拒绝是预期结果。把下面的 `<INITIAL-REMOTE-TIP>` 替换为刚才复制的完整 ID：

```text
git push --force-with-lease=refs/heads/practice:<INITIAL-REMOTE-TIP> practice result/ex11:refs/heads/practice
git ls-remote practice refs/heads/practice
git rev-parse HEAD
py -3 "<LAUNCHER>\lab.py" check 11 --repo .
```

此题只使用本地 `practice` remote，不推送 GitHub。

## Exercise 12

在 launcher 中：

```text
py -3 lab.py start 12
```

进入新工作区后：

```text
git log --oneline --graph --decorate HEAD
git reflog --oneline
```

从 reflog 复制 `feat: recoverable loan receipt` 对应的 commit ID，然后执行：

```text
git show <COPIED-COMMIT-ID>
git switch -c recovery/receipt <COPIED-COMMIT-ID>
git status
py -3 "<LAUNCHER>\lab.py" check 12 --repo .
```

此题不推送，保留本地 `recovery/receipt` 分支。

## Final

在 launcher 中：

```text
py -3 lab.py start final
```

进入新工作区后：

```text
git log --oneline --graph --decorate HEAD lab-v1/final/student lab-v1/final/faculty lab-v1/final/search lab-v1/final/donor
git push origin "lab-v1/final/start^{commit}:refs/heads/result/final"
git switch -c feature/final
git rebase -i lab-v1/final/start
```

在 rebase todo 编辑器中，将两个 checklist WIP 改成：

```text
reword <FIRST-CHECKLIST-ID> wip checklist
fixup <SECOND-CHECKLIST-ID> wip checklist tests
```

commit message 改为：

```text
docs: add release checklist
```

继续输入：

```text
git revert --no-edit lab-v1/final/bad-policy
git merge --no-edit lab-v1/final/student
git merge --no-edit lab-v1/final/faculty
```

faculty merge 冲突是预期结果。编辑 `src/library/LoanPolicy.java`，删除冲突标记，保留 student=3、faculty=5、loanDays=14；此时 fee 暂时仍为 `daysLate * 100`。

```text
git add src/library/LoanPolicy.java
git commit --no-edit
git merge --no-edit lab-v1/final/search
git log --reverse --oneline lab-v1/final/start..lab-v1/final/donor
git show 2c647b6
git cherry-pick 2c647b6
git status
git log --oneline --graph --decorate -15
py -3 "<LAUNCHER>\lab.py" check final --repo .
git push -u origin feature/final
```

在自己的 fork 内创建并合并 PR：

```text
gh pr create --repo <YOUR-FORK> --base result/final --head feature/final --title "Final: repair the tangled release" --body "Squashed checklist WIP, reverted the bad policy, merged student/faculty/search histories, and cherry-picked only the fee fix. Final check passes."
gh pr merge <PR-NUMBER> --repo <YOUR-FORK> --merge
git fetch origin
git switch -c submitted/final origin/result/final
py -3 "<LAUNCHER>\lab.py" check final --repo .
```

## 应发布与不发布的分支

普通 push 提交：`result/ex01`、`result/ex02`、`result/ex05`、`result/ex07`、`result/ex09`、`result/ex10`。

PR merge 提交：`feature/ex04` 合入 `result/ex04`；`feature/final` 合入 `result/final`。

03、06、08、11、12 是本地练习，不发布。
