# Linux_Study_Notes
记载linux指令和相关知识

---

### **一、基础操作命令**  
1. **文件/目录操作**  
   - `cd`：切换目录  
     ```bash
     cd /var/log  # 进入日志目录
     ```
   - `ls`：列出文件  
     ```bash
     ls -lrt  # 按时间倒序列出文件（常用于找最新日志）
     ```
   - `cat`/`tail`/`less`：查看文件内容  
     ```bash
     tail -f app.log  # 实时追踪日志更新（测试时监控日志）
     ```
   - `grep`：文本搜索  
     ```bash
     grep "ERROR" test_report.log  # 过滤日志中的错误信息
     ```
   - `find`：查找文件  
     ```bash
     find /home -name "*.log"  # 搜索所有.log文件
     ```

2. **权限管理**  
   - `chmod`：修改文件权限  
     ```bash
     chmod 755 startup.sh  # 赋予脚本可执行权限
     ```
   - `sudo`：提权执行  
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

