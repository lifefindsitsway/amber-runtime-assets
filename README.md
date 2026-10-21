# Amber Runtime Assets

[Amber](https://github.com/lifefindsitsway/Amber) 开发、测试与打包 Windows 客户端所用的第三方运行时构件。本仓库只保存说明与每个版本的清单；构件全部通过 [Releases](https://github.com/lifefindsitsway/amber-runtime-assets/releases) 分发。

## 当前版本

[`runtime-assets-2026.10.21`](https://github.com/lifefindsitsway/amber-runtime-assets/releases/tag/runtime-assets-2026.10.21)

| 组件 | 版本 | 平台 | 许可证 |
|---|---|---|---|
| Node.js | 24.21.0 | Windows x64（随客户端打包） | MIT |
| yt-dlp | 2026.08.19 | Linux x64、Windows x64 | Unlicense（内嵌组件见许可证包） |
| Deno | 2.9.7 | Linux x64、Windows x64 | MIT |
| FFmpeg（BtbN GPL 静态构建） | n9.0.2-3-ga5923073bf | Linux x64、Windows x64 | GPL-3.0-or-later（`ffmpeg -L`） |
| Noto Sans SC | Sans2.004 | 通用 | OFL-1.1 |

第三方二进制与字体是上游发布的原样文件；FFmpeg 源码归档是对应 commit 的重新打包；清单、声明、许可证包与完整对应源码包由本项目整理生成。每个文件的上游地址、版本、大小与 SHA-256 见本仓库 [`runtime-assets-2026.10.21/manifest.json`](runtime-assets-2026.10.21/manifest.json)。

## 下载与校验

```sh
mkdir runtime-assets && cd runtime-assets
gh release download runtime-assets-2026.10.21 --repo lifefindsitsway/amber-runtime-assets
sha256sum SHA256SUMS   # 应为 44de90ff887b9be736a0111a6a7f7c996939e3801dd383264855ce89cbf01214
sha256sum -c SHA256SUMS
```

不登录 GitHub 也可以直接下载单个文件：

```text
https://github.com/lifefindsitsway/amber-runtime-assets/releases/download/runtime-assets-2026.10.21/<文件名>
```

Release 发布后不可变。`gh release verify runtime-assets-2026.10.21 --repo lifefindsitsway/amber-runtime-assets` 校验 GitHub 为该 Release 签发的证明，`gh release verify-asset runtime-assets-2026.10.21 <文件> --repo lifefindsitsway/amber-runtime-assets` 校验单个文件。它们只证明文件与发布时一致；文件与上游是否一致，以 `manifest.json` 中每个文件的上游校验记录（`upstreamChecksumRef`）为准。

在 Amber 仓库中使用下载好的目录：

```sh
pnpm setup:tools --assets runtime-assets
pnpm pack:sidecar --assets runtime-assets
```

## 许可证与对应源码

许可证文本：

- `runtime-licenses.zip`：运行时组件（Node.js、yt-dlp、Deno、FFmpeg、Noto Sans SC）的许可证与声明原文，含 FFmpeg 静态链接的 Rust 标准库的许可证。
- `third-party-licenses.tar.xz`：Windows x64 FFmpeg 构建的全部组件与运行时组件的许可证。
- `third-party-licenses-linux64.tar.xz`：Linux x64 FFmpeg 构建额外使用的组件的许可证，以及 Implib.so（其 MIT 许可的桩代码模板链接进 Linux x64 构建）和两个构建静态链接的 Rust 标准库的许可证。

索引见 Release 中的 `THIRD_PARTY_NOTICES.md`。

FFmpeg 的完整对应源码（Complete Corresponding Source）在同一 Release 中：

- `ffmpeg-ccs.tar.xz`：Windows x64 构建全部组件的源码、BtbN 构建脚本（含各平台工具链镜像定义与补丁）以及构建配置。
- `ffmpeg-ccs-linux64.tar.xz`：Linux x64 构建额外使用的 21 个组件的源码与构建配置，以及生成动态加载桩代码的 Implib.so 源码快照，须与 `ffmpeg-ccs.tar.xz` 合用；其中 `COVERAGE.tsv` 列出 Linux 构建每个阶段的源码所在的包与 commit。

此外，`ffmpeg-source-a5923073bf.tar.xz` 是 FFmpeg 本体源码，`ffmpeg-build-info-3e6685ed.tar.xz` 是构建脚本快照与两个二进制的构建配置输出，`ffmpeg-dependencies.json` 列出每个构建依赖的仓库、固定 commit 与各平台启用情况。

## 版本规则

- tag 形如 `runtime-assets-<YYYY.MM.DD>`，日期为冻结日期；同一天再次冻结时加 `.2`、`.3`……后缀。
- 已发布的版本不修改、不删除。任何构件或元数据变化都发布新 tag，并在本仓库新增对应目录。

## 反馈

请在本仓库提交 [Issue](https://github.com/lifefindsitsway/amber-runtime-assets/issues)。
