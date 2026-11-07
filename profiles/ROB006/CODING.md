# Coding
- 语言 C（少量 C++），RISC-V 交叉编译（riscv64-anyka-linux-musl）；风格跟随所改文件/模块，别混入新风格。
- 注释用 Doxygen 风格：文件头 `/** … @file / @date / @author / @brief … */`（末尾含 Amicro 版权行）；函数用 `@brief / @param [in] 名 / @return`；枚举成员用行尾 `/**< 说明 */`。
- 命名遵循 `AM_` 前缀 + 蛇形：类型 `AM_S32`/`AM_CHAR`/`AM_BOOL`、枚举 `AM_XXX_E`，参照现有代码。
- 应用代码放 youpets-apps/<app>/src，遵守既有分层（main/conf/core/service/...）；内核/驱动/应用改动分开提交。
- 新任务用 git worktree：在当前仓库上一级建 `git worktree add ../<repo>-<desc> -b <feature|feat>/<desc>`（例：chassisd → chassisd-ipc-uart-0x2a-rpc @ feature/ipc-uart-0x2a-rpc、chassisd-pet-tease @ feat/pet-tease）；主仓库 checkout 保持 main。
- 每个 app 的 CFLAGS、工具链、依赖来自工作区根 rules.mk；新增源文件要纳入对应 Makefile 的 SRCDIRS/CSRCS。
- 复用 component/ 已有库（liblog/libcjson/libqueue/libtime/libimbus/libuart），不重复造轮子；新增依赖先评估。
- 版本串走 version.mk + git-commit.sh（{build: 时间 短commit}），不要手写。
