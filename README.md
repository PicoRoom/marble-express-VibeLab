# 弹珠速递 · VibeLab Android 工程

- 应用名：弹珠速递
- Android 包名：`com.bujiaban.marble.pico`
- 版本：0.1.0 / code 1
- 图标：沿用已确认的薄荷绿滑道、橙色收件箱和弹珠三层图标，含三层 SDF。
- Unity 源工程：[PicoRoom/marble-express](https://github.com/PicoRoom/marble-express/tree/997860553ceaf3c8adaec55ad30c8bcfd624211d)
- 对应源工程 commit：`997860553ceaf3c8adaec55ad30c8bcfd624211d`

本次导出包含整套挑战等比适配选区、六种自由挑战、漏斗与引导修正、固定空间字幕与按钮、退出入口和音效。这里是按 `il2cpp-prebuild-v6` 处理后的独立 Android Gradle 工程；开发和修改玩法请使用源工程仓库。

## 工具链与构建

Unity 6000.4.6f1 导出，Gradle 9.1.0、AGP 9.0.0、NDK 27.2.12479018、Build Tools 36.0.0、Java 17 语言级别、ARM64、min SDK 32、target/compile SDK 35。SDK/JDK/NDK 路径由构建环境按平台流程提供。Unity 6 标准导出不包含 gradlew；使用对应 Gradle 安装运行 `gradle :launcher:assembleRelease`。

`libil2cpp.so` 已由本次导出强制重新编译，C++ 源码及本地构建缓存已移除，`buildIl2Cpp` 任务保留并禁用；`.vibelab-prebuilt.json` 记录 metadata 和全部随项目提交的原生库哈希。保留 Unity 模板的 debug 签名配置，不含私有签名文件。

## 本机验证 · 2026-11-10

- 原始导出预编译及 Release 构建成功。
- 删除 C++ 源码和构建缓存后再次独立构建成功，`buildIl2Cpp SKIPPED`。
- 清理前后 APK 内所有原生库的名称、ABI、SHA256 一致。
- APK 包名、默认及中文应用名、三层图标、三层 SDF、签名、ZIP 完整性和 zipalign 校验通过。
- 验证 APK SHA256：`ed93e23d734e263eb484a40c3521c552a11235f99b6b331799cb32d5b90fdc8b`。

APK 和缓存不提交 Git。尚未验证 VibeLab 云构建或新包名在实体头显上的运行；新包名与旧包独立安装、独立保存数据。
