# 弹珠速递 · VibeLab Android 工程

应用名：弹珠速递；包名：`com.bujiaban.marble.pico`；版本：0.1.0 / code 1。

Unity 源工程：[PicoRoom/marble-express](https://github.com/PicoRoom/marble-express)。此次导出基于 `997860553ceaf3c8adaec55ad30c8bcfd624211d`，加上本机尚未提交的代码体积和字体优化；玩法、音频、贴图分辨率和现有 SDF atlas 保持。

## 压缩与复现

Unity 6000.4.6f1 的 AndroidBuild 和 SceneBootstrap 启用 IL2CPP `OptimizeSize`，保持 Minimal managed stripping。主运行库从 75,037,568 字节降到 36,746,264 字节。该选项优化代码体积，真机运行性能尚未复测。

源工程的 `tools/prepare_font.py` 将原可变字体实例化为默认字重的 Marble UI Sans，保留全部 30,892 字符、31,036 字形、排版指标和 Dynamic TMP 功能。字体从 17,773,360 字节降到 10,594,448 字节。许可证和归属见 `licenses/MarbleUISans-OFL.txt`、`licenses/FONT-NOTICE.txt`。

在仅支持 ARM64 的前提下，清理 `LoaderForUnitySDK.aar`、`platformloader.aar`、`tobservicelib-release.aar` 中 8 个非 ARM64 原生库条目，合计减少 1,359,951 字节。全部保留成员的数据与 ZIP 元信息保持一致；清理后重新构建，APK 的全部 362 个文件内容及 16 个原生库均与清理前一致，签名和 zipalign 验证通过。重新 Unity 导出会恢复供应商原始 AAR，需要再次核查这一步。

修改源码后须重新 Unity 导出、预编译 IL2CPP，并按平台 il2cpp-prebuild-v6 流程清理和验证。此仓库保留匹配的 ARM64 运行库与 metadata，禁用 buildIl2Cpp，移除 C++ 中间源；不能随意替换运行库或 metadata。

## 独立构建

Gradle 9.1.0、AGP 9.0.0、NDK 27.2.12479018、Build Tools 36.0.0、Java 17、ARM64、min SDK 32、target/compile SDK 35。配置 Android SDK 环境后运行 `gradle :launcher:assembleRelease`。Unity 标准导出不包含 gradlew。平台提供构建工具链，并统一处理签名。

本地验证：18 项 EditMode 测试通过；974 个 Unity FontEngine 样本像素和指标一致；清除 C++ 中间源及构建缓存后独立 Release 构建成功；清理前后 APK 原生库 SHA256 一致，打包 metadata 与工程一致，ZIP 和 zipalign 检查通过。未进行云构建或 PICO 真机复测。
