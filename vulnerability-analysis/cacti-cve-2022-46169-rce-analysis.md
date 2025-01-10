---
description: 之前写的，从微步搬回到新的page上先
---

# Cacti CVE-2022-46169 远程命令执行漏洞分析

#### 漏洞编号

CVE-2022-46169

#### 影响范围

| 漏洞编号           | 影响厂商                | 产品    |
| -------------- | ------------------- | ----- |
| CVE-2022-46169 | The Cacti Group Inc | Cacti |



漏洞分析

### **CVE-2022-46169**

CVE-2022-46169漏洞利用包含两部分

1.身份认证绕过

2.远程命令执行

#### **漏洞分析**

1. **身份认证绕过分析**

由CVE描述可知，产生漏洞的文件为remote\_agent.php，

默认直接访问会显示报错信息FATAL: You are not authorized to use this service

查找到报错信息提示的代码处：

```
if (!remote_client_authorized()) {
    print 'FATAL: You are not authorized to use this service';
    exit;
}
```

因此身份认证绕过实际上则是需要绕过函数remote\_client\_authorized()的判断，函数remote\_client\_authorized()的实现如下：

\[PICTURE]

remote\_client\_authorized()函数首先调用get\_client\_addr()函数，此函数在lib/functions.php中调用

\[picture2]

```
code demo
```
