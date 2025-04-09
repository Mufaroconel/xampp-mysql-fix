## 🛠️ MySQL Not Starting in XAMPP – MacOS Fix

### 📌 **Issue Description**

When trying to start **MySQL via XAMPP**, the server displays:
```
Starting MySQL Database...
```
But then fails silently or immediately stops. This usually means **port 3306 is already in use** by another MySQL instance running outside of XAMPP.

---

### 🔍 **Root Cause**

- **Another MySQL server (Oracle MySQL)** was running on port `3306`, managed by macOS's `launchd` system.
- This service was **auto-starting on system boot**, preventing XAMPP from using the port.

Identified process:
```bash
sudo lsof -i :3306
```

Example output:
```
COMMAND   PID     USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
mysqld    32665   _mysql 30u  IPv6  ...         0t0     TCP *:mysql (LISTEN)
```

And checked who owns it:
```bash
sudo launchctl list | grep mysql
# Result:
com.oracle.oss.mysql.mysqld
```

---

## ✅ Resolution Steps

### 🔧 1. Disable the Auto-Starting MySQL Server

Stop and disable Oracle MySQL so it doesn't use port 3306:

```bash
sudo launchctl unload -w /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

### 🛑 2. Kill the Running MySQL Process

Find the running `mysqld` process:

```bash
sudo lsof -i :3306
```

Then kill it:

```bash
sudo kill -9 <PID>
# Replace <PID> with the actual PID shown in the previous step.
```

Confirm port 3306 is free:

```bash
sudo lsof -i :3306
# No output means success!
```

---

### ▶️ 3. Start MySQL via XAMPP

Now start the MySQL server through XAMPP’s control panel or terminal:

```bash
sudo /Applications/XAMPP/xamppfiles/bin/mysql.server start
```

---

## 🔁 Re-enabling Oracle MySQL (If Needed)

If you want to use the Oracle MySQL server again later:

### ✅ To Start and Auto-Enable:

```bash
sudo launchctl load -w /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

### ▶️ To Start It Just Once (No Auto Start):

```bash
sudo launchctl load /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

### ⛔ To Stop Again:

```bash
sudo launchctl unload -w /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

---

## 🧠 Tips

- Keep only **one MySQL service** active at a time to avoid conflicts.
- Always check port usage with:
```bash
sudo lsof -i :3306
```
- You can build a small toggle script for quickly switching between Oracle MySQL and XAMPP if needed.

---
