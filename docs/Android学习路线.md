# Android 开发学习路线

> 从零到独立开发上架 · 共 5 个阶段 · 约 12 周
>
> 当前项目 `android_learn` 已是 Jetpack Compose 空白模板（Kotlin + Compose + Material3），
> 是官方推荐的新技术栈起点。

---

## 学习路线总览

```
阶段1 Kotlin语言基础  →  阶段2 UI基础Compose  →  阶段3 状态与导航
        ↑                                              ↓
阶段5 工程化与发布    ←    阶段4 数据层
```

| 阶段 | 主题 | 时长 | 核心技术 |
|------|------|------|----------|
| 1 | Kotlin 语言基础 | 2 周 | 语法、空安全、协程 |
| 2 | UI 基础 Compose | 2 周 | 组件、布局、Modifier |
| 3 | 状态与导航 | 2 周 | State、ViewModel、Navigation |
| 4 | 数据层 | 3 周 | Room、Retrofit、Repository |
| 5 | 工程化与发布 | 3 周 | Hilt、测试、上架 |

---

## 阶段 1 · Kotlin 语言基础（约 2 周）

Android 的开发语言是 Kotlin（不是 Java）。这是地基,必须先打牢。

### 必学知识点

- **变量与类型**：`val` 不可变 / `var` 可变、空安全（`?` 和 `!!`）
- **函数**：默认参数、命名参数、Lambda 表达式
- **面向对象**：类与继承、`data class`、`object` 单例、`companion object`
- **集合**：`List`、`Map`、`Set` 与集合操作符（`map`、`filter`、`forEach`）
- **协程基础**：`suspend`、`launch`、`Dispatchers` — 异步编程必备

### 练习方式

打开 `app/src/main/java/com/example/android_learn/MainActivity.kt`，
先别碰 UI，在 `onCreate` 里写几行 Kotlin 代码：声明变量、遍历集合、
定义一个 data class，用 `Log` 打印出来看效果。

### 学习资源

- Google 官方 [Kotlin 语言基础 Codelab](https://developer.android.com/kotlin/learn)
- Kotlin 官网 [Koans 交互练习](https://play.kotlinlang.org/koans)

---

## 阶段 2 · UI 基础 Jetpack Compose（约 2 周）

现在的 Android 官方 UI 框架是 Compose（本项目已在用），不再用旧 XML 布局。
声明式 UI，思路类似 Flutter / SwiftUI。

### 必学知识点

- **基础组件**：`@Composable` 函数、`Text` / `Button` / `Image`
- **布局**：`Column`（纵向）、`Row`（横向）、`Box`（层叠）、`Spacer`
- **Modifier**：调整大小、间距、背景、点击 — Compose 的核心概念
- **列表**：`LazyColumn` / `LazyRow`（替代旧 RecyclerView）
- **主题**：Material3 颜色、字体、形状

### 练习方式

把 `Greeting("Android")` 改成个人名片界面 — 头像 + 姓名 + 简介，纵向排列。
再改成能滚动的联系人列表。

---

## 阶段 3 · 状态与导航（约 2 周）

让 UI 响应用户点击、输入、切换页面。

### 必学知识点

- **状态**：`remember` / `mutableStateOf`
- **状态提升**（state hoisting）：单向数据流设计模式
- **副作用**：`LaunchedEffect` / `DisposableEffect`
- **导航**：Navigation Compose 多页面跳转与传参
- **ViewModel**：配置变更（如旋转屏幕）时保留数据

### 练习方式

做一个计数器（点按钮 +1），再做一个待办清单 App —
输入框 + 添加按钮 + 列表，能增删条目。

---

## 阶段 4 · 数据层（约 3 周）

让 App 能存数据、联网。

### 必学知识点

- **架构**：`ViewModel` + `StateFlow` 把数据逻辑从 UI 剥离
- **本地存储**：Room 数据库（SQLite 封装）
- **网络请求**：Retrofit / OkHttp 发起 HTTP 请求
- **图片加载**：Coil 加载网络图片
- **Repository 模式**：统一本地 + 远程数据来源

### 练习方式

做一个"一言"App — 从公开 API 拉一句话显示；
再用 Room 把喜欢的话存到本地，离线也能看。

---

## 阶段 5 · 工程化与发布（约 3 周）

### 必学知识点

- **依赖注入**：Hilt 框架
- **测试**：JUnit 单元测试与 UI 测试
- **架构模式**：MVVM / MVI
- **发布流程**：签名打包、上架 Google Play 或国内应用市场

### 练习方式

用 Hilt 重构前面的 App，写几个测试，打包成 APK/AAB 装到真机。

---

## 第一步Action

打开 `app/src/main/java/com/example/android_learn/MainActivity.kt`，
读懂这 40 行代码 — 它是整个 App 的入口：

1. `MainActivity` 继承 `ComponentActivity`，是 App 主界面
2. `onCreate` 是入口方法，`setContent { ... }` 放 Compose 界面
3. `Greeting` 是 `@Composable` 函数，用 `Text` 显示文字
4. 把 `"Hello $name!"` 里的 `name` 改成自己的名字，运行看效果

---

## 技术栈速查

| 类别 | 技术 | 说明 |
|------|------|------|
| 语言 | Kotlin | Google 官方 Android 开发语言 |
| UI | Jetpack Compose | 声明式 UI 框架 |
| 设计 | Material3 | Google 设计规范 |
| 架构 | MVVM | Model-View-ViewModel |
| 数据库 | Room | SQLite 封装 |
| 网络 | Retrofit | HTTP 客户端 |
| 依赖注入 | Hilt | 基于 Dagger |
| 导航 | Navigation Compose | 页面路由 |
| 异步 | Coroutines + Flow | 协程与响应式流 |