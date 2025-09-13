# System Monitoring with Bash Script and Cron Jobs  

## 1. Introduction  

### What is Bash Scripting?  
Bash is a tool in Linux that lets us write small programs (called scripts). These scripts can run many commands automatically. Instead of typing the same commands again and again, we save them in a file and run them whenever needed. This makes it easier to manage tasks like checking CPU, memory, and disk usage.  

### What are Cron Jobs?  
Cron is like an alarm clock for Linux. It allows us to run tasks at fixed times or after regular intervals without doing them manually. For example, we can make a script run every 2 minutes, every day at midnight, or once an hour. It’s mostly used for things like monitoring, backups, cleaning files, or system maintenance.  

---

## 2. Objective  
The main goals of this project were:  
1. Write a script (`system_usage.sh`) to check CPU, memory, and disk usage.  
2. Save the output in a file called `system_report.log`.  
3. Set up a cron job to run the script every 2 minutes automatically.  
4. Add another cron job to clear the log file every hour so it doesn’t become too large.  

---

## 3. Steps Performed  

### Step 1: Write the Script  
A script named `system_usage.sh` was created. It collects system details and saves them in a log file:  

```bash
#!/bin/bash
echo "----- System Report: $(date) -----" >> /path/to/system_report.log

echo "CPU Usage:" >> /path/to/system_report.log
top -bn1 | grep "Cpu(s)" >> /path/to/system_report.log

echo "Memory Usage:" >> /path/to/system_report.log
free -h >> /path/to/system_report.log

echo "Disk Usage:" >> /path/to/system_report.log
df -h >> /path/to/system_report.log

echo "-----------------------------------" >> /path/to/system_report.log
```

### Step 2: Make the Script Executable  
```bash
chmod +x system_usage.sh
```

### Step 3: Schedule with Cron  

**Run script every 2 minutes:**  
```bash
crontab -e
*/2 * * * * /bin/bash /path/to/system_usage.sh
```

**Clear log every hour:**  
```bash
* */1 * * * > /path/to/system_report.log
```

---

## 4. Example Log Output  

This is how the log file looks after the script runs:  

```
----- System Report: Wed Sep 10 09:15:01 2025 -----
CPU Usage:
%Cpu(s):  3.0 us,  1.0 sy,  0.0 ni, 95.5 id,  0.5 wa,  0.0 hi,  0.0 si,  0.0 st

Memory Usage:
             total        used        free      shared  buff/cache   available
Mem:           7.7G        2.5G        3.1G        150M        2.1G        5.0G
Swap:          2.0G          0B        2.0G

Disk Usage:
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
```

---

## 5. Issues Faced During Implementation  

1. **Permission Problems**  
   - The script did not run at first because it wasn’t marked as executable. Running `chmod +x` fixed it.  
 
2. **Cron Job Not Running**  
   - In some cases, the cron service was not active. It had to be started with:  
     ```bash
     sudo systemctl enable cron
     sudo systemctl start cron
     ```  
