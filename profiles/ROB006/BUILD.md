# Build
- 环境：Docker CE + GNU Make ≥ 4.3；工具链在 /opt/riscv64-anyka-linux-musl/bin（先 export PATH 再加进 PATH）。
- 看配置：make vars；目标改动在 config.mk（芯片/传感器/Flash/WiFi/DTB）。
- 单 app：make -C youpets-apps/<app>，产物 build/anyka/<app>（已 strip + .debug）。
- 整包 apps：youpets-apps/pack.sh（构建 appd/mediad/configd/chassisd 并汇总到 out/）。
- 全量镜像：make all（内核/uboot/opensbi/驱动 + buildroot rootfs + 打包 ROB006 烧录件），或 ./auto_build.sh。
- 生成 compile_commands.json：make compiledb（依赖 bear）。
- 容器内跑构建/调试：make shell。
