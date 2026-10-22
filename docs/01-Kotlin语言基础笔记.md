# Kotlin 语言基础笔记

> 阶段 1 · 约 2 周
>
> Android 的开发语言是 Kotlin（不是 Java）。这是地基，必须先打牢。
> 本笔记配合项目 `android_learn` 使用，所有代码可在 `MainActivity.kt` 的 `onCreate` 中试验。

---

## 1. 变量与类型

### 1.1 val 与 var

```kotlin
val name = "Android"   // 不可变（只读），赋值后不能再改，相当于 Java 的 final
var count = 0          // 可变，可以重新赋值

count = 1              // OK
// name = "iOS"        // 编译错误！val 不能重新赋值
```

**原则：优先用 `val`，只有确实需要修改时才用 `var`。** 这是 Kotlin 的核心编程习惯。

### 1.2 类型推断

Kotlin 能自动推断类型，不需要显式写：

```kotlin
val age = 25            // 自动推断为 Int
val price = 9.99        // 自动推断为 Double
val message = "Hello"   // 自动推断为 String

// 也可以显式声明（不确定类型时用）
val score: Int = 100
val pi: Double = 3.14159
```

### 1.3 常用基本类型

| 类型 | 说明 | 示例 |
|------|------|------|
| `Int` | 整数 | `42` |
| `Long` | 长整数 | `42L` |
| `Double` | 双精度浮点 | `3.14` |
| `Float` | 单精度浮点 | `3.14f` |
| `Boolean` | 布尔 | `true` / `false` |
| `String` | 字符串 | `"Hello"` |
| `Char` | 单个字符 | `'A'` |

### 1.4 字符串模板

用 `$` 引用变量，`${}` 写表达式 —— Android 开发中极其常用：

```kotlin
val name = "Android"
val version = 14

println("学习 $name $version")            // 学习 Android 14
println("名字长度是 ${name.length}")        // 名字长度是 7
println("明年是 ${version + 1}")            // 明年是 15
```

---

## 2. 空安全（Kotlin 核心特性）

Kotlin 在编译期就帮你拦截空指针异常（NPE），这是它比 Java 强大的地方。

### 2.1 不可空类型

默认类型不接受 null：

```kotlin
var name: String = "Android"
// name = null   // 编译错误！String 不允许 null
```

### 2.2 可空类型 `?`

在类型后加 `?` 表示允许 null：

```kotlin
var name: String? = "Android"
name = null       // OK，因为 String? 允许 null
```

### 2.3 安全调用 `?.`

可空类型用 `?.` 调用方法，遇到 null 时返回 null 而不是崩溃：

```kotlin
val name: String? = null
println(name?.length)    // null，不会崩溃

// 对比 Java：name.length() 遇到 null 直接 NPE
```

### 2.4 Elvis 运算符 `?:`

提供默认值，左侧为 null 时取右侧：

```kotlin
val name: String? = null
val length = name?.length ?: 0    // 如果 name 为 null，length 就是 0

// 等价于
val length2 = if (name != null) name.length else 0
```

### 2.5 非空断言 `!!`

强制告诉编译器"我确定它不是 null"。**小心使用**，如果为 null 会崩溃：

```kotlin
val name: String? = "Android"
println(name!!.length)     // 7

val wrong: String? = null
// println(wrong!!.length)  // 运行时崩溃！NullPointerException
```

**建议：能用 `?.` 和 `?:` 解决的，就别用 `!!`。**

---

## 3. 函数

### 3.1 基本定义

```kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}

// 调用
val result = add(3, 5)    // 8
```

### 3.2 表达式函数体

函数体只有一行返回值时，可以简写：

```kotlin
fun add(a: Int, b: Int) = a + b
fun double(x: Int) = x * 2
```

### 3.3 默认参数

参数可以给默认值，调用时可省略：

```kotlin
fun greet(name: String, greeting: String = "Hello") {
    println("$greeting, $name!")
}

greet("Android")              // Hello, Android!
greet("Kotlin", "Hi")         // Hi, Kotlin!
```

### 3.4 命名参数

调用时用参数名指定，增加可读性，顺序可变：

```kotlin
greet(greeting = "你好", name = "Android")   // 你好, Android!
```

### 3.5 无返回值的函数

返回类型用 `Unit`（类似 Java 的 void，可省略）：

```kotlin
fun printLog(msg: String): Unit {
    println(msg)
}

// 简写
fun printLog(msg: String) {
    println(msg)
}
```

---

## 4. 条件与循环

### 4.1 if 表达式

Kotlin 的 `if` 是表达式，有返回值：

```kotlin
val max = if (a > b) a else b

// 类似三元运算符（Kotlin 没有三元运算符 ? : ，因为 if 表达式就够了）
```

### 4.2 when（增强版 switch）

```kotlin
val score = 85
val grade = when {
    score >= 90 -> "A"
    score >= 80 -> "B"
    score >= 70 -> "C"
    else -> "D"
}

// 匹配具体值
when (day) {
    1, 2, 3, 4, 5 -> println("工作日")
    6, 7 -> println("周末")
    else -> println("无效")
}
```

### 4.3 for 循环

```kotlin
for (i in 1..5) {          // 1, 2, 3, 4, 5（包含两端）
    println(i)
}

for (i in 1 until 5) {     // 1, 2, 3, 4（不包含 5）
    println(i)
}

for (i in 10 downTo 1) {   // 10, 9, ..., 1（倒序）
    println(i)
}

for (i in 0..10 step 2) {  // 0, 2, 4, 6, 8, 10（步长 2）
    println(i)
}
```

### 4.4 遍历集合

```kotlin
val fruits = listOf("苹果", "香蕉", "橘子")

for (fruit in fruits) {
    println(fruit)
}

// 带索引
for ((index, fruit) in fruits.withIndex()) {
    println("$index: $fruit")
}
```

---

## 5. 类与对象

### 5.1 基本类

```kotlin
class Person(val name: String, var age: Int) {
    // val name / var age 自动成为属性（构造函数参数加 val/var）

    fun introduce() {
        println("我叫 $name，今年 $age 岁")
    }
}

val p = Person("小明", 25)
p.introduce()         // 我叫 小明，今年 25 岁
println(p.name)       // 小明
p.age = 26            // var 可修改
```

### 5.2 data class（数据类）

专门存数据的类，自动生成 `equals`、`hashCode`、`toString`、`copy`：

```kotlin
data class User(val name: String, val age: Int)

val u1 = User("小明", 25)
println(u1)                    // User(name=小明, age=25) — 自动 toString

val u2 = u1.copy(age = 26)     // 复制并修改部分字段
println(u2)                    // User(name=小明, age=26)

println(u1 == User("小明", 25)) // true — 自动 equals 比较内容
```

**Android 开发中数据模型几乎都用 data class**（对应实体类、API 响应模型等）。

### 5.3 object 单例

用 `object` 替代 Java 的单例模式，全局唯一实例：

```kotlin
object Calculator {
    fun add(a: Int, b: Int) = a + b
    fun multiply(a: Int, b: Int) = a * b
}

// 直接用类名调用，不需要创建实例
val sum = Calculator.add(1, 2)        // 3
```

### 5.4 companion object（伴生对象）

类内部的静态成员：

```kotlin
class AppConfig {
    companion object {
        const val VERSION = "1.0"
        val DEFAULT_NAME = "Android"

        fun create() = AppConfig()
    }
}

println(AppConfig.VERSION)     // 1.0
```

---

## 6. 集合

### 6.1 不可变集合（默认）

```kotlin
val nums = listOf(1, 2, 3)         // List<Int> 只读
val names = setOf("A", "B", "C")   // Set<String> 只读
val map = mapOf("a" to 1, "b" to 2) // Map<String, Int> 只读

// nums.add(4)   // 错误！不可变集合没有 add 方法
```

### 6.2 可变集合

```kotlin
val nums = mutableListOf(1, 2, 3)
nums.add(4)                    // [1, 2, 3, 4]
nums.removeAt(0)               // [2, 3, 4]
nums[0] = 10                   // [10, 3, 4]

val map = mutableMapOf("a" to 1)
map["b"] = 2                   // 添加
map.remove("a")                // 删除
```

### 6.3 集合操作符（函数式 API，非常常用）

```kotlin
val nums = listOf(1, 2, 3, 4, 5)

// map：转换每个元素
val doubled = nums.map { it * 2 }          // [2, 4, 6, 8, 10]

// filter：过滤
val evens = nums.filter { it % 2 == 0 }    // [2, 4]

// forEach：遍历
nums.forEach { println(it) }

// sortedBy：排序
val sorted = nums.sortedBy { it }          // 升序

// find：查找第一个匹配的
val first = nums.find { it > 3 }           // 4

// groupBy：分组
val grouped = nums.groupBy { it % 2 }      // {1=[1,3,5], 0=[2,4]}

// 链式调用
val result = nums
    .filter { it > 2 }
    .map { it * 10 }
    .sortedBy { it }                        // [30, 40, 50]
```

**`it` 是 Lambda 的隐式单参数名**，当 Lambda 只有一个参数时可用 `it` 代替。

---

## 7. Lambda 与高阶函数

### 7.1 Lambda 表达式

```kotlin
// 完整写法
val sum = { a: Int, b: Int -> a + b }
println(sum(3, 5))     // 8

// 作为函数参数传递
val nums = listOf(1, 2, 3)
nums.forEach({ n -> println(n) })

// Lambda 是最后一个参数时，可放括号外
nums.forEach { n -> println(n) }

// 单参数用 it
nums.forEach { println(it) }
```

### 7.2 高阶函数

接收函数作为参数，或返回函数：

```kotlin
fun operate(a: Int, b: Int, op: (Int, Int) -> Int): Int {
    return op(a, b)
}

val sum = operate(3, 5) { a, b -> a + b }       // 8
val diff = operate(3, 5) { a, b -> a - b }      // -2
```

---

## 8. 协程基础（异步编程）

Android 的网络请求、数据库操作都不能在主线程做，必须异步。Kotlin 用协程解决。

### 8.1 基本概念

- `suspend`：标记挂起函数，可在协程中暂停执行不阻塞线程
- `CoroutineScope`：协程作用域，管理协程生命周期
- `launch`：启动一个协程（不返回结果）
- `Dispatchers`：指定在哪个线程执行

### 8.2 简单示例

```kotlin
import kotlinx.coroutines.*

// 在主线程启动协程
CoroutineScope(Dispatchers.Main).launch {
    // IO 线程做耗时操作（网络/数据库）
    val data = withContext(Dispatchers.IO) {
        fetchDataFromNetwork()    // suspend 函数
    }
    // 回到主线程更新 UI
    textView.text = data
}

suspend fun fetchDataFromNetwork(): String {
    delay(1000)    // 模拟网络延迟，不阻塞线程
    return "数据"
}
```

### 8.3 Dispatchers 选择

| Dispatcher | 用途 |
|-----------|------|
| `Dispatchers.Main` | 主线程，UI 操作 |
| `Dispatchers.IO` | 网络请求、数据库、文件读写 |
| `Dispatchers.Default` | CPU 密集计算 |

---

## 9. 实战练习

### 练习 1：基础语法（在 onCreate 里试验）

打开 `MainActivity.kt`，在 `onCreate` 顶部加入：

```kotlin
import android.util.Log

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    enableEdgeToEdge()

    // 练习：Kotlin 基础
    val name = "Android"
    var count = 0
    count = 10

    val user = User("小明", 25)
    Log.d("KotlinPractice", "名字: ${user.name}, 年龄: ${user.age}")

    val nums = listOf(1, 2, 3, 4, 5)
    val sum = nums.filter { it > 2 }.sum()
    Log.d("KotlinPractice", "大于2的数之和: $sum")

    // ... 原有 setContent 代码
}
```

### 练习 2：data class

自己定义一个 `Course`（课程）数据类，包含课程名、学分、是否必修，创建几个实例放进列表，用 `filter` 筛选必修课。

---

## 10. 学习资源

- **官方教程**：[Kotlin 语言基础 Codelab](https://developer.android.com/kotlin/learn)
- **交互练习**：[Kotlin Koans](https://play.kotlinlang.org/koans) — 在浏览器里写代码答题
- **语言手册**：[Kotlin 官方文档](https://kotlinlang.org/docs/home.html)
- **视频课**：Google 官方 Android 开发免费课程（含 Kotlin 入门）

---

## 11. 进入下一阶段标志

能独立完成以下任务，说明可以进入阶段 2：

- [ ] 理解 `val` / `var` 区别，能正确选用
- [ ] 看到可空类型 `String?` 会用 `?.` 和 `?:` 处理
- [ ] 会写函数，会用默认参数和命名参数
- [ ] 能定义 `data class`，知道它的自动生成方法
- [ ] 熟练使用 `map` / `filter` / `forEach` 操作集合
- [ ] 看懂协程基本结构（不必精通，有概念即可）