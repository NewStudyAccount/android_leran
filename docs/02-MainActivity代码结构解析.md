# MainActivity.kt 代码结构解析

> 学习方案「第一步 Action」配套笔记
>
> 目标：读懂 Compose 空白模板的 47 行代码，搞清"App 是怎么从代码变成屏幕上的界面的"。
> 配合 `app/src/main/java/com/example/android_learn/MainActivity.kt` 边看边对照。

---

## 1. 全文件鸟瞰：4 块积木

整个文件只有四个部分，各司其职：

```
┌─────────────────────────────────────────────────────────┐
│ MainActivity.kt（47 行）                                  │
├─────────────────────────────────────────────────────────┤
│ ① import 区                          （3-14 行）        │
│    声明用到的类来自哪些包 —— "工具清单"                     │
├─────────────────────────────────────────────────────────┤
│ ② class MainActivity : ComponentActivity()（16-31 行）  │
│    App 入口界面 · 系统自动调用                             │
├─────────────────────────────────────────────────────────┤
│ ③ @Composable fun Greeting(...)      （33-39 行）       │
│    自定义 UI 组件 · 函数即零件                             │
├─────────────────────────────────────────────────────────┤
│ ④ @Preview fun GreetingPreview()     （41-47 行）       │
│    仅编辑器预览用 · 不打进 APK                             │
└─────────────────────────────────────────────────────────┘
```

| 部分 | 一句话职责 | 需要深究吗 |
|------|-----------|-----------|
| ① import 区 | 告诉 Kotlin 后面用到的 `Text`、`Scaffold` 等类去哪个包里找 | 暂时不用细看 |
| ② MainActivity | App 启动后的第一个界面，唯一干活的是 `setContent` | **重点** |
| ③ Greeting | 你自己写的 UI 零件，接收 `name`，显示一句话 | **重点** |
| ④ GreetingPreview | 编辑器右侧"样品间"，不运行 App 也能看渲染效果 | 知道即可 |

---

## 2. 逐块拆解

### 2.1 MainActivity 类（第 16-31 行）

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent { ... }
    }
}
```

三个关键语法点：

**① 冒号 = 继承。**
`class MainActivity : ComponentActivity()` 表示 MainActivity 继承自 `ComponentActivity`。
`ComponentActivity` 是 Google 写好的基类，自带"能显示界面、响应生命周期"等全部能力——
继承它等于白拿这些能力，自己只写差异部分（这里是"界面长什么样"）。

**② `override fun onCreate` = 生命周期回调。**
`onCreate` 是系统在**你打开 App 的瞬间自动调用**的方法（界面诞生时刻），不需要你手动调。
`override` 关键字表示"我重写父类的这个方法，塞进自己的逻辑"。
参数 `savedInstanceState: Bundle?` 末尾的 `?` 是空安全语法——这个参数可能为 null
（正常启动时就是 null，屏幕旋转重建时才带数据）。

**③ 为什么系统知道要启动这个类？**
因为在 `app/src/main/AndroidManifest.xml` 里把它注册成了 LAUNCHER 入口。
App 图标一点，系统就找到 MainActivity，调用它的 onCreate。

> 类比：MainActivity 是"店面"，AndroidManifest 是"工商注册"，系统是"顾客"——
> 顾客按注册信息找到店面，店面自动开门（onCreate）迎客。

### 2.2 Greeting 组件函数（第 33-39 行）

```kotlin
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Text(
        text = "Hello $name!",
        modifier = modifier
    )
}
```

**Compose 的核心思想：一个函数就是一个界面零件。**

- `@Composable` 注解：给函数发"UI 描述资格证"。没有它，函数里不能调用 `Text` 这类界面元素
- 参数 `name: String`：零件的输入口——喂什么名字，显示什么话
- `"Hello $name!"`：字符串模板，`$` 把变量值塞进字符串（Kotlin 笔记 1.4 节）
- `modifier: Modifier = Modifier`：**默认参数**（Kotlin 笔记 3.x 节）——
  调用时不传就用默认值，所以 Preview 里 `Greeting("Android")` 只传一个参数也合法

> 类比 HTML：`Greeting` 相当于自定义标签 `<Greeting name="Android" />`，
> 调用一次渲染一次，参数不同渲染结果不同。

### 2.3 GreetingPreview 预览（第 41-47 行）

```kotlin
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    Android_learnTheme {
        Greeting("Android")
    }
}
```

- 只服务于 Android Studio 右侧的 Preview 面板，**不打进 APK**，不影响真机运行
- 想改预览效果就改这里传的参数，想改真实 App 就改 MainActivity 里传的参数——两处独立

---

## 3. 真正的戏肉：setContent 界面嵌套链

MainActivity 里唯一干活的是 `setContent { ... }`（第 20-29 行）。
花括号层层嵌套像俄罗斯套娃，其实是一颗**洋葱**——每层包一层，最里心才是用户看到的字：

```
Android_learnTheme {                    ← 🎨 主题层：全局皮肤（颜色/字体）
    Scaffold(...) { innerPadding ->     ← 🦴 骨架层：页面骨架，算好系统栏边距
        Greeting(                       ← 🧩 组件层：你写的零件
            name = "Android",
            modifier = Modifier.padding(innerPadding)
        ) {
            Text("Hello $name!")        ← ✍️ 文字层：屏幕上真正显示的字
        }
    }
}
```

| 层 | 代码 | 职责 |
|---|---|---|
| 🎨 主题 | `Android_learnTheme { }` | 全局皮肤——App 用什么颜色、字体，坏了它背锅 |
| 🦴 骨架 | `Scaffold { }` | 页面基本骨架，处理状态栏/导航栏的边距 |
| 🧩 组件 | `Greeting(name = "Android")` | 自己写的零件，被塞进骨架里 |
| ✍️ 文字 | `Text("Hello $name!")` | 最里心，屏幕上真正显示的内容 |

**数据从外往里流，一层传一层**：Scaffold 算出边距 → `innerPadding` 喂给你 →
你用 `Modifier.padding(innerPadding)` 转交给 Greeting → Greeting 再交给 Text。

---

## 4. 三个最容易懵的语法细节

### 4.1 尾随 lambda（trailing lambda）

```kotlin
setContent { X }
// 完全等价于
setContent(content = { X })
```

Kotlin 允许把函数的**最后一个参数**（恰好是个 lambda）从括号里拎出来放后面。
纯粹是少打几个字的语法糖——看懂这个，嵌套花括号的恐惧消失 80%。
Compose 整个框架都建立在这个语法上，`Theme { }`、`Scaffold { }`、`Column { }` 全是尾随 lambda。

### 4.2 带参数的 lambda：`{ innerPadding -> ... }`

```kotlin
Scaffold(modifier = ...) { innerPadding ->
    Greeting(name = "Android", modifier = Modifier.padding(innerPadding))
}
```

`innerPadding ->` 是 lambda 的**参数声明**：Scaffold 算好了"该避开系统栏多少边距"，
通过这个参数喂给你。箭头左边是参数名，右边是怎么用它。
（对比 Kotlin 笔记 7.2 节的 `it`：单参数时可以简写 `it`，这里显式命名更清晰。）

### 4.3 `super.onCreate(savedInstanceState)`

`super` 指父类 `ComponentActivity`。这行是喊父类先把自家的初始化干完
（父类内部有一堆必须执行的逻辑），然后才轮到你加的戏。
**重写生命周期方法时，`super` 调用不能省**，否则界面根本起不来。

---

## 5. 动手验证（5 分钟）

改一个字，验证你真的看懂了这条链：

1. 打开 `MainActivity.kt`，找到**第 24 行**：`name = "Android"`
2. 改成自己的名字：`name = "你的名字"`
3. 运行 App（或只看右侧 Preview）

屏幕上 `Hello Android!` 变成 `Hello 你的名字!`——
因为你改的值顺着 **Greeting 的参数 → Text → $name 字符串模板** 一路流到了屏幕。
这条"数据向下流动"的链，就是 Compose 乃至阶段 3「单向数据流」的地基。

---

## 6. 自查清单

看完本文，应能不假思索地回答：

- [ ] 文件四块各自的职责是什么？
- [ ] `class A : B()` 里的冒号什么意思？
- [ ] `onCreate` 是谁调用的？什么时候执行？
- [ ] `@Composable` 注解的作用？
- [ ] `setContent { ... }` 的花括号是什么语法？
- [ ] `Scaffold { innerPadding -> }` 里 `innerPadding` 从哪来、到哪去？
- [ ] 把 `name = "Android"` 改掉后，屏幕上的字为什么会变？
