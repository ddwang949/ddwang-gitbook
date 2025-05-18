# Linux

1.常用命令

| 命令                     | 说明               | 示例                                   |
| ---------------------- | ---------------- | ------------------------------------ |
| `ls`                   | 列出目录内容           | `ls -l /etc`                         |
| `cd`                   | 切换目录             | `cd /var/log`                        |
| `pwd`                  | 显示当前路径           | `pwd`                                |
| `mkdir`                | 创建目录             | `mkdir newdir`                       |
| `rmdir`                | 删除空目录            | `rmdir olddir`                       |
| `rm`                   | 删除文件/目录          | `rm -rf /tmp/test`                   |
| `cp`                   | 复制文件或目录          | `cp file1 file2`                     |
| `mv`                   | 移动或重命名           | `mv oldname newname`                 |
| `touch`                | 创建空文件            | `touch file.txt`                     |
| `find`                 | 查找文件             | `find /etc -name "*.conf"`           |
| `grep`                 | 文本搜索             | `grep -aFri 'error' /var/log/syslog` |
| `tree`                 | 显示目录结构树状图（可能需安装） | `tree /home`                         |
| `cat`                  | 查看文件内容           | `cat /etc/passwd`                    |
| `less`                 | 分页查看文件内容         | `less /var/log/messages`             |
| `more`                 | 类似 `less`，更老旧    | `more largefile.log`                 |
| `head`                 | 查看文件前几行          | `head -n 20 /var/log/syslog`         |
| `tail`                 | 查看文件最后几行         | `tail -f /var/log/nginx/access.log`  |
| `vim`                  | 编辑器              | `vim /etc/hosts`                     |
| `chmod`                | 修改权限             | `chmod 755 script.sh`                |
| `chown`                | 修改属主             | `chown root:root /var/www/html`      |
| `id`                   | 查看当前用户信息         | `id`                                 |
| `whoami`               | 显示当前用户名          | `whoami`                             |
| `su`                   | 切换用户             | `su - root`                          |
| `sudo`                 | 提权执行命令           | `sudo systemctl restart nginx`       |
| `passwd`               | 修改用户密码           | `passwd username`                    |
| `ping`                 | 测试网络连通性          | `ping 8.8.8.8`                       |
| `ifconfig` / `ip addr` | 查看网络接口           | `ip addr`                            |
| `netstat` / `ss`       | 查看端口和连接          | `ss -tuln`                           |
| `curl` / `wget`        | 下载或测试HTTP请求      | `curl http://example.com`            |
| `traceroute`           | 路由追踪             | `traceroute google.com`              |
| `nslookup`             | DNS 解析查询         | `nslookup baidu.com`                 |
| `top` / `htop`         | 实时资源监控           | `top`                                |
| `free -h`              | 查看内存使用           | `free -h`                            |
| `df -h`                | 查看磁盘使用           | `df -h`                              |
| `du -sh *`             | 查看目录大小           | `du -sh /var/*`                      |
| `uptime`               | 系统运行时间           | `uptime`                             |
| `ps aux`               | 查看进程列表           | \`ps aux                             |

2.常用日志位置

/var/log/auth.log 记录登录尝试、SSH连接、sudo使用等认证相关事件\
/var/log/syslog 记录大多数系统信息\
/var/log/apache2/access\_log 记录所有HTTP请求详情\
/var/log/apache2/error.log 记录所有出错的HTTP请求错误信息\
/root/.bash\_history | /home/$user/.bash\_history 记录键入的shell命令历史



3.常用文件位置

<table><thead><tr><th width="223.99993896484375">路径</th><th>用途说明</th></tr></thead><tbody><tr><td><code>/etc/passwd</code></td><td>用户账户信息（用户名、UID、GID、shell 等）</td></tr><tr><td><code>/etc/shadow</code></td><td>用户密码信息（加密密码、过期策略等，只有 root 可读）</td></tr><tr><td><code>/etc/group</code></td><td>用户组信息</td></tr><tr><td><code>/etc/sudoers</code></td><td>sudo 权限配置（推荐通过 <code>visudo</code> 编辑）</td></tr><tr><td><code>/etc/hostname</code></td><td>当前主机名设置</td></tr><tr><td><code>/etc/hosts</code></td><td>本地静态域名解析</td></tr><tr><td><code>/etc/fstab</code></td><td>系统启动时的挂载设置</td></tr><tr><td><code>/etc/resolv.conf</code></td><td>DNS 服务器配置</td></tr><tr><td><code>/etc/crontab</code></td><td>系统计划任务（可设定用户）</td></tr><tr><td><code>/var/log/messages</code></td><td>系统通用日志（含内核、服务等）</td></tr><tr><td><code>/var/log/syslog</code></td><td>Debian/Ubuntu 系统日志</td></tr><tr><td><code>/var/log/auth.log</code></td><td>登录认证相关日志（如 SSH 登录、sudo）</td></tr><tr><td><code>/var/log/secure</code></td><td>CentOS/Red Hat 上的认证日志</td></tr><tr><td><code>/var/log/dmesg</code></td><td>启动时的内核日志</td></tr><tr><td><code>/var/log/boot.log</code></td><td>系统启动过程日志</td></tr><tr><td><code>/var/log/cron</code></td><td>定时任务日志</td></tr><tr><td><code>/var/log/httpd/</code></td><td>Apache Web 服务日志</td></tr><tr><td><code>/var/log/nginx/</code></td><td>Nginx 日志（access.log 和 error.log）</td></tr><tr><td><code>/var/log/audit/</code></td><td>SELinux 或 auditd 安全审计日志</td></tr><tr><td><code>~/.bashrc</code></td><td>当前用户 bash shell 启动脚本</td></tr><tr><td><code>~/.ssh/authorized_keys</code></td><td>SSH 公钥认证列表</td></tr><tr><td><code>/var/www/html</code></td><td>默认网页目录（Apache/Nginx）</td></tr><tr><td><code>/etc/nginx/</code></td><td>Nginx 主配置目录</td></tr><tr><td><code>/etc/apache2/</code></td><td>Apache 主配置目录（Debian）</td></tr><tr><td><code>/etc/apache2/apache2.conf</code></td><td>主配置文件</td></tr><tr><td><code>~/.bash_history</code></td><td>当前用户的 Bash 历史命令记录</td></tr><tr><td><code>~/.profile</code></td><td>一般 shell 的配置文件</td></tr><tr><td><code>/etc/ssh/sshd_config</code></td><td>SSH 服务端配置文件</td></tr><tr><td><code>/etc/ssh/ssh_config</code></td><td>SSH 客户端默认配置</td></tr><tr><td><code>~/.ssh/authorized_keys</code></td><td>用户可登录的公钥列表</td></tr><tr><td><code>/var/log/auth.log</code></td><td>SSH 登录日志</td></tr><tr><td><code>/etc/crontab</code></td><td>系统级计划任务表</td></tr><tr><td><code>/etc/cron.d/</code></td><td>模块化计划任务配置</td></tr></tbody></table>

