# Testing
- 交叉编译通过后上板验证：youpets-apps/skills/youpets-board-test/scripts/board.sh（ssh root@172.16.20.75，免密）。
- /app 是只读 squashfs，测试版部署到 /mnt/app（SD 卡）或 /data/app（DEST 可强制指定），不要直接写 /app。
- mediad 冒烟：board.sh smoke，必须 all passed；configd 短测：board.sh configd（退出码 2 是 -t 到点，正常）。
- 部署会停服务并重启，上板前先 ps 确认板子没人占用；记下脚本打印的回退命令（rollback pre-...）。
- 改了方法名/错误码时，同步更新 smoke 脚本里 check 的期望值。
