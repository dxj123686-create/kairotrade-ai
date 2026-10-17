# Kairo Crypto (OKX) — Android starter project

这是参考用户提供的 KairoTrend 截图独立实现的安卓端原型，使用 Android WebView 包装本地 HTML 界面。

## 已实现
- OKX 公共行情 REST API：ticker + K线 candles，无需 API key
- BTC/USDT、ETH/USDT、SOL/USDT，以及 BTC/ETH 永续参考行情选项
- 1m、5m、15m、1H、4H、1D K线周期
- RSI(14)、短期/中期均线趋势、近20根K线支撑阻力估算
- 仓位/风险/止盈止损/盈亏比计算器
- 本机交易计划记录（localStorage）
- 深色移动端界面

## 当前限制
- 这是 MVP 原型，不是经过审计的交易系统。
- 图表优先使用 CDN 的 Lightweight Charts；首次运行需网络连接。如 CDN 不可用，内置简易 canvas K线作为回退。
- OKX API 可能受地区、网络或服务状态影响；页面会显示连接状态。
- “支撑/阻力”和趋势判断为简单规则估算，不代表 OKX 官方信号。
- 不支持真实下单、不要求交易 API 密钥。
- 交易记录目前仅保存在本机 WebView 存储中，清除应用数据可能会丢失。
- APK 尚未编译。当前交付的是可继续构建的 Android Studio 项目源码。

## 构建 APK
1. 在电脑安装 Android Studio（建议稳定版）及 Android SDK 35。
2. 用 Android Studio 打开此项目根目录。
3. 等待 Gradle Sync 完成。
4. 选择 Build > Build Bundle(s) / APK(s) > Build APK(s)。
5. APK 通常生成于 `app/build/outputs/apk/debug/app-debug.apk`。

也可以在安装 JDK 17 和 Android SDK 的环境中运行：
`gradle assembleDebug`

## OKX 数据接口
- `GET https://www.okx.com/api/v5/market/ticker?instId=BTC-USDT`
- `GET https://www.okx.com/api/v5/market/candles?instId=BTC-USDT&bar=15m&limit=100`

公共行情接口不需要 API key。不要把交易 API secret 写进客户端应用。


## 手机-only：使用 GitHub Actions 云端编译（无需在手机安装 Android Studio）
1. 在浏览器打开 GitHub 并登录/注册账号。
2. 创建一个新的 Public 或 Private repository（仓库）。
3. 在仓库页面选择 Add file → Upload files，把 ZIP 解压后所有项目文件上传；注意要包含隐藏目录 `.github/workflows/build-apk.yml`。如果手机网页不方便上传隐藏目录，可先上传源代码，再用 Add file → Create new file 创建 `.github/workflows/build-apk.yml` 并粘贴该文件内容。
4. 提交到 `main` 分支后，打开仓库的 Actions 标签页，选择 “Build Android APK”，点击 Run workflow（或等待 push 自动触发）。
5. 等待任务完成，打开该次运行页面，在 Artifacts 区域下载 `Kairo-Crypto-debug-APK`，解压后得到 `app-debug.apk`。
6. 在手机文件管理器中点击 APK 安装。Android 可能要求允许当前浏览器/文件管理器“安装未知应用”；仅对你信任的 APK 开启，并在安装后关闭该权限。

说明：云端构建会将代码交给 GitHub Actions 运行。不要把 OKX 交易密钥、密码或其他秘密提交到仓库。本项目只调用 OKX 公共行情，不需要交易密钥。
