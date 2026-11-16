# release-hub

个人软件发版仓库：各产品的安装包、更新清单与版本日志统一存放。
**不放源码**，只放二进制包和 `latest.json`。

## 目录结构

```
release-hub/
├── README.md
├── DemandStation/           VR 体验馆管理系统（Windows 客户端 + 服务端）
│   ├── latest.json          客户端启动时读取的更新清单
│   └── CHANGELOG.md         版本日志
└── MiNiWorld/               MiNiWorld 发布包
    ├── latest.json
    └── CHANGELOG.md
```

一个产品一个子目录、一个 `latest.json`。新增产品直接加目录，不用改仓库结构。

## 客户端读取的地址

```
清单   https://gitee.com/bai-maxifeng/release-hub/raw/master/<产品>/latest.json
兜底   https://raw.githubusercontent.com/ruirui-y/release-hub/master/<产品>/latest.json
附件   https://gitee.com/bai-maxifeng/release-hub/releases/download/<产品>-v<版本>/<安装包文件名>
```

国内优先 Gitee（GitHub 直连经常不通），GitHub 只作兜底。

> Gitee 侧已核实：`bai-maxifeng/release-hub`（2026-11-16 用 `git ls-remote` 确认存在，HEAD 与 GitHub 同步）。
> 页面上显示的 `ruirui-y/release-hub` 是「导入来源」标识，不是仓库地址。

## Release tag 规范

共用仓库的 tag 命名空间是共享的，**必须带产品前缀**，否则 `v1.0.0` 会重名。

```
DemandStation-v1.0.0
DemandStation-v1.0.1
MiNiWorld-v1.0.0
```

## latest.json 字段

```json
{
    "latest": "1.0.0",
    "minimum": "1.0.0",
    "url": "https://github.com/ruirui-y/release-hub/releases",
    "url_cn": "https://gitee.com/bai-maxifeng/release-hub/releases",
    "notes": "一句话说明这版改了什么",
    "published_at": "2026-11-16",
    "exe": "GitHub 安装包直链",
    "exe_cn": "Gitee 安装包直链",
    "size": 48084732,
    "sha256": "64 位小写十六进制"
}
```

| 字段 | 作用 | 缺了会怎样 |
|---|---|---|
| `latest` | 最新版本号，和本地版本比 | 客户端认为没更新 |
| `minimum` | 最低可用版本，低于它就强制更新 | 只软提示，不硬拦 |
| `url` | 国外下载页（GitHub） | 「去下载」按钮打不开 |
| `url_cn` | 国内下载页（Gitee） | 国内用户退回用 `url` |
| `exe` | 国外安装包直链（GitHub） | 国外用户退用 `exe_cn` |
| `exe_cn` | 国内安装包直链（Gitee） | 国内用户退回用 `exe` |
| `size` | 字节数，校验完整性 | 跳过大小检查 |
| `sha256` | 安装包指纹，64 位十六进制 | **不能自动更新**，按钮退回「去下载」 |
| `notes` | 悬停提示里的一句话 | 不显示说明 |

## 国内国外怎么选（客户端不用探测网络）

客户端先拉这份 JSON，**记下它是从哪个源拿到的**，然后：

```
从 Gitee 拿到  → exe_cn + url_cn   （说明国内链路通）
从 GitHub 拿到 → exe   + url       （国内不通，只能走海外）
```

不 ping、不测速、不判国家。来源本身就是答案。

`size` 和 `sha256` **只有一份**，前提是两边放的是**同一个安装包**（字节完全一致）。
将来如果要出「国内版 / 国外版」功能不同的包，`sha256` 就得拆成两个键，现在不预留。

## 发版流程

```
① 本地打包出安装包
② 建 Release，tag 用 <产品>-v<版本>，把安装包传上去
③ 算 size 和 sha256，写进该产品的 latest.json
④ 改 latest 和 notes，提交推送
```

第 ③ 步手动容易错，等 DemandStation 客户端接好了再写一个发版脚本自动填。

## 两个坑

1. **JSON 不能带 BOM**。PowerShell 的 `Set-Content` / `Out-File` 默认写 UTF-8 with BOM，
   Qt 的 `QJsonDocument::fromJson` 不跳过 BOM，会直接判非法——
   表现是"检查更新永远失败"，日志里还看不出原因。
   正确写法：

   ```powershell
   [System.IO.File]::WriteAllText($path, $json, (New-Object System.Text.UTF8Encoding($false)))
   ```

2. **Gitee raw 有缓存**。客户端请求带 `?v=yyyyMMddHH` 这种按小时变的参数，
   否则新版本可能要等一整天才能被读到。

## 容量

Gitee 免费仓库对单文件和总容量都有限制，安装包 46MB 起步（带 Qt 运行库的更大）。
**首个包上传后要实测一次**，超限就得改走 GitHub Release 或网盘直链。
