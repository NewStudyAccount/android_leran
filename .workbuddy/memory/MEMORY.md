# android_leran 项目长期记忆

## 构建环境约定（2026-10-23）
- Gradle 全部网络流量走本机 Clash（127.0.0.1:7890），配置位于 `C:/Users/AI/.gradle/gradle.properties`（systemProp.http/https.proxyHost/Port）
- **若 Clash 端口变更或关闭，需同步修改该文件，否则 Gradle 构建/同步会连接失败**
- Gradle 9.4.1 发行版已预置于本地 wrapper 缓存（SHA256 校验过），无需再下载
- 国内镜像（腾讯/华为）截至 2026-10-23 尚未同步 Gradle 9.4.1；阿里云 maven 镜像（maven.aliyun.com/repository/google）有 AGP 9.2.1，可作依赖下载备用方案
- 命令行验证用 `sh ./gradlew`（git-bash），本机 PATH 有 JDK 21.0.7；Android SDK 在 `D:\develop\Android\sdk`

## 学习者画像与进度（2026-10-23）
- 用户是 Android 零基础初学者：Kotlin 语法（继承、lambda、嵌套调用）需要"逐行拆解 + 结构图"式讲解，纯文字效果差
- 学习进度：阶段 1（Kotlin 基础）笔记已完成；正在做学习方案"第一步 Action"（读懂 MainActivity.kt 模板结构，已用文件解剖图 + setContent 嵌套图讲完）
- 下一步：引导用户把 MainActivity 第 24 行 `name = "Android"` 改成自己名字并运行，然后开始阶段 1 的 onCreate 练手练习（Log 打印变量/集合/data class）
