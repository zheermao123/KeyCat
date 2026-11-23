# KeyCat

> [!IMPORTANT]
> **本仓库是基于开源项目 [ElloboDK/KeyCat](https://github.com/ElloboDK/KeyCat) 创建的副本（复制版），并非官方仓库。**
> 原项目与全部版权归原作者 **[ElloboDK](https://github.com/ElloboDK)** 所有，本仓库仅作个人学习与备份用途。
> 请支持原项目：**https://github.com/ElloboDK/KeyCat** ⭐

一只趴在键盘上的桌面猫：你打字它伸爪按键，动鼠标它扭头看，摸头会闭眼，没人理会睡着，被烦到会用尾巴捶桌。支持 macOS 13+ 和 Windows 10+，除了从 GitHub 检查和下载更新，不联网。

![KeyCat](docs/screenshots/keycat.gif)

下载页：<https://ellobodk.github.io/KeyCat/>，会按访问者的系统给出对应的安装包，页面上的猫可以直接用键盘鼠标逗。

## 安装

**macOS**：在终端执行

```sh
curl -L -o /tmp/KeyCat-macOS.zip https://github.com/ElloboDK/KeyCat/releases/latest/download/KeyCat-macOS.zip \
  && unzip -qo /tmp/KeyCat-macOS.zip -d /Applications && open /Applications/KeyCat.app
```

首次启动需在「系统设置 → 隐私与安全性 → 输入监控」里允许 KeyCat，然后重启应用。它只读取哪个键被按下，不记录输入内容。手动从浏览器下载时，若提示「已损坏」，执行 `xattr -dr com.apple.quarantine /Applications/KeyCat.app`。装在「应用程序」文件夹里的 KeyCat 会自动更新：启动 15 秒后和之后每 6 小时检查一次，有新版就在后台下载，退出时替换，也可以在菜单里点「重启并更新」。1.0.2 及更早的版本没有这个功能，需要用上面的命令重装一次。

**Windows**：从 [Releases](https://github.com/ElloboDK/KeyCat/releases/latest) 下载 `KeyCat-Windows-Setup.exe` 运行。SmartScreen 拦截时点「更多信息 → 仍要运行」。安装包只有几 MB，猫由系统自带的 WebView2 绘制，极少数没装 WebView2 的 Win10 会在安装时自动下载。应用会自动更新，1.0.x 的旧版会自动升级过来并保留设置。

## 使用

猫住在菜单栏（Windows 是托盘）。右键猫或点图标打开菜单，可以换毛色和外观、调大小、设置多久睡着，在「调节反应…」里调整烦躁和好奇的灵敏度。按住猫身体可以拖动位置。直播或录屏时可以打开「鼠标穿透」，点击会直接落到猫下面的窗口，猫照常显示和反应，这时菜单只能从菜单栏或托盘图标打开。

Windows 上很多游戏以管理员身份运行，普通权限的 KeyCat 收不到发给它们的按键，猫就不动了。在菜单里打开「以管理员身份运行」，KeyCat 会弹出 UAC 授权并以管理员身份重启，之后每次手动启动都会弹 UAC。开机自启这时改用计划任务，登录时直接以管理员身份启动，不弹 UAC。关掉这个选项后，要等 KeyCat 下次启动才恢复成普通权限。独占全屏的游戏会盖住所有窗口，这种情况下猫本身就看不到，请把游戏改成无边框窗口模式。

## 构建

```sh
sh mac/build.sh       # → build/KeyCat.app（通用二进制，需 Xcode 命令行工具）
sh mac/package.sh     # → dist/KeyCat-macOS.zip + .sha256 + latest-mac.json

cd win && npm ci
npm start             # 本机调试
npm run dist          # → win/target/release/bundle/nsis/KeyCat_<版本>_x64-setup.exe（需 Rust 和 Node.js 22+，在 Windows 上执行）
```

mac 版用 `signing/` 里自动生成的本地证书签名，这样重新编译后「输入监控」授权仍然有效。mac 自动更新只接受同一张证书签名的新版，所以发布包必须在有这个目录的机器上打，证书丢了已安装的 mac 版也收不到更新。Windows 自动更新的签名私钥也放在这里（`tauri-updater.key` 和它的密码 `tauri-updater.password`），内容同时存在仓库的 `TAURI_SIGNING_PRIVATE_KEY`、`TAURI_SIGNING_PRIVATE_KEY_PASSWORD` 两个 Secret 里，公钥写在 `win/tauri.conf.json`。私钥丢了已安装的版本就收不到更新，要另外备份。不要提交这个目录。

## 发布

mac 和 Windows 共用一个 Release，两边产物都要上传，否则另一端的下载链接或自动更新会失效。

1. 把 `win/package.json` 的 `version` 和 `mac/Info.plist` 的 `CFBundleShortVersionString` 改成同一个版本，`CFBundleVersion` 加 1，合入 main。
2. 执行 `sh mac/package.sh`，推送 `vX.Y.Z` tag，再执行 `gh release create vX.Y.Z --generate-notes dist/KeyCat-macOS.zip dist/KeyCat-macOS.zip.sha256 dist/latest-mac.json`。`latest-mac.json` 是 mac 版的更新清单，mac 版靠 `CFBundleVersion` 判断新旧，所以每次发布都要加 1。
3. tag 触发 GitHub Actions，几分钟后会把 Windows 安装包、`latest.json`（新版的更新清单）和 `latest.yml`（让 1.0.x 旧版升级过来）补传到同一个 Release。

## 目录

```
mac/    macOS 外壳（Swift）：浮窗、菜单、键鼠监听、自动更新、构建/签名/打包脚本
win/    Windows 外壳（Tauri，Rust）：托盘、全局键鼠钩子、自动更新，页面由系统 WebView2 绘制
web/    两端共用的 three.js 场景：猫、键盘、动作，tuning.js 是可调参数的唯一来源
site/   下载页，由 .github/workflows/pages.yml 连同 web/ 一起部署到 GitHub Pages，每 3 小时刷新一次下载量
tools/  开发用渲染脚本（serve.py 起本地服务，readme_gif.py 生成 README 动图，其余脚本在内置浏览器里截图对比）
```

## 说明

这是一个从零实现的学习项目，形象和交互参考了一款付费桌面宠物应用，仅供个人使用和内部交流，不用于商业分发，也不包含对方的任何素材。three.js 为 MIT 许可（见 `web/THREE_LICENSE`），Tauri 为 MIT / Apache-2.0 双许可。

## 仓库来源（Attribution）

本仓库复制自开源项目：

- **原仓库**：<https://github.com/ElloboDK/KeyCat>
- **原作者**：[ElloboDK](https://github.com/ElloboDK)
- **复制时间**：2026-11-23（当时最新版本 v1.0.4，提交 `31d0316`）

所有代码、文档与设计的权利归原作者所有，本仓库的改动仅限本 README 中的仓库说明。如原作者提出要求，本仓库将配合删除或调整。

同步上游更新的方法：

```sh
git fetch upstream          # 拉取原仓库更新
git merge upstream/main     # 合并到本地
git push origin main        # 推送到本仓库
```

## 本仓库收录的其他开源项目源码

本仓库还以 git subtree 方式收录了以下开源项目的源码快照（均保留其原始 LICENSE 与版权声明，权利归各自原作者所有）：

| 子目录 | 项目 | 原仓库 | 协议 |
|--------|------|--------|------|
| `weasel/` | 小狼毫 Weasel（Rime 输入法 Windows 前端） | <https://github.com/rime/weasel> | GPL-3.0 |
| `rime-ice/` | 雾凇拼音 Rime Ice（拼音配置与词库） | <https://github.com/iDvel/rime-ice> | GPL-3.0 |

> 注：KeyCat 与这两个项目无任何关系，仅作个人学习与备份用途收录。这两个子目录的内容来自各自原仓库的快照，其更新不会随 KeyCat 自动进行。

同步更新它们的源码：

```sh
git fetch upstream-weasel && git subtree pull --prefix=weasel upstream-weasel master --squash
git fetch upstream-rime-ice && git subtree pull --prefix=rime-ice upstream-rime-ice main --squash
git push origin main
```
