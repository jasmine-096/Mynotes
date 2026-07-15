---
created: 2026-07-14T11:53
updated: 2026-07-15T10:26
---
### 一、 `sql`的基础框架：

整体结构：
```
数据库（Database）
│
├── 表1（Table）
│     │
│     ├── 行（Row）  ← 一条记录
│     ├── 行
│     └── 行
│
├── 表2
│
└── 表3
```

表内结构：

列：表示基本属性
行：表示一项完整数据


`eg`：

| prod_id | prod_name | price | category |
| ------- | --------- | ----- | -------- |
| 001     | Apple     | 5     | fruit    |
| 002     | Banana    | 3     | fruit    |
| 003     | Keyboard  | 200   | device   |

列：prod_id,prod_name,price,category

行：001,Apple,5,fruit ......

#### 注释

行内注释：`-- or #`

多行注释：`/*。。。*/`

### 二、检索
#### 1.普通检索

`SELECT 列名 FROM 表名`来检索表中的列值

可以使用通配符`*`来检索所有列

对于列中的相同值，使用`SELECT DISTINCT`来检索不同值

使用`LIMIT 数值 OFFSET `数值来限制输出，前一个数值表示限制多少行，后一个数值表示从第几行开始，==行数是从0开始，因此第1行实际上是第0行==

#### 2. 排序检索

使用`OREDER BY 列名`语句来对列进行升序排序，但要保证它为最后一条子句

也可以按多个列进行排序，`eg：ORDER BY prod_price,prod_name;`,它的含义是，先对`prod_price`进行排序，如果存在多个值相同的情况，再按照`prod_name`进行排序，但如果没有多个`prod_price`值相同，那么整句就相当于只对`prod_price`进行排序

也可以不用输入列名，比如：`SELECT age,height,weight`，如果想对age与weight进行排序，可以写`ORDER BY 1,3`，数值2,3是`SELECT`中的列的相对位置

使用`ORDER BY 列名 DESC;`可以对该列进行降序排序，但如果你要对多个列进行降序排序，需要对每个列都加上`DESC`

### 三、过滤

#### 1.基础过滤

使用WHERE子句来进行基础过滤
```sql
SELECT prod_name
FROM products
WHERE prod_price = 6;
```

上面的代码是说，挑选出产品价格是6的产品名称

| 分类    | 操作符                   | 说明                      | 示例                                            |
| ----- | --------------------- | ----------------------- | --------------------------------------------- |
| 比较操作符 | `=`                   | 等于                      | `WHERE age = 18`                              |
|       | `<>` 或 `!=`           | 不等于                     | `WHERE status != 'inactive'`                  |
|       | `>`                   | 大于                      | `WHERE salary > 5000`                         |
|       | `<`                   | 小于                      | `WHERE score < 60`                            |
|       | `>=`                  | 大于等于                    | `WHERE age >= 18`                             |
|       | `<=`                  | 小于等于                    | `WHERE price <= 100`                          |
| 逻辑操作符 | `AND`                 | 同时满足多个条件                | `WHERE age > 18 AND gender = 'M'`             |
|       | `OR`                  | 满足任一条件                  | `WHERE city = '北京' OR city = '上海'`            |
|       | `NOT`                 | 取反                      | `WHERE NOT status = 'deleted'`                |
| 范围与集合 | `BETWEEN ... AND ...` | 在某个范围内（含边界）             | `WHERE age BETWEEN 18 AND 30`                 |
|       | `IN (...)`            | 匹配列表中的任意一个值             | `WHERE dept IN ('IT', 'HR')`                  |
|       | `NOT IN (...)`        | 不在列表中                   | `WHERE status NOT IN ('closed', 'cancelled')` |
| 模糊匹配  | `LIKE`                | 模式匹配（`%` 多个字符，`_` 单个字符） | `WHERE name LIKE '张%'`                        |
|       | `NOT LIKE`            | 不匹配模式                   | `WHERE email NOT LIKE '%@test.com'`           |
| 空值判断  | `IS NULL`             | 值为 NULL                 | `WHERE phone IS NULL`                         |
|       | `IS NOT NULL`         | 值不为 NULL                | `WHERE address IS NOT NULL`                   |

OR运算，满足任一条件皆可，`WHERE Age>60 OR Age<10'意思是，年龄大于60或小于10的都符合条件 

sql与其他语言一样，会优先进行AND运算，列如：
`WHERE id = 'AVL' OR id = 'ASL' AND price >= 1000`,这段代码会造成歧义：价格超过1000且品牌是ASL的商品与品牌是AVL且价格不限的商品。如果按照原意应该是：AVL与ASL中价格超过1000的商品

通过加上括号：`WHERE （id = 'AVL' OR id = 'ASL'） AND price >= 1000`，即可消除歧义


