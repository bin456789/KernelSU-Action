# KernelSU Action

使用 Github Actions 编译集成 [KernelSU](https://github.com/tiann/KernelSU) 的内核

并定时检测 KernelSU 新版本，有则编译新内核

## 使用

本仓库用于编译 **小米 8 (dipper)** 的 **PixelExperience** 内核

如需编译其他内核：

1. Fork 本仓库，修改 [配置文件](.github/workflows/main.yml) 的 `env` 部分

2. 运行编译：在 Actions 页面，点击左侧 `Check and Build`，再点击右侧 `Run workflow` 按钮

3. 若编译成功，Releases 页面可下载 `boot.zip`，在 Recovery 下刷入即可

## 参考配置

本例：小米 8 (dipper) PixelExperience 14

<https://github.com/PixelExperience-Devices/kernel_xiaomi_sdm845/tree/fourteen>

```yaml
env:
  kernel_repository: PixelExperience-Devices/kernel_xiaomi_sdm845
  kernel_branch: fourteen
  device: dipper
  clang_version: r487747c
  defconfig: |
    dipper_defconfig
  kernel_patch: |
    backport-path-umount.patch
  kernelsu_patch: |
```

## 参数

### defconfig

在机器内核仓库的 `arch/arm64/configs` 文件夹能找到机器的 `defconfig`

---

### clang_version

只能填写以下 AOSP Clang 版本号，可能需要尝试不同的版本号才能成功编译

| AOSP Clang | 基于 Clang | 最兼容的内核                                                                                                                                                                                                                    |
| ---------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| r365631c   | 9          | [4.4](https://android.googlesource.com/kernel/common/+/refs/heads/deprecated/android-4.4-p/build.config.common) [4.9](https://android.googlesource.com/kernel/common/+/refs/heads/deprecated/android-4.9-q/build.config.common) |
| r383902    | 11         | [4.14](https://android.googlesource.com/kernel/common/+/refs/heads/deprecated/android-4.14-stable/build.config.common)                                                                                                          |
| r416183b   | 12         | [4.19](https://android.googlesource.com/kernel/common/+/refs/heads/android-4.19-stable/build.config.common) [5.4](https://android.googlesource.com/kernel/common/+/refs/heads/android12-5.4/build.config.common)                |
| r450784e   | 14         | [5.10](https://android.googlesource.com/kernel/common/+/refs/heads/android13-5.10/build.config.constants)                                                                                                                       |
| r487747c   | 17         | [5.15](https://android.googlesource.com/kernel/common/+/refs/heads/android14-5.15/build.config.constants) [6.1](https://android.googlesource.com/kernel/common/+/refs/heads/android14-6.1/build.config.constants)               |

若想使用官方 Clang 编译，则改成填写官方 Clang 版本号，例如 17

---

### kernel_patch 和 kernelsu_patch

分别是应用到 `内核` 和 `KernelSU` 的补丁文件列表，补丁文件需放在 `patches` 文件夹

补丁详细说明:

<https://github.com/tiann/KernelSU/discussions/955>

<https://kernelsu.org/zh_CN/guide/how-to-integrate-for-non-gki.html#how-to-backport-path-umount>

如果 KernelSU 功能正常，**不要** 添加补丁
