# Workflow
从建 worktree 到合回 main 的标准流程（以 chassisd 为例）：

1. 确认主仓库状态：`git status --short`、`git branch --show-current`、`git worktree list`；先识别已有未提交/未跟踪文件，避免误带入功能提交。
2. 从 main 建 worktree（放主仓库同级目录）：`git worktree add -b <feature|feat>/<desc> ../<repo>-<desc> main`；主目录保持 main，开发目录独立。
3. 在 worktree 中开发 + 测试，跑干净构建：`make -C test/<target> clean all`，再 `make clean` 与 `make CC=/opt/riscv64-anyka-linux-musl/bin/riscv64-anyka-linux-musl-gcc OBJCOPY=... STRIP=...`；`git diff --check`。
4. 上板验证：部署测试版到目标板（IP 以实际为准），验证 RPC 注册 / 协议帧 / 运动 / 看门狗 / release-reacquire 等；结束后恢复板上原程序并清理临时文件。
5. 在功能分支提交：只暂存本次功能相关文件，`git diff --cached --check`、`git diff --cached --stat`，`git commit -m "feat: ..."`。
6. 合回 main，不生成 merge commit：主仓库 `git merge --ff-only <feature>`；若误产生 merge commit 且功能树一致，把 main 指回功能提交。
7. 最终检查：`git status --short`、`git log --oneline --decorate -5`、`git rev-list --left-right --count origin/main...main`；确认无 merge commit、main 比 origin/main 超前 1 个提交、未跟踪文件未被提交/删除；确认后 `git push origin main`。
8. 清理（不再需要时）：`git worktree remove ../<repo>-<desc>`、`git branch -d <feature>`。

核心原则：同级目录建 worktree；开发提交全部在功能分支；合回优先 `git merge --ff-only`；最后才清理 worktree。
