# Workflow
从建 worktree 到 MR 合入 dev 的标准流程（以 chassisd 为例）：

1. 确认主仓库状态：`git status --short`、`git branch --show-current`、`git worktree list`；先识别已有未提交/未跟踪文件，避免误带入功能提交。
2. 从 dev 建 worktree（放主仓库同级目录）：`git worktree add -b <feature|feat>/<desc> ../<repo>-<desc> dev`；主目录保持 dev，开发目录独立。
3. 在 worktree 中开发 + 测试，跑干净构建：`make -C test/<target> clean all`，再 `make clean` 与 `make CC=/opt/riscv64-anyka-linux-musl/bin/riscv64-anyka-linux-musl-gcc OBJCOPY=... STRIP=...`；`git diff --check`。
4. 上板验证：部署测试版到目标板（IP 以实际为准），验证 RPC 注册 / 协议帧 / 运动 / 看门狗 / release-reacquire 等；结束后恢复板上原程序并清理临时文件。
5. 在功能分支提交：只暂存本次功能相关文件，`git diff --cached --check`、`git diff --cached --stat`，`git commit -m "feat: ..."`。
6. push 功能分支并提 MR：`git push -u origin <feature>`，在 Web 提 MR（目标分支 dev）；不直接合回 main。
7. 最终检查：`git status --short`、`git log --oneline --decorate -5`；确认只推了本次功能分支、MR 目标为 dev、未跟踪文件未被提交/删除；等评审合入 dev（不直接 push main）。
8. 清理（不再需要时）：`git worktree remove ../<repo>-<desc>`、`git branch -d <feature>`。

核心原则：同级目录建 worktree；开发提交全部在功能分支；push 后提 MR 合入 dev，不直接合 main；最后才清理 worktree。
