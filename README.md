# 弹珠速递 · VibeLab Android 工程

应用名：弹珠速递；包名：`com.bujiaban.marble.pico`；版本：0.1.0 / code 1；ARM64。

本次导出对应 Unity 源工程 [PicoRoom/marble-express](https://github.com/PicoRoom/marble-express) 提交 `333d1a6d27e53b4e6a23324a40f621eb43bdf97e`。供平台提交的公开仓库为 [PicoRoom/marble-express-VibeLab](https://github.com/PicoRoom/marble-express-VibeLab)。玩法变更须回到 Unity 源工程重新导出、预编译与验证，不替换不配套的运行库或 metadata。

## 本次修复与真机验证

首次运行的授权弹窗暂停不再让尚无任务的会话显示“原路线已保留”。定位阶段持续显示真实的平面识别状态，恢复作品提示放在底部，权限或识别错误可以正常显示。

2026-11-13，19 项 EditMode 测试与定位流程检查通过。使用同一源码构建的测试 APK 在 PICO B3110 上完成空间数据、手势与眼动授权，锁定 80 × 50 cm 区域，并在“弹跳抬升”任务中用 1 件道具成功配送（日志 `Result=Delivered`，约 1.20 秒）；完成状态和成绩已保存。此记录覆盖实际操作路径，不代表所有关卡均已真机复测。重新导出的成品与该测试 APK 的 global-metadata.dat 字节一致。

## 导出与构建

Unity 6000.4.6f1、Gradle 9.1.0、AGP 9.0.0、Java 17、NDK 27.2.12479018、Build Tools 36.0.0，min SDK 32、target/compile SDK 35。仅 launcher 是 Application module，其余均为 library；无 CMake 链路。

导出后在同级 staging 副本中执行 `:unityLibrary:buildIl2Cpp --rerun` 强制预编译，核对 ARM64 ELF、NDK r27c/build 12479018、APK 内全部 16 个原生库与配套 metadata 后才移除 C++ 中间源。保留原 buildIl2Cpp 任务体和 inputs/outputs，在末尾禁用任务，清空 jni.srcDirs；`.vibelab-prebuilt.json` 使用平台 schema 1 / il2cpp-prebuild-v6。

工具链路径保留 Unity 标准导出值，由平台处理；unityProjectPath 与 unity.projectPath 已清空。不提交 local.properties、签名密钥、APK、构建缓存、IL2CPP 中间源码、symbols 或 Unity 源工程；Unity 标准导出不含 gradlew，平台提供 wrapper 并统一签名。

配置 JAVA_HOME、ANDROID_HOME 与 ANDROID_SDK_ROOT 后，在工程根目录运行：

```sh
gradle assembleRelease --console=plain --no-daemon
```

本机已清空模块构建目录后验证根目录 assembleRelease 成功，buildIl2Cpp 为 SKIPPED。清理前后 APK 的全部 16 个原生库名称、ABI、SHA256 一致。签名、逐库 llvm-readelf 与 zipalign 检查通过；9 个供应商插件库的 LOAD 对齐为 4 KB，按平台默认 page_size_policy=warn 记录，不能据此声明整个 APK 兼容 16 KB。

## 保留的体积优化与许可证

IL2CPP 使用 OptimizeSize，managed stripping 保持 Minimal。Marble UI Sans 从原可变字体的默认字重生成，保留全部字符、字形与 Dynamic TMP 能力，既有 SDF atlas 不变；来源、归属和 OFL 见 `licenses/` 及源工程 tools/prepare_font.py。

仅 ARM64 的发布工程从 LoaderForUnitySDK.aar、platformloader.aar、tobservicelib-release.aar 移除 8 个非 ARM64 原生库条目，所有保留成员数据不变，清理前后 APK 原生库一致。重新导出会恢复供应商原始 AAR，应重新核查。其余供应商内容保留。详细来源和验证记录见 `export-info.json` 与 `unity-build.provenance.json`。
