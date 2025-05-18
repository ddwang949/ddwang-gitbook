# Linux安全加固



Ubuntu Linux 常见安全加固措施

本章介绍 Ubuntu 系统中常见的安全加固操作，适用于通用服务器环境。加固目标包括最小权限原则、攻击面收缩、身份认证强化、日志审计等。

***

### 1. 用户与账户安全

#### 禁用 root 远程登录

编辑 SSH 配置文件：

```bash
sudo vim /etc/ssh/sshd_config
# 添加或修改以下项
PermitRootLogin no
```

#### 创建普通用户并授权

```bash
sudo adduser devuser
sudo usermod -aG sudo devuser
```

#### 强制使用 SSH 密钥认证登录

```bash
# 修改 SSH 配置
PasswordAuthentication no
```

***

### 2. 系统更新与补丁

#### 手动更新

```bash
sudo apt update && sudo apt upgrade -y
```

#### 启用自动安全更新

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

***

### 3. 密码与认证策略

#### 安装密码复杂度模块

```bash
sudo apt install libpam-pwquality
```

编辑 `/etc/pam.d/common-password`：

```
password requisite pam_pwquality.so retry=3 minlen=12 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
```

#### 设置密码有效期

```bash
sudo chage -M 90 -W 14 username
```

***

### 4. 防火墙配置（UFW）

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose
```

***

### 5. 服务精简与端口控制

#### 停止并禁用无用服务

```bash
sudo systemctl disable apache2
sudo systemctl stop cups
```

#### 查看开放端口

```bash
sudo ss -tuln
```

***

### 7. 日志与审计

#### 启用审计服务

```bash
sudo apt install auditd audispd-plugins
sudo systemctl enable auditd
```

#### 添加审计规则示例

```bash
sudo auditctl -w /etc/shadow -p wa -k shadow-watch
```

#### 常见日志位置

* `/var/log/auth.log`
* `/var/log/syslog`
* `/var/log/faillog`

***

