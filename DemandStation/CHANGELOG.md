# DemandStation 版本日志

VR 体验馆管理系统（Windows 客户端 + 服务端）。

格式：`## <版本> (<日期>)`，下面分「新增 / 修复 / 变更」三组，没有的组不写。

## 1.0.1 (2026-11-16)

新增：
- 客户端自更新改走静态 `latest.json`，安装包直接从 Gitee / GitHub 的 Release 下载，不再经业务服务端分片下发
- 国内优先 Gitee、海外走 GitHub，按这份 JSON 从哪个源拿到自动选链
- sha256 校验通过后静默启动安装程序

修复：
- 版本号补满三段 `x.y.z`，与 `latest.json` 对齐

安装包：`DemandStation_Setup_1.0.1.exe`，50,400,904 字节，
sha256 `e7c4478d6f393fbc27dd8294d8c1becae722c3f4feec11bebc5abc99b46013e0`

## 1.0.0 (2026-11-16)

首个对外版本。
