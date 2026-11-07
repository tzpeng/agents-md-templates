# Coding
- 语言 C（少量 C++），RISC-V 交叉编译（riscv64-anyka-linux-musl）；风格跟随所改文件/模块，别混入新风格。
- 应用代码放 youpets-apps/<app>/src，遵守既有分层（main/conf/core/service/...）；内核/驱动/应用改动分开提交。
- 每个 app 的 CFLAGS、工具链、依赖来自工作区根 rules.mk；新增源文件要纳入对应 Makefile 的 SRCDIRS/CSRCS。
- 复用 component/ 已有库（liblog/libcjson/libqueue/libtime/libimbus/libuart），不重复造轮子；新增依赖先评估。
- 版本串走 version.mk + git-commit.sh（{build: 时间 短commit}），不要手写。
- 保留 Amicro 版权头：Copyright (C) 2014-2026, Amicro Co.Ltd. All rights reserved.
