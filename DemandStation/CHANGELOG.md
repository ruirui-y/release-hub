# DemandStation 版本日志

VR 体验馆管理系统（Windows 客户端 + 服务端）。

格式：`## <版本> (<日期>)`，下面分「新增 / 修复 / 变更」三组，没有的组不写。

## 1.0.1 (2026-11-18)

修复：
- 安装包内的 PvP 游戏配置换成当前标准版：`EnableAI` 从 0 改回 1、`GunOffset` 补上 `0.0,-23,180`、`MapSize` 从 `15mx9m` 改为 `6mx8m`、`MaxAINum` 从 0 改为 6，并补齐 `ID` / `Nick` / `Team` / `HaveGun` / `TrueGearMac` 等 9 个字段
- PvP 四个地图（Science / Sport / SciFi / Sky）的尺寸列表补上 `9mx9m`，原来只有 6 项，界面选不到这个尺寸
- Pack 目录只保留 PvP，清掉代码从未读取的 `Config`、路径对不上一直没生效的 `MiNiWorld` Configs、以及当前不发布的 `Dragon`
- 安装包新增 `[InstallDelete]`，只删 `Pack\Config` / `Pack\MiNiWorld` / `Pack\Dragon` 三个废弃子目录，不碰 `{app}\Pack` 本身，避免把用户自己放进去的游戏本体一起删掉

变更：
- 只删 `Pack` 一个目录，`ClientInstall\Configs\` 下的设备缓存 `local_data.db`、账号配置 `app_settings.json`、语言皮肤设置全部保留

安装包：`DemandStation_Setup_1.0.1.exe`，50,355,052 字节，
sha256 `dc4d0c66d3e7904d902c97a0fcae5d1b896b0af1ba338ed1bc36220c0c4a2f59`

> 这个版本号此前曾用于内部验证更新链路，对应发行版与安装包已删除，此处为重新发布。

## 1.0.0 (2026-11-16)

首个正式版本。

新增：
- 设备管理：UDP 广播自动发现头显，实时显示在线状态、头显电量、手柄电量、屏幕亮灭；支持改昵称、分组、游玩时长、拖动排序
- 游戏控制：卡片式游戏大厅（PVP / MiniWorld），多台头显并行启动，倒计时同步，心跳与 UDP 双通道检测退出
- 离线优先：断网可继续操作，改动写本地 SQLite，网络恢复后自动补发积压操作，重连指数退避 1s 到 30s
- 会员与支付：账号登录、微信扫码充值、积分余额与消费流水、每次启动按设置扣积分
- 头显代理安装：ADB 一键安装 DemandService，装前自动卸载旧版，进度实时显示
- 软件更新：启动自动检查新版本，一键下载安装包，sha256 校验后静默安装，支持手动打开下载页兜底

变更：
- 更新分发从服务端分片下发改为第三方 Release 托管，服务端不再承担安装包带宽
- 删除 `Update` 独立工程（zip 解压替换器），安装改由 Inno Setup 覆盖完成

安装包：`DemandStation_Setup_1.0.0.exe`，50,357,847 字节，
sha256 `06e33737956d376a112bf0b1058fc6f6d86ccfe0137ab771a89a77b1825219d3`
