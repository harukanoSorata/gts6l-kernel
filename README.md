# gts6l-kernel — Samsung Galaxy Tab S6 Wi-Fi (SM-T860) 主线内核构建

基于 [Bentlybro/gts6l-mainline-linux](https://github.com/Bentlybro/gts6l-mainline-linux) 的
公开内核侧文件（defconfig / 设备树 / 补丁 / 自研驱动），用 GitHub Actions 云端
交叉编译出 Tab S6 Wi-Fi 可用的主线 Linux 内核。

## 产物

| 文件 | 用途 |
|---|---|
| `Image` | arm64 内核（EFI-stub 可直接由 systemd-boot/U-Boot 加载） |
| `gts6.dtb` | 设备树（ANA38401 面板、fts1ba90a 触摸、SM5705 电量计等） |
| `modules.tar.gz` | `/lib/modules` 内核模块（解到 rootfs 的 /lib/modules/） |
| `kernel.config` | 编译用配置 |

## 手动触发

Actions 页面 → Build gts6lwifi kernel → Run workflow。

## 部署到平板（概述）

1. `boot` 分区刷入 Project Aloha UEFI（DualBootKernelPatcher 方式，见 Bentlybro 仓库 docs/BOOT_METHOD.md）
2. `cache`（sda27）格式化为 FAT，放入 systemd-boot + `Image` + `gts6.dtb`
3. `userdata`（sda30）写入 aarch64 rootfs（ext4）
4. 启动：ABL → UEFI → systemd-boot → Linux

## 来源与致谢

全部内核侧文件来自 Bentlybro 的开源项目，遵循其仓库许可；本项目仅添加
GitHub Actions 构建工作流。
