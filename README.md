# Linux_Study_Notes
记载linux指令和相关知识

---

### **一、基础操作命令**  
1. **文件/目录操作**  
   - `cd`：切换目录(change directory)  
     ```bash
     cd /var/log  # 进入日志目录
     ```
     
   - `ls`：列出文件(list)  
     ```bash
     ls -lrt  # 按时间倒序列出文件（常用于找最新日志）
     ```
     
   - `cat`/`tail`/`less`：查看文件内容(concatenate\less is more)  
     ```bash
     tail -f app.log  # 实时追踪日志更新（测试时监控日志）
     ```
     
   - `grep`：文本搜索(Globally search a Regular Expression and Print)  
     ```bash
     grep "ERROR" test_report.log  # 过滤日志中的错误信息
     ```
     
   - `find`：查找文件  
     ```bash
     find /home -name "*.log"  # 搜索所有.log文件
     ```

2. **权限管理**  
   - `chmod`：修改文件权限(change mode)  
     ```bash
     chmod 755 startup.sh  # 赋予脚本可执行权限
     ```
   - `sudo`：提权执行(Super User Do)  
     ```bash
     sudo yum install jmeter  # 安装测试工具
     ```

---

### **二、测试专用场景命令**  
1. **环境部署**  
   - `scp`：跨服务器传输文件  
     ```bash
     scp test.jar user@test-server:/opt  # 部署测试包到服务器
     ```
   - `wget`/`curl`：下载/测试接口  
     ```bash
     curl -X GET http://api:8080/health  # 测试服务健康状态
     ```
   - `tar`/`unzip`：压缩解压  
     ```bash
     tar -zxvf test-suite.tar.gz  # 解压测试套件
     ```

2. **进程管理**  
   - `ps`：查看进程  
     ```bash
     ps -ef | grep java  # 查找Java进程（如Tomcat）
     ```
   - `kill`：终止进程  
     ```bash
     kill -9 1234  # 强制杀死卡死的测试服务
     ```
   - `nohup`：后台运行  
     ```bash
     nohup ./run_tests.sh &  # 后台执行自动化测试脚本
     ```

3. **网络诊断**  
   - `netstat`：查看端口占用  
     ```bash
     netstat -tulnp | grep 8080  # 检查8080端口是否被占用
     ```
   - `ping`/`telnet`：网络连通性测试  
     ```bash
     telnet db-server 3306  # 测试数据库端口是否可达
     ```

---

### **三、进阶实用命令**  
1. **性能监控**  
   - `top`/`htop`：实时监控系统资源  
     ```bash
     top -p 5678  # 监控指定进程的CPU/内存占用
     ```
   - `free`/`df`：查看内存/磁盘  
     ```bash
     free -h  # 检查内存是否不足导致测试失败
     ```

2. **日志分析**  
   - `awk`：提取特定字段  
     ```bash
     awk '/ERROR/ {print $5}' app.log  # 提取错误日志的关键字段
     ```
   - `sed`：文本替换  
     ```bash
     sed -i 's/old_ip/new_ip/g' config.properties  # 批量修改配置文件
     ```

3. **自动化脚本**  
   - 管道符 `|`：组合命令  
     ```bash
     cat test.log | grep "FAILED" | wc -l  # 统计失败用例数
     ```
   - `crontab`：定时任务  
     ```bash
     crontab -e  # 添加每天凌晨执行测试脚本的任务
     ```

---

### **四、高频面试问题场景**  
1. **如何定位测试环境问题？**  
   ```bash
   # 1. 检查服务状态
   systemctl status nginx
   # 2. 查看错误日志
   tail -n 100 /var/log/nginx/error.log | grep "500"
   # 3. 确认端口监听
   netstat -tulnp | grep 80
   ```

2. **如何统计接口响应时间？**  
   ```bash
   # 使用curl测试API耗时
   curl -o /dev/null -s -w "Time: %{time_total}s\n" http://api/test
   ```

---

### **五、学习建议**
1. **重点掌握**：`grep`、`tail`、`find`、`ps`、`netstat`（使用率超80%）
2. **工具链扩展**：
   - 日志分析：`ELK`（Elasticsearch+Logstash+Kibana）
   - 性能测试：`jmeter` + `nmon`（资源监控）
3. **实战练习**：在Linux服务器上部署测试环境，并完成日志分析、故障排查全流程。

---

### 2.你如何使用 Linux 命令进行调试和问题排查？
在 **Linux** 环境中，作为测试工程师，使用命令行工具进行调试和问题排查是常见且必要的技能。Linux 提供了许多强大的命令行工具，可以帮助我们快速定位系统或应用程序中的问题。以下是我使用 Linux 命令进行调试和问题排查的几个常见方法：

#### **1. 查看系统资源使用情况**
在进行性能排查时，首先需要了解系统资源（如 CPU、内存、磁盘、网络）的使用情况。这些信息能帮助我们识别瓶颈或潜在的性能问题。

- **`top`**：查看系统的 CPU 使用、内存占用情况以及各进程的资源消耗。
  ```bash
  top
  ```
  `top` 命令会动态显示当前系统的进程信息，包括各个进程的 CPU 和内存占用情况，帮助你找到资源占用过高的进程。

- **`htop`**：`htop` 是 `top` 的增强版，提供更加友好的交互式界面和可视化显示，便于操作和查看。
  ```bash
  htop
  ```

- **`free`**：查看内存使用情况。
  ```bash
  free -h
  ```
  `free` 显示系统的内存和交换空间使用情况，`-h` 参数使其以更易读的格式显示（如 GB、MB 等）。

- **`df`**：查看磁盘空间使用情况。
  ```bash
  df -h
  ```
  `df` 显示文件系统的磁盘使用情况，`-h` 参数使其以人类可读的格式显示（如 GB、MB 等）。

- **`iostat`**：查看 I/O 性能，特别是磁盘和网络设备的性能。
  ```bash
  iostat -xz 1
  ```
  `iostat` 用来监控系统的 CPU 使用情况、设备 I/O 状况。`-xz` 参数用于显示详细的设备信息。

#### **2. 查找日志文件**
系统日志是排查问题的一个重要信息源，尤其是在调试应用程序或服务故障时。

- **`tail`**：查看日志文件的尾部内容，实时查看新日志。
  ```bash
  tail -f /var/log/syslog
  ```
  `tail -f` 会实时跟踪和输出日志文件的最新内容，帮助你实时监控系统的变化。

- **`grep`**：从日志文件中查找关键字，筛选出相关信息。
  ```bash
  grep "error" /var/log/syslog
  ```
  `grep` 用于从日志中查找特定的关键字（如 "error"），帮助快速定位系统或应用中的异常信息。

- **`journalctl`**：查看 systemd 日志（如果系统使用了 systemd）。
  ```bash
  journalctl -u nginx
  ```
  `journalctl` 用于查看由 `systemd` 管理的服务日志，`-u` 参数后跟服务名（如 `nginx`），用于查看特定服务的日志。

### **3. 监控网络连接**
当遇到网络问题时，我们可以使用以下命令来监控和排查网络连接问题。

- **`ping`**：检查与目标主机的网络连通性。
  ```bash
  ping google.com
  ```
  `ping` 用来测试主机之间的网络连接是否正常。

- **`netstat`**：查看当前网络连接、端口使用情况。
  ```bash
  netstat -tuln
  ```
  `netstat` 用于查看系统的网络连接状态，`-tuln` 参数显示监听的端口和连接状态。

- **`ss`**：比 `netstat` 更快的工具，查看端口和连接状态。
  ```bash
  ss -tuln
  ```

- **`iftop`**：实时显示网络流量。
  ```bash
  iftop
  ```
  `iftop` 用于实时查看网络接口的带宽使用情况，并显示流量较大的连接。

### **4. 调试进程问题**
有时进程出现问题时，需要查看进程的状态或堆栈信息。

- **`ps`**：查看当前系统中的所有进程。
  ```bash
  ps aux
  ```
  `ps aux` 显示系统中的所有进程，包含进程的 CPU 和内存使用情况。

- **`strace`**：跟踪系统调用，诊断进程执行过程中的问题。
  ```bash
  strace -p <pid>
  ```
  `strace` 用于跟踪进程的系统调用和信号。`-p` 后接进程 ID，帮助诊断进程运行时可能出现的错误。

- **`lsof`**：查看打开的文件和端口。
  ```bash
  lsof -i :80
  ```
  `lsof` 列出所有打开的文件，`-i :80` 可以查看所有与端口 80 相关的文件和连接。

### **5. 检查服务状态**
在排查服务问题时，检查服务的状态和日志是常用的操作。

- **`systemctl`**：查看和管理 systemd 服务的状态。
  ```bash
  systemctl status nginx
  ```
  `systemctl status` 用于查看服务（如 `nginx`）的当前状态。如果服务未启动或停止，可能需要重启服务进行调试。

- **`service`**：检查和管理传统 SysV 服务的状态（如果系统未使用 systemd）。
  ```bash
  service nginx status
  ```

### **6. 性能分析和调优**
在进行性能优化时，以下工具可以帮助分析应用的瓶颈：

- **`perf`**：性能分析工具，用于分析 CPU 和内存性能。
  ```bash
  perf stat -e cache-misses,cache-references,cycles,instructions sleep 10
  ```
  `perf` 可以用来收集应用运行时的性能数据，如 CPU 使用率、缓存命中率等。

- **`vmstat`**：查看系统的虚拟内存、进程、CPU、I/O 等性能统计。
  ```bash
  vmstat 1
  ```
  `vmstat` 输出系统的各种统计数据，用于诊断内存、I/O 等性能问题。

### **7. 查找和删除大文件**
在排查磁盘空间问题时，找到并处理大文件是重要的步骤。

- **`du`**：查看目录或文件的磁盘使用情况。
  ```bash
  du -sh /var/log/*
  ```
  `du` 用于查看目录或文件的磁盘使用情况，`-s` 用于汇总目录大小，`-h` 以人类可读的格式显示结果。

- **`find`**：查找系统中的大文件。
  ```bash
  find / -type f -size +100M
  ```
  `find` 用于查找文件，`-size +100M` 查找大于 100MB 的文件。

### **总结：**

在 **Linux** 环境中，测试工程师可以通过这些命令行工具来有效地进行调试和问题排查：
- 使用 `top`, `htop`, `iostat`, `free` 等命令检查系统资源。
- 使用 `grep`, `journalctl`, `tail` 等命令查看日志文件。
- 使用 `ping`, `netstat`, `ss`, `iftop` 等命令监控网络连接。
- 使用 `ps`, `strace`, `lsof` 等命令调试进程问题。
- 使用 `systemctl`, `service` 管理和检查服务状态。
- 使用 `du`, `find` 查找和删除大文件。

这些工具能够帮助你快速定位问题并提供解决方案，是在 Linux 环境中进行调试和问题排查时的重要武器。

---

### 3. 描述一个你通过 Linux 命令解决的实际问题。
在我参与的项目中，曾经遇到过一个涉及 **磁盘空间不足** 的问题。当时，应用的 **日志文件** 不断增加，导致服务器的磁盘空间被消耗殆尽，影响了系统的稳定性和响应时间。以下是我通过 **Linux 命令** 排查和解决这个问题的过程：

#### **问题描述：**

在我们的生产环境中，应用程序不断记录日志文件，但这些日志文件没有自动轮转（log rotation）机制。随着时间的推移，日志文件不断增大，占用了大量磁盘空间，最终导致磁盘满了。由于磁盘空间不足，系统开始变得非常缓慢，某些服务甚至崩溃。

#### **解决方案：**

#### **1. 确定磁盘空间占用情况**
首先，我使用 `df` 命令查看磁盘空间使用情况，确认哪些分区的空间已满：

```bash
df -h
```

输出的结果如下：

```plaintext
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       100G   98G     0 100% /
```

从输出中可以看到，根文件系统（`/`）的使用率已经达到 100%，这导致了磁盘空间不足。

#### **2. 查找占用空间较大的文件和目录**
然后，我使用 `du` 命令查找具体是哪些文件或目录占用了大量磁盘空间。首先，我检查了 `/var/log` 目录，因为日志文件通常会存放在这个目录下。

```bash
du -sh /var/log/*
```

输出的结果如下：

```plaintext
50G    /var/log/app.log
2G     /var/log/syslog
3G     /var/log/auth.log
```

可以看到，`/var/log/app.log` 文件占用了 **50GB** 的空间，而其他日志文件的大小较小。显然，`app.log` 文件的大小导致了磁盘空间的过度占用。

#### **3. 检查并删除旧的日志文件**
为了释放空间，我使用了 `ls -lh` 命令来查看 `app.log` 文件的具体大小和最后修改时间：

```bash
ls -lh /var/log/app.log
```

输出：

```plaintext
-rw-r--r-- 1 root root 50G Mar  1 10:00 /var/log/app.log
```

确认了 `app.log` 文件是最近生成的，并且文件的大小已经达到了 **50GB**。

为了解决磁盘空间不足的问题，我决定暂时删除该日志文件。首先，我确保该文件的内容不会影响系统的运行，并使用 `mv` 命令将它移到备份目录进行保存：

```bash
mv /var/log/app.log /var/log/app.log.bak
```

这将文件移动到备份位置，以防后续需要查看。然后，我使用 `touch` 命令创建一个新的空日志文件来避免应用程序出现日志文件缺失的情况：

```bash
touch /var/log/app.log
```

#### **4. 配置日志轮转机制（Log Rotation）**
为了避免将来再次出现磁盘空间不足的问题，我决定配置 **logrotate** 工具来自动轮转日志文件。

我编辑了 `/etc/logrotate.d/app` 配置文件，并添加了日志轮转规则：

```bash
/var/log/app.log {
    size 10G
    rotate 5
    compress
    missingok
    notifempty
    create 0644 root root
}
```

这段配置的意思是：
- 当 `app.log` 文件达到 **10GB** 时，自动轮转。
- 保留 5 个轮转的日志文件（即最多保存 50GB 的日志文件）。
- 轮转后的日志文件会进行压缩。
- 如果日志文件为空，则不会进行轮转。
- 新日志文件创建时，权限为 `0644`，并且由 `root` 用户拥有。

#### **5. 验证和测试**
在配置完日志轮转后，我运行了 `logrotate` 命令来手动测试配置是否正确：

```bash
logrotate -d /etc/logrotate.d/app
```

该命令以 **debug** 模式运行，模拟日志轮转过程，并输出详细信息。确认没有错误后，我手动运行了日志轮转：

```bash
logrotate /etc/logrotate.d/app
```

最后，验证了 `/var/log/app.log` 文件的大小是否被控制，并且轮转后的日志文件已被压缩。

#### **6. 监控和预警**
为了防止类似的问题再次发生，我设置了一个 **定期监控** 脚本，使用 `df` 和 `du` 命令定期检查磁盘空间，并通过 **邮件通知** 或 **Slack** 通知团队管理员，确保磁盘空间及时得到管理。

---

#### **总结：**

通过这一系列的 **Linux 命令**（如 `df`、`du`、`ls`、`mv`、`touch` 和 `logrotate`），我成功解决了服务器 **磁盘空间不足** 的问题。具体步骤如下：
1. 使用 `df` 和 `du` 查找空间占用情况。
2. 删除不必要的旧日志并备份重要文件。
3. 配置 **logrotate** 自动轮转日志，防止日志文件无限制增长。
4. 设置监控和预警机制，避免类似问题的再次发生。

这个过程不仅帮助我排查和解决了实际问题，还通过配置自动化的 **日志轮转机制**，提升了系统的稳定性，并避免了将来磁盘空间耗尽的风险。
