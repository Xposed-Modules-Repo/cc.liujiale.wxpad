# 微信平板模式（极简版）

这是一个 Android Xposed 模块。它让微信把手机识别为 Lenovo TB-9707F 平板，并修改平板登录校验结果。此功能用于让同一微信号在手机和平板上同时在线。

本项目是「飞鱼」（`com.sky.ant.plus`）的独立重写版，与原版作者无关。它只保留平板登录功能。

| 项目 | 信息 |
|---|---|
| 模块包名 | `cc.liujiale.wxpad` |
| 桌面显示名 | 微信平板模式（极简版） |
| Xposed API | libxposed API 102（`io.github.libxposed.api`） |
| 作用域 | `com.tencent.mm`（声明在 APK 内，Vector 上需手动勾选） |
| 源码 | <https://github.com/Slacker-LLC/wx-pad> |
| 依赖 | 无第三方运行时依赖 |
| 许可 | Apache-2.0（含图标） |

> **修改微信客户端行为可能违反微信用户协议，也可能触发账号风控。使用前请自行评估风险。** 完整免责声明见[源码仓库](https://github.com/Slacker-LLC/wx-pad/blob/main/DISCLAIMER.md)。

## 下载与安装

在本仓库的 [Releases](https://github.com/Xposed-Modules-Repo/cc.liujiale.wxpad/releases) 页下载最新 APK。

1. 安装 APK。
2. 在支持 libxposed API 102 的框架里启用本模块，例如 Vector 或新版 LSPosed。模块列表会把它显示为 `API 102`。
3. 在框架里给本模块勾选微信 `com.tencent.mm`。模块的 APK 内已经声明了这个作用域（`META-INF/xposed/scope.list`），但 Vector 2.2 不会自动应用它，必须手动勾选。
4. 停用其他微信平板模块，避免多个模块同时 Hook 同一逻辑。
5. 强制停止微信，再重新打开。

作用域的改动在微信下次启动时生效。按 tag `wxpad` 查看框架日志即可确认是否生效：出现 `hooked [local]` 与 `hooked [cgi]` 两行，说明两个目标都已挂上。

## 自动适配与限制

模块扫描微信 APK 中的 DEX 字符串，查找目标方法，再用反射检查方法签名。它不依赖固定的微信类名，并会缓存扫描结果。

自动适配不代表支持所有版本。目前已在微信 8.0.79（versionCode 3200）上验证。微信更新后，如果相关字符串或方法签名改变，模块可能无法生效。可在框架日志中按 tag `wxpad` 查找 `kind N not hooked` 或 `scan found 0 target(s)`。

模块只对微信包名 `com.tencent.mm` 生效。微信仍会按原流程发送平板登录校验请求；本模块不单独发起网络请求。

## 构建

需要 JDK 17 和 Android SDK（build-tools 36.0.0、platforms/android-36）。源码与构建说明见 <https://github.com/Slacker-LLC/wx-pad>：

```bash
bash build.sh
```
