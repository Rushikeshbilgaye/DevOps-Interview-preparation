# 🐧 Linux Interview Questions & Answers

This file contains **50 Linux interview questions and answers** for DevOps Engineer interview preparation.

* **20 Normal Questions & Answers**
* **30 Scenario-Based Questions & Answers**

---

# 📚 Part 1 — Normal Linux Interview Questions

## Q1. What is Linux?

**Answer:**

Linux is an open-source operating system based on the Unix operating system architecture.

In DevOps, Linux is widely used for:

* Application servers
* Web servers
* Database servers
* Docker hosts
* Kubernetes nodes
* Jenkins servers
* AWS EC2 instances

---

## Q2. What is the difference between Linux and Unix?

**Answer:**

Unix is an operating-system family originally developed at AT&T Bell Labs, while Linux is an open-source Unix-like operating system kernel created by Linus Torvalds.

Linux is widely available through distributions such as:

* Ubuntu
* Amazon Linux
* RHEL
* CentOS
* Debian

---

## Q3. What is a Linux distribution?

**Answer:**

A Linux distribution is a complete operating system built around the Linux kernel.

Examples:

```text
Ubuntu
Debian
Amazon Linux
Red Hat Enterprise Linux
Rocky Linux
AlmaLinux
```

A distribution generally includes:

* Linux kernel
* Package manager
* System utilities
* Libraries
* Shell
* System services

---

## Q4. What is the Linux kernel?

**Answer:**

The kernel is the core component of Linux.

It manages communication between hardware and software.

Main responsibilities include:

* CPU management
* Memory management
* Process management
* Device management
* Networking
* File-system management

You can check the kernel version using:

```bash
uname -r
```

---

## Q5. What is a shell?

**Answer:**

A shell is a command-line interface that allows users to interact with the Linux operating system.

Common shells include:

```text
bash
sh
zsh
fish
```

To check the current shell:

```bash
echo $SHELL
```

---

## Q6. What is the difference between `pwd`, `ls`, and `cd`?

**Answer:**

### `pwd`

Shows the current working directory.

```bash
pwd
```

### `ls`

Lists files and directories.

```bash
ls
ls -l
ls -la
```

### `cd`

Changes the current directory.

```bash
cd /var/log
```

---

## Q7. What are Linux file permissions?

**Answer:**

Linux permissions determine who can:

* Read
* Write
* Execute

a file or directory.

Example:

```text
-rwxr-xr--
```

The permissions are divided into:

```text
Owner
Group
Others
```

Permission values:

```text
r = 4
w = 2
x = 1
```

For example:

```bash
chmod 755 script.sh
```

means:

```text
Owner  = rwx = 7
Group  = r-x = 5
Others = r-x = 5
```

---

## Q8. What is the difference between `chmod` and `chown`?

**Answer:**

`chmod` changes file permissions.

```bash
chmod 755 script.sh
```

`chown` changes file ownership.

```bash
chown ubuntu:ubuntu file.txt
```

So:

```text
chmod → permissions
chown → ownership
```

---

## Q9. What is a process in Linux?

**Answer:**

A process is a running instance of a program.

For example, when you execute:

```bash
nginx
```

Linux creates a process for it.

You can view processes using:

```bash
ps aux
```

or:

```bash
top
```

---

## Q10. What is PID?

**Answer:**

PID stands for **Process ID**.

Every running process in Linux has a unique process ID.

Example:

```bash
ps aux
```

Output may contain:

```text
root    1234    1    0.1    nginx
```

Here:

```text
1234 = PID
```

You can terminate a process using:

```bash
kill 1234
```

---

## Q11. What is the difference between `kill` and `kill -9`?

**Answer:**

`kill` normally sends a termination signal to a process.

```bash
kill PID
```

`kill -9` sends `SIGKILL`, which immediately terminates the process.

```bash
kill -9 PID
```

A good troubleshooting practice is to try normal termination first:

```bash
kill PID
```

and use:

```bash
kill -9 PID
```

only when necessary.

---

## Q12. How do you check CPU and memory utilization in Linux?

**Answer:**

Common commands are:

```bash
top
```

```bash
htop
```

```bash
free -h
```

```bash
uptime
```

For CPU information:

```bash
lscpu
```

For memory:

```bash
free -h
```

---

## Q13. How do you check disk usage?

**Answer:**

Use:

```bash
df -h
```

This displays filesystem-level disk usage.

To find the size of directories:

```bash
du -sh /var/*
```

To find large files:

```bash
du -ah /var | sort -rh | head
```

---

## Q14. What is the difference between `df` and `du`?

**Answer:**

`df` shows filesystem disk usage.

```bash
df -h
```

`du` shows disk usage of files and directories.

```bash
du -sh /var/log
```

Simple way to remember:

```text
df → filesystem
du → directory/file
```

---

## Q15. What is SSH?

**Answer:**

SSH stands for **Secure Shell**.

It is used to securely connect to remote Linux servers.

Example:

```bash
ssh ubuntu@192.168.1.10
```

In AWS EC2:

```bash
ssh -i my-key.pem ubuntu@EC2_PUBLIC_IP
```

SSH normally uses port:

```text
22
```

---

## Q16. What is a Linux service?

**Answer:**

A service is a background process that provides a specific function.

Examples:

```text
nginx
ssh
docker
jenkins
```

On systems using `systemd`, services can be managed using:

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
```

---

## Q17. How do you check Linux logs?

**Answer:**

Common log locations include:

```text
/var/log/
```

For example:

```bash
ls /var/log
```

To view a log:

```bash
cat /var/log/syslog
```

To monitor a log in real time:

```bash
tail -f /var/log/syslog
```

For systemd services:

```bash
journalctl
```

For a specific service:

```bash
journalctl -u nginx
```

---

## Q18. What is `grep`?

**Answer:**

`grep` is used to search for text patterns inside files or command output.

Example:

```bash
grep "ERROR" application.log
```

Case-insensitive search:

```bash
grep -i "error" application.log
```

Search recursively:

```bash
grep -r "database" /etc/
```

---

## Q19. What is `find`?

**Answer:**

`find` is used to search for files and directories.

Example:

```bash
find /var/log -name "*.log"
```

Find files larger than 100 MB:

```bash
find / -type f -size +100M
```

Find files modified in the last 1 day:

```bash
find /var/log -type f -mtime -1
```

---

## Q20. What is a cron job?

**Answer:**

Cron is used to schedule tasks automatically in Linux.

To edit cron jobs:

```bash
crontab -e
```

Example:

```text
0 2 * * * /home/ubuntu/backup.sh
```

This runs the script every day at 2:00 AM.

Cron format:

```text
Minute Hour Day Month Weekday Command
```

---

# 🔥 Part 2 — 30 Scenario-Based Linux Interview Questions

## Q21. Scenario: Linux server CPU utilization is 100%. How will you troubleshoot?

**Answer:**

I would troubleshoot it step-by-step.

### Step 1: Check CPU usage

```bash
top
```

or:

```bash
htop
```

### Step 2: Identify the process consuming CPU

```bash
ps aux --sort=-%cpu | head
```

### Step 3: Check whether the process is expected

If it is an application process, I would check its logs.

### Step 4: Check system load

```bash
uptime
```

### Step 5: Investigate the root cause

Possible causes:

* Infinite loop
* Traffic spike
* Memory pressure
* Application bug
* Background process

### Step 6: Take corrective action

Depending on the root cause, I may restart the service, scale the application, or fix the application issue.

---

## Q22. Scenario: Your Linux server disk is 100% full. What will you do?

**Answer:**

First I would confirm disk utilization:

```bash
df -h
```

Then identify large directories:

```bash
du -sh /* 2>/dev/null
```

Check `/var`:

```bash
du -sh /var/*
```

Find large files:

```bash
find /var -type f -size +500M
```

I would then investigate:

* Application logs
* Docker images
* Docker containers
* Temporary files
* Old backups

For logs, I would avoid blindly deleting active logs and instead use the application's log-rotation mechanism.

Finally:

```bash
df -h
```

to verify that sufficient space has been recovered.

---

## Q23. Scenario: A Linux server is running very slowly. How will you troubleshoot it?

**Answer:**

I would check the system in this order:

```bash
uptime
top
free -h
df -h
```

Then check:

```bash
iostat
```

if available.

I would investigate:

* CPU usage
* Memory usage
* Swap usage
* Disk utilization
* Disk I/O
* Network problems
* Number of running processes

Then I would identify the resource bottleneck and investigate the responsible process or service.

---

## Q24. Scenario: SSH connection to a Linux server is failing. How will you troubleshoot?

**Answer:**

I would check:

### 1. Network connectivity

```bash
ping SERVER_IP
```

### 2. SSH port

```bash
nc -zv SERVER_IP 22
```

### 3. SSH service

If console access is available:

```bash
systemctl status ssh
```

or:

```bash
systemctl status sshd
```

### 4. Firewall

Check:

```bash
sudo ufw status
```

### 5. SSH configuration

```bash
sudo cat /etc/ssh/sshd_config
```

### 6. Authentication

Check:

```bash
ls -la ~/.ssh
```

and verify the correct private key and permissions.

For AWS EC2, I would also check:

* Security Group
* Network ACL
* Route table
* Instance status

---

## Q25. Scenario: A web server is not responding. What will you check?

**Answer:**

First I would test the server locally:

```bash
curl http://localhost
```

Then check the web-server service:

```bash
systemctl status nginx
```

Check whether it is listening:

```bash
ss -tulpn | grep :80
```

Then check logs:

```bash
tail -f /var/log/nginx/error.log
```

I would also check:

```bash
df -h
free -h
top
```

If the server is accessed remotely, I would additionally check firewall and network configuration.

---

## Q26. Scenario: Nginx service has stopped. How will you troubleshoot?

**Answer:**

First:

```bash
systemctl status nginx
```

Check logs:

```bash
journalctl -u nginx
```

Check configuration:

```bash
nginx -t
```

If the configuration is valid:

```bash
systemctl restart nginx
```

Then verify:

```bash
systemctl status nginx
curl http://localhost
```

If it stops again, I would investigate the actual root cause instead of repeatedly restarting it.

---

## Q27. Scenario: A service starts successfully but stops after a few seconds. What will you do?

**Answer:**

I would check:

```bash
systemctl status service-name
```

Then:

```bash
journalctl -u service-name
```

I would investigate:

* Application errors
* Configuration errors
* Missing environment variables
* Permission problems
* Port conflicts
* Dependency failures
* Resource exhaustion

I would also check whether another process is using the required port:

```bash
ss -tulpn
```

---

## Q28. Scenario: You get "Permission denied" while executing a script. What will you check?

**Answer:**

First:

```bash
ls -l script.sh
```

If execute permission is missing:

```bash
chmod +x script.sh
```

Then:

```bash
./script.sh
```

If the problem is ownership:

```bash
chown user:user script.sh
```

I would also check:

* Parent directory permissions
* Mount options
* SELinux/AppArmor where applicable
* Script interpreter/shebang

---

## Q29. Scenario: A user cannot access a file, but you can. How will you troubleshoot?

**Answer:**

First check ownership and permissions:

```bash
ls -l file.txt
```

Then check the user's identity:

```bash
id username
```

Check directory permissions:

```bash
namei -l /path/to/file.txt
```

The user may lack permission on one of the parent directories.

I would also check ACLs:

```bash
getfacl file.txt
```

---

## Q30. Scenario: Memory utilization is 95%. What will you do?

**Answer:**

First:

```bash
free -h
```

Then identify memory-consuming processes:

```bash
ps aux --sort=-%mem | head
```

or:

```bash
top
```

I would check whether the memory usage is:

* Application-related
* Cache
* Swap-related
* Due to a memory leak

I would investigate the application before killing processes.

---

## Q31. Scenario: A process is consuming too much memory. What will you do?

**Answer:**

Identify the process:

```bash
ps aux --sort=-%mem | head
```

Check the process:

```bash
ps -p PID -f
```

Then check application logs and behavior.

If necessary, I may restart the service according to the operational procedure.

For a production system, I would also investigate why memory usage increased instead of simply killing the process.

---

## Q32. Scenario: You accidentally deleted an important file. What will you do?

**Answer:**

First, I would stop making changes that could overwrite the deleted data.

Then I would determine whether:

* The file exists in a backup
* The file is tracked in Git
* The application has another copy
* A snapshot is available
* The filesystem has a recovery mechanism

For production systems, recovery should follow the organization's backup and disaster-recovery procedure.

---

## Q33. Scenario: A log file is continuously growing and consuming disk space. What will you do?

**Answer:**

First identify the file:

```bash
du -sh /var/log/*
```

Check what is writing to it.

Then investigate log rotation:

```bash
cat /etc/logrotate.conf
```

and:

```bash
ls /etc/logrotate.d/
```

I would configure appropriate log rotation and retention rather than simply deleting the active log file.

Then verify disk space:

```bash
df -h
```

---

## Q34. Scenario: A server has a very high load average. What does it mean?

**Answer:**

Load average represents the amount of work waiting for CPU or certain uninterruptible system resources.

Check it with:

```bash
uptime
```

or:

```bash
top
```

I would compare the load average with the number of CPU cores:

```bash
nproc
```

Then investigate:

* CPU saturation
* Disk I/O
* Blocked processes
* Memory pressure

A high load average does not automatically mean CPU utilization is high.

---

## Q35. Scenario: A process is stuck and cannot be terminated normally. What will you do?

**Answer:**

First:

```bash
kill PID
```

Check whether it is still running:

```bash
ps -p PID
```

If it does not terminate and there is a valid reason to force termination:

```bash
kill -9 PID
```

If even `kill -9` does not work, I would investigate whether the process is stuck in an uninterruptible kernel state, often related to I/O.

---

## Q36. Scenario: A port is already in use when you start an application. How will you troubleshoot?

**Answer:**

Find which process is using the port:

```bash
ss -ltnp | grep :8080
```

or:

```bash
lsof -i :8080
```

Then identify the process:

```bash
ps -p PID -f
```

I would determine whether:

* The existing process should be running
* The application should use another port
* A previous instance is still running

I would not blindly kill the process without understanding its purpose.

---

## Q37. Scenario: You need to find all files larger than 1 GB. What command will you use?

**Answer:**

```bash
find / -type f -size +1G 2>/dev/null
```

To investigate a particular filesystem:

```bash
find /var -type f -size +1G 2>/dev/null
```

---

## Q38. Scenario: You need to find the top 10 CPU-consuming processes. What command will you use?

**Answer:**

```bash
ps aux --sort=-%cpu | head -11
```

The first line contains the header, so the command displays approximately the top 10 processes.

Another option is:

```bash
top
```

---

## Q39. Scenario: You need to find the top 10 memory-consuming processes. What will you use?

**Answer:**

```bash
ps aux --sort=-%mem | head -11
```

I can also use:

```bash
top
```

and sort processes by memory usage.

---

## Q40. Scenario: An application cannot connect to another server. How will you troubleshoot?

**Answer:**

I would troubleshoot layer by layer.

### Check DNS

```bash
nslookup hostname
```

or:

```bash
dig hostname
```

### Check connectivity

```bash
ping hostname
```

### Check the application port

```bash
nc -zv hostname 8080
```

### Check routing

```bash
ip route
```

Then investigate:

* Firewall
* Security groups
* Network ACLs
* Application configuration
* Destination service
* DNS
* Authentication

---

## Q41. Scenario: DNS resolution is not working on a Linux server. What will you check?

**Answer:**

First:

```bash
nslookup example.com
```

or:

```bash
dig example.com
```

Check DNS configuration:

```bash
cat /etc/resolv.conf
```

Check network configuration:

```bash
ip addr
ip route
```

Then verify connectivity to the configured DNS server.

I would also check whether the problem affects only one hostname or all DNS queries.

---

## Q42. Scenario: A scheduled cron job is not running. How will you troubleshoot?

**Answer:**

First check the cron entry:

```bash
crontab -l
```

Check cron service:

```bash
systemctl status cron
```

or on some distributions:

```bash
systemctl status crond
```

Then verify:

* Script permissions
* Script path
* Environment variables
* Absolute paths
* Output/error logs
* Cron syntax

I would also manually execute the script:

```bash
/path/to/script.sh
```

to determine whether the script itself works.

---

## Q43. Scenario: A script works manually but fails when executed by cron. Why?

**Answer:**

Cron runs with a limited environment.

The script may depend on:

* `PATH`
* Environment variables
* Working directory
* Relative paths
* User permissions

I would use absolute paths.

For example:

```bash
/usr/bin/docker
```

instead of:

```bash
docker
```

I would also explicitly define required environment variables and log the script output.

---

## Q44. Scenario: You need to monitor a log file in real time. Which command will you use?

**Answer:**

```bash
tail -f application.log
```

A more convenient option is:

```bash
tail -n 100 -f application.log
```

This displays the last 100 lines and continues monitoring new entries.

---

## Q45. Scenario: You need to search "ERROR" across multiple application logs. What will you do?

**Answer:**

Use:

```bash
grep -r "ERROR" /var/log/myapp/
```

Case-insensitive:

```bash
grep -ri "error" /var/log/myapp/
```

With line numbers:

```bash
grep -rin "error" /var/log/myapp/
```

---

## Q46. Scenario: A server has many zombie processes. What will you do?

**Answer:**

First identify them:

```bash
ps aux | awk '$8 ~ /Z/ {print}'
```

A zombie process has already terminated, but its parent has not collected its exit status.

I would identify the parent process:

```bash
ps -o pid,ppid,state,cmd -p PID
```

Then investigate the parent application.

The correct solution is usually to fix or restart the parent process so it properly reaps child processes, rather than trying to kill the zombie itself.

---

## Q47. Scenario: An application is generating too many processes. How will you investigate?

**Answer:**

Check the process count:

```bash
ps -e --no-headers | wc -l
```

Find processes by application:

```bash
ps aux | grep application-name
```

Check process limits:

```bash
ulimit -a
```

I would investigate whether the application is:

* Creating child processes continuously
* Failing to clean up processes
* Receiving excessive traffic
* Misconfigured

Then I would fix the root cause and consider appropriate process/resource limits.

---

## Q48. Scenario: You need to copy a file from one Linux server to another. Which command will you use?

**Answer:**

Use `scp`:

```bash
scp file.txt ubuntu@SERVER_IP:/home/ubuntu/
```

For directories:

```bash
scp -r mydirectory ubuntu@SERVER_IP:/home/ubuntu/
```

For larger or repeated transfers, `rsync` is often more efficient:

```bash
rsync -av mydirectory/ ubuntu@SERVER_IP:/home/ubuntu/mydirectory/
```

---

## Q49. Scenario: You need to check which process is listening on port 80. What will you do?

**Answer:**

Use:

```bash
ss -ltnp | grep :80
```

or:

```bash
lsof -i :80
```

This helps identify the application and PID using port 80.

For example, the process might be:

```text
nginx
apache2
```

---

## Q50. Scenario: A production Linux server has suddenly become unavailable. Explain your troubleshooting approach.

**Answer:**

I would follow a structured incident-response process.

### Step 1 — Confirm the problem

Check:

```bash
ping SERVER_IP
```

and:

```bash
nc -zv SERVER_IP 22
```

if appropriate.

### Step 2 — Check infrastructure

If it is a cloud server, check:

* Instance status
* Network
* Security rules
* Load balancer
* Monitoring alerts

### Step 3 — Access the server

If console access is available, check:

```bash
uptime
top
free -h
df -h
```

### Step 4 — Check services

```bash
systemctl --failed
systemctl status nginx
```

### Step 5 — Check logs

```bash
journalctl -xe
```

and application logs.

### Step 6 — Identify the root cause

Possible causes include:

```text
CPU exhaustion
Memory exhaustion
Disk full
Network failure
Application crash
Configuration change
Security issue
Hardware/infrastructure failure
```

### Step 7 — Restore service

Apply the safest corrective action according to the incident procedure.

### Step 8 — Verify

Test:

```bash
curl http://localhost
```

and perform an external health check.

### Step 9 — Prevent recurrence

After recovery:

* Document the incident
* Identify root cause
* Improve monitoring
* Configure alerts
* Fix the underlying issue
* Add automation where appropriate

---

# 🎯 Quick Linux Commands for Interviews

## System Information

```bash
uname -a
hostname
hostnamectl
uptime
whoami
id
```

## CPU

```bash
top
htop
lscpu
nproc
```

## Memory

```bash
free -h
```

## Disk

```bash
df -h
du -sh *
```

## Processes

```bash
ps aux
ps aux --sort=-%cpu
ps aux --sort=-%mem
```

## Networking

```bash
ip addr
ip route
ss -tulpn
ping
curl
```

## Files

```bash
ls
cp
mv
rm
find
locate
```

## Text Processing

```bash
cat
less
head
tail
grep
awk
sed
cut
sort
uniq
```

## Services

```bash
systemctl status
systemctl start
systemctl stop
systemctl restart
systemctl enable
```

## Logs

```bash
journalctl
tail -f
grep
```

## Permissions

```bash
chmod
chown
chgrp
umask
```

## Archive

```bash
tar
gzip
gunzip
zip
unzip
```

---

# 🧠 Linux Troubleshooting Cheat Sheet

| Problem             | First Commands                                              |
| ------------------- | ----------------------------------------------------------- |
| High CPU            | `top`, `ps aux --sort=-%cpu`                                |
| High Memory         | `free -h`, `ps aux --sort=-%mem`                            |
| Disk Full           | `df -h`, `du -sh`                                           |
| Service Down        | `systemctl status`, `journalctl`                            |
| Port Conflict       | `ss -ltnp`, `lsof -i`                                       |
| SSH Failure         | Network → port 22 → SSH service → firewall → authentication |
| DNS Issue           | `dig`, `nslookup`, `/etc/resolv.conf`                       |
| Permission Issue    | `ls -l`, `id`, `namei`, `getfacl`                           |
| Application Failure | Logs → process → port → dependencies                        |
| High Load           | `uptime`, `top`, `nproc`, I/O checks                        |

---

# 🎤 Interview Tip

When an interviewer gives you a Linux production scenario, **don't immediately give one command**.

Use this structure:

```text
Understand the problem
        ↓
Check system health
        ↓
Identify the affected resource
        ↓
Check logs
        ↓
Find the root cause
        ↓
Apply the fix
        ↓
Verify the fix
        ↓
Prevent recurrence
```

This demonstrates **real troubleshooting ability**, rather than just memorized Linux commands.

---

# ✅ Linux Interview Checklist

* [ ] Linux fundamentals
* [ ] File system
* [ ] Permissions
* [ ] Users & groups
* [ ] Processes
* [ ] CPU & memory
* [ ] Disk management
* [ ] Networking
* [ ] SSH
* [ ] Services
* [ ] Logs
* [ ] Cron
* [ ] Shell commands
* [ ] Troubleshooting
* [ ] Production scenarios

---

**Total: 50 Linux Interview Questions**

```text
20 Normal Questions
+
30 Scenario-Based Questions
=
50 Linux Interview Questions
```

