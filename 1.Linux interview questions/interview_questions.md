
# 🔟 Top Scenario-Based Linux Interview Questions (with Easy Explanations)

This guide covers **10 real-world Linux interview questions** that test not just commands but *practical problem-solving*.  
Format: **Scenario → Steps → Concept → Best Practice/Pitfall**  

---

## 1. A process is using too much CPU. How will you find and stop it?
- **Step 1:** Use `top` or `htop` → shows live CPU/memory usage.  
- **Step 2:** Find the process ID (PID).  
- **Step 3:** Kill it → `kill -9 <PID>`.  

**Concept:** Monitoring (top/htop), process control (kill).  
**Tip:** Don’t kill blindly—check if it’s a system-critical process first.

---

## 2. Your server disk is full. How will you find the files consuming space?
- `df -h` → check disk usage (human-readable).  
- `du -sh *` → show size of each item in current dir.  
- `du -sh /var/* | sort -h` → find largest folder.  

**Concept:** Disk usage analysis.  
**Pitfall:** Deleting logs won’t free space until the process writing to them is restarted.

---

## 3. A service (e.g., Apache/Nginx) is down. How will you troubleshoot?
- `systemctl status apache2` → check if running.  
- Check logs: `/var/log/apache2/error.log`.  
- Restart: `systemctl restart apache2`.  

**Concept:** Systemd + logs.  
**Tip:** If it fails, check port conflicts → `ss -tulnp`.

---

## 4. You can’t connect to a server via SSH. How do you fix it?
- `systemctl status sshd` → is SSH running?  
- `ss -tuln | grep 22` → check if port 22 is open.  
- Firewall check: `ufw status` or `iptables -L`.  

**Concept:** Networking + firewall.  
**Tip:** Wrong permissions on `~/.ssh/authorized_keys` can also block login.

---

## 5. A script is not running as expected. How do you debug it?
- Make executable: `chmod +x script.sh`.  
- Run in debug mode: `bash -x script.sh`.  
- Check error codes: `echo $?` (0 = success).  

**Concept:** Debugging + exit codes.  
**Tip:** Use `set -e` in scripts to stop on errors.

---

## 6. A file got deleted accidentally. Can you recover it?
- If process still using it: `lsof | grep deleted`.  
- Copy from `/proc/<PID>/fd/<fd-number>`.  
- If process not running → only backup can help.  

**Concept:** File deletion behavior.  
**Tip:** Always keep backups or snapshots.

---

## 7. Website is slow on server. How do you troubleshoot?
- Check CPU/memory: `top`.  
- Disk I/O: `iostat -x`.  
- Network: `ping`, `traceroute`.  
- Open connections: `netstat -an | grep ESTABLISHED`.  

**Concept:** Performance troubleshooting.  
**Tip:** Logs filling up or DB misconfig often cause slowdowns.

---

## 8. How will you find which process is using a specific port (e.g., 8080)?
- `lsof -i :8080`  
- OR `ss -tulnp | grep 8080`  

**Concept:** Port-to-process mapping.  
**Pitfall:** Some processes require `sudo` to view.

---

## 9. You created a cron job, but it’s not running. How do you check?
- Cron logs: `/var/log/cron` or `/var/log/syslog`.  
- User crontab: `crontab -l`.  
- Always use full paths in scripts.  

**Concept:** Cron troubleshooting.  
**Tip:** Redirect output for debugging:  
  ```bash
  * * * * * /path/script.sh >> /tmp/script.log 2>&1
