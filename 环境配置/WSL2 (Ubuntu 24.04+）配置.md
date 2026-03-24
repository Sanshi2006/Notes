① 如果有 `NVIDIA` 独显，先下载驱动程序
②`powershell` 中执行  `wsl --install -d Ubuntu-24.04`  
③ 测试： `wsl -l -v`
④ 把 WSL 迁移到 D 盘：
导出压缩包：`wsl --export Ubuntu-24.04 D:\WSL_Export\ubuntu_backup.tar`
注销原来的：`wsl --unregister Ubuntu-24.04`
重新导入 D 盘路径：`wsl --import Ubuntu-24.04 D:\WSL_Data\Ubuntu2404 D:\WSL_Export\ubuntu_backup.tar`
执行：`wsl --manage Ubuntu-24.04 --set-default-user sanshiya`
⑤ 在 `powershell` $\rightarrow$  设置 $\rightarrow$ `Ubuntu-24.04` 把命令行改为 `wsl.exe -d Ubuntu-24.04`
⑥ 下载 docker  重启后改一下文件路径 不要存在 C 盘
