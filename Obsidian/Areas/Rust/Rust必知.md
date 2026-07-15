---
created: 2026-06-10T23:35
updated: 2026-06-12T15:29
---
**1.元组i结构体**
```rust
use std::ops::Add;
struct Millimeters(u32);
struct Meters(u32);
impl Add<Meters> for Millimeters {
    type Output = Millimeters;
    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + (other.0 * 1000))
    }
}
```
`
` struct Meters(u32)`这里用到了元组结构体，与普通话结构体相比，在创建实例时有所不同，列如创建Meters实例`let a = Meters(8)` ,访问时只能用下标

2.**self与Self**

self 表示当前类型的实例（对象）
Self 是一个类型别名，表示当前正在实现的类型`
如：`fn copy(&self) -> Self`，self表示传入者，Self表示传入者的类型

3.**关联函数与方法**

它们都放在`impl` 块内

关联函数：
```rust
 fn new(width: u32, height: u32) -> Self {
        Self { width, height }
    }
```
**无self参数**
调用方式：**类型名::函数名()**

方法：
```rust
fn resize(&mut self, width: u32, height: u32) {
        self.width = width;
        self.height = height;
```
第一个参数必须是**self**
调用方式：**实例.方法名()**

**4.`option<T>` 枚举类型**
```rust
enum Option<T> {
    Some(T),  // 包含一个值
    None,     // 没有值
}
```



rust中声明const常量时必须标注类型

rust要求if表达式的所有分支返回的类型必须完全一致

如果把if......else当做表达式来写(比如赋值)，必须保证它在所有情况都有一个值

初始化数组：
		1.  指定类型与长度: let a:[i32;3] = [1,2,3]
		2. 全部初始化为一个值: let a = [0;5]
		3. 空数组: let empty:[i32;0] = []

数组切片（eg:arr)这样表示:arr[1..3] (表示截取第二个到第四个数)

rust中访问元组元素用的是"."而不是"()"!

动态数组创建宏`vec！` 

&引用不可变！！！

```
fn vec_map_multiply_two(input: &[i32]) -> Vec<i32> { input 
.iter() // 1. 生成input的不可变引用迭代器（元素类型&i32） 
.map(|element| { // 2. 对每个元素执行转换（多行闭包形式） 
element * 2 // 3. 核心逻辑：元素乘以2（自动解引用&i32为i32） }) 
.collect() // 4. 收集结果为Vec<i32>（由函数返回值推断类型） }
```

**`{:?}` 占位符**： 它告诉 Rust 使用 `Debug` trait 来格式化变量。因为你定义的是 `struct UnitStruct;`，对于**单元结构体**，`Debug` 的默认输出就是它的**类型名称**

字符串更新语法:
```
let u1 = User { name: "Alice".into(), age: 20, }; // 只改 age，其余从 u1 来 let u2 = User { age: 21, ..u1 };
``` 

拼接字符串
```rust
// 1. format!（最常用、最安全）
let s = format!("{}{}", "hello", " rust");

// 2. +
let s = "hello".to_string() + " rust";

// 3. push_str
let mut s = String::new();
s.push_str("hello");
s.push_str(" rust");
```
**结果都是：`String` 类型**

---

替换单词
```
let s = "I love Rust".replace("Rust", "Java");
```
- 替换**所有**匹配的内容
- 返回新 `String`
 去掉空格
```
// 去掉 所有空格
let s = "a b c".replace(" ", "");  // abc

// 去掉 首尾空格
let s = "  hello  ".trim();        // hello
```

一句话记住
- 拼接 → `format!`
- 替换 → `replace`
- 去空格 → `replace(" ", "")` 或 `trim()`
- 这些操作**都生成新字符串，不修改原来的**

- 直接写的字符串字面量（如 `"abc"`）→ 必是 `&str`；
- 调用 `to_string()`/`to_owned()`/`into()`（对 `&str`）/`format!`/`replace()`/`to_lowercase()` → 生成 `String`；
- 切片操作（`[0..1]`）、`trim()` 等仅 “裁剪 / 截取” 原字符串的操作 → 返回 `&str`。

### Rust 模块 `use`/`as`/`pub use` 核心知识点总结

#### 1. 基础概念：模块与访问控制

- Rust 模块内的标识符（常量、函数、结构体等）**默认私有**，外部无法访问；
- 只有标记 `pub` 的标识符，才能被模块外部访问（如子模块里的 `pub const PEAR`）。

#### 2. `use` 关键字：引入模块路径

- 作用：将深层模块的标识符 “拉” 到当前作用域，避免每次写完整长路径；
- 语法：`use 模块路径::标识符;`，其中 `self::` 指代**当前模块**（如 `self::fruits::PEAR` 明确从当前模块的 `fruits` 子模块找 `PEAR`）；
- 局限：普通 `use` 仅作用于模块内部，外部仍无法访问引入的标识符。

#### 3. `as` 关键字：重命名标识符

- 作用：给引入的标识符起简短别名，或解决命名冲突；
- 语法：`use 模块路径::标识符 as 新名称;`（如 `PEAR as fruit` 把 `PEAR` 重命名为 `fruit`）；
- 注意：重命名仅改变 “访问名称”，不改变标识符本身的属性（如常量 / 变量类型）。

#### 4. `pub use`：重导出（核心考点）

- 作用：`pub use` = `use`（引入作用域） + `pub`（暴露给外部），也叫 “重导出”；
- 场景：把深层模块的功能 “提上来”，对外提供简洁的接口（隐藏内部模块结构）；
- 你的问题核心：仅写 `use self::fruits::PEAR as fruit` 时，`fruit` 是模块内私有，`main` 无法访问；必须加 `pub` 变成 `pub use`，外部才能通过 `delicious_snacks::fruit` 访问。

#### HashMap Entry API 核心用法

- `entry(key).or_default()`：
    
    - 作用：查找 HashMap 中指定 key，存在则返回该值的可变引用（`&mut V`），不存在则插入该类型的默认值（需实现 `Default` trait）并返回可变引用。
    - 优势：比 `or_insert(类型::default())` 更简洁，减少冗余代码，易维护（结构体字段变化时自动适配默认值）。
    - 对比 `or_insert()`：`or_insert()` 需手动传入初始值，适用于非默认初始化场景，通用性更强。
    
- 返回值类型：`&mut V`（可变引用），目的是直接修改 HashMap 中的值，避免二次查找，保证效率。

#### 一、枚举（Enum）

1. **关联数据特性**：Rust 枚举可携带关联数据（如 `Command::Append(usize)`），用于传递动态参数（如追加次数）；
2. **模式匹配**：通过 `match` 表达式处理枚举所有变体，Rust 强制覆盖所有分支，避免逻辑遗漏；
3. **类型匹配**：`Append` 关联的 `usize` 是 Rust 内置无符号整数类型，适配平台内存宽度，专为计数 / 长度 / 索引设计。

#### 二、字符串处理（String/&str）

1. **核心类型区分**：
    
    - `&str`：字符串切片，仅借用数据、无所有权，如 `trim()` 返回 `&str`；
    - `String`：拥有所有权的字符串，可独立管理内存，是函数返回 / 存储的首选；
    
2. **关键操作**：
    
    - 转大写：`to_uppercase()` 直接返回 `String`；
    - 去空白：`trim()` 返回 `&str`，需通过 `to_string()` 转为 `String`；
    - 字符串重复：`repeat(n)` 生成 n 次重复的字符串，参数为 `usize`，返回 `String`；
    - 字符串拼接：`format!("{}{}", s1, s2)` 是最优方式，自动兼容 `String`/`&str`，返回新 `String`；
    
3. **易错点**：
    
    - 方法调用顺序：需先做 `trim()`（轻量借用操作）再转 `String`，避免悬垂引用；
    - 拼接方式对比：`format!` 比 `push_str()`（需可变变量）、`+` 运算符（需 `&str` 右侧参数）更简洁通用。
    

#### 三、所有权与移动语义

1. **值传递**：函数参数 `Vec<(String, Command)>` 为值传递，遍历会移动元组所有权，无需解引用；
2. **返回值设计**：返回 `Vec<String>` 而非 `Vec<&str>`，因为 `String` 拥有独立所有权，避免 `&str` 依赖外部数据生命周期导致的悬垂引用；
3. **临时值处理**：动态生成的字符串（如转大写、拼接）必须用 `String`，不能直接返回指向临时值的 `&str`。

#### 四、模块（Module）与可见性

1. **跨模块引用**：
    
    - 模块内引用父作用域枚举：`use super::Command`；
    - 测试模块导入其他模块函数：`use crate::my_module::transformer`；
    
2. **可见性控制**：函数需用 `pub` 修饰才能被外部模块访问（如 `pub fn transformer`）。

#### 五、向量（Vec）

1. **基础操作**：`Vec::new()` 创建空向量，`push()` 向尾部添加元素，支持遍历和解构元组（`for (s, cmd) in input`）；
2. **场景适配**：`Vec<(String, Command)>` 存储 “字符串 + 命令” 元组，`Vec<String>` 存储处理后的结果，契合 “有序集合” 的需求。

#### 六、测试与语法规范

1. **测试导入**：需显式导入目标函数（`use crate::my_module::transformer`）才能在测试模块中使用；
2. **语法细节**：
    
    - 解引用：仅当变量为引用类型（如 `&usize`）时需 `*n` 解引用，值类型无需；
    - `unwrap()` 适用场景：仅用于 `Option<T>/Result<T, E>`，普通类型（如 `usize`）无需使用；
    - 变量命名：避免笔误（如 `tramsformer` → `transformed`），保证代码可读性。
    

#### 七、核心逻辑梳理

1. **需求实现**：`Append(n)` 是 “给原字符串追加 n 次 'bar'”，而非 “重复原字符串”，需用 `format!("{}{}", s, "bar".repeat(n))` 实现；
2. **类型统一**：`match` 所有分支需返回相同类型（`String`），确保编译通过；
3. **运行验证**：通过 `cargo test` 验证功能正确性，`cargo run` 查看运行结果。

#### 1. if-let 核心

- 本质：`match` 表达式的简化语法糖，专用于**只处理一种模式、忽略其他模式**的场景，避免冗余的 `_ => ()` 分支。
- 语法：`if let 模式 = 表达式 { 匹配成功逻辑 }`（可选加 `else` 处理匹配失败）。
- 核心场景：提取 `Option` 中 `Some` 的值、处理枚举的单个变体。

#### 2. while-let 核心

- 本质：「循环版的 if-let」，**只要模式匹配成功就持续执行循环体，匹配失败则退出循环**。
- 语法：`while let 模式 = 表达式 { 循环体 }`（`else` 仅在「匹配失败导致循环结束」时执行，手动 `break` 则不执行）。
- 核心场景：处理可耗尽的操作（如 `Vec::pop()`/ 迭代器 `next()`），替代冗余的 `loop + match`。

#### 3. 嵌套 Option（关键易错点）

- 成因：当 `Vec<Option<T>>` 调用 `pop()` 时，`pop()` 本身返回 `Option<元素>`，因此最终返回值是 `Option<Option<T>>`（双层 Option）。
- 匹配规则：
    
    - 需用 `Some(Some(变量))` 嵌套匹配，外层 `Some` 匹配「是否弹出元素」，内层 `Some` 匹配「元素是否为有效值」；
    - 仅写单个 `Some` 会导致变量类型为 `Option<T>`，无法直接与 `T` 类型的值比较。
    

#### 4. 核心对比与原则

表格

| 语法        | 适用场景                    | 核心逻辑               |
| --------- | ----------------------- | ------------------ |
| if-let    | 单次模式匹配（忽略其他）            | 匹配成功执行一次，失败跳过      |
| while-let | 循环模式匹配（直到失败）            | 匹配成功循环执行，失败退出      |
| 嵌套 Option | `Vec<Option<T>>::pop()` | 双层 `Some` 匹配，逐层提取值 |

### 关键易错点回顾

1. 拼写错误：`Some` 而非 `Somme`（Rust 大小写敏感）；
2. 类型混淆：`Vec<Option<T>>::pop()` 返回 `Option<Option<T>>`，需嵌套匹配，避免将 `Option<T>` 直接与 `T` 比较；
3. 逻辑边界：`while-let` 的 `else` 仅在「匹配失败退出」时执行，手动 `break` 不会触发。

#### 1. 问题根源：Rust 所有权转移导致的编译错误

- 原代码中 `match optional_point { Some(p) => ... }` 会**转移 `optional_point` 的所有权**，导致后续打印 `optional_point` 时因所有权丢失报错；
- Rust 中默认的模式匹配会获取值的所有权，除非显式声明 “只借用不转移”。

#### 2. 两种核心解决方案（效果等价）

表格

|方案|写法特征|核心逻辑|
|---|---|---|
|对匹配目标加引用|`match &optional_point { Some(p) }`|先对整个 `optional_point` 做不可变借用，再匹配，`p` 为 `&Point`|
|使用 `ref` 关键字|`match optional_point { Some(ref p) }`|匹配原值，但对内部的 `p` 声明 “只取引用”，`p` 为 `&Point`|

#### 3. 拓展：可变引用场景（`ref mut`）

- 若需修改值，需满足两个条件：
    
    1. 原变量声明为可变：`let mut optional_point = ...`；
    2. 模式中使用 `ref mut`：`Some(ref mut p)`，此时 `p` 为 `&mut Point`，可修改字段值。
    

### 关键记忆点

1. Rust 模式匹配默认转移所有权，需显式借用（`&` 或 `ref`）才能保留原变量的使用权；
2. `&` 是 “对外借用”（对整个匹配目标加引用），`ref` 是 “对内借用”（在模式内对变量加引用），两者效果一致；
3. 可变引用需搭配 `mut` 变量 + `ref mut` 关键字使用。

