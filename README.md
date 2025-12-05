# 🖤 NU–Information Exchange System

<p align="center">
  <img src="https://i.imgur.com/Uk0H3UL.png" width="100%" alt="Dark Banner"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Language-C++11-1e1e1e?style=for-the-badge&logo=c%2B%2B&logoColor=00599C">
  <img src="https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-1e1e1e?style=for-the-badge&logo=windows-terminal&logoColor=white">
  <img src="https://img.shields.io/badge/Networking-TCP%20%7C%20UDP-1e1e1e?style=for-the-badge&logo=protocols&logoColor=white">
  <img src="https://img.shields.io/badge/Status-Stable-1e1e1e?style=for-the-badge&logo=vercel&logoColor=00ff99">
</p>

---

# 🌙 Overview

A modern **TCP + UDP based distributed communication system** for connecting NU campuses.
This is the **official troubleshooting and diagnostics guide**, optimized for **dark mode users**, maintainers, and developers.

---

# ❗ Common Errors & Fixes

---

## 🔧 1. **Invalid Campus Choice ("Invalid choice!")**

You must enter a **number**, not text.

| Campus   | Number |
| -------- | ------ |
| Lahore   | `1`    |
| Karachi  | `2`    |
| Peshawar | `3`    |
| Chiniot  | `4`    |
| Multan   | `5`    |

💡 **Fix:** Enter only digits `1–5`.

---

## 🌐 2. **Cannot Connect to Server ("Failed to connect")**

Start server *first*:

```bash
./server
```

Wait for:

```
TCP Server listening on port 8080
UDP Server listening on port 8081
```

Then run client:

```bash
./client
```

---

## 🔒 3. **Port Already in Use**

### Linux/macOS

```bash
sudo lsof -ti:8080 | xargs kill -9
sudo lsof -ti:8081 | xargs kill -9
```

### Windows

```bash
netstat -ano | findstr :8080
taskkill /PID <PID> /F
```

---

## 🔐 4. Authentication Failed

Check that server credentials match:

```cpp
map<string, string> campusCredentials = {
    {"Lahore", "NU-LHR-123"},
    {"Karachi", "NU-KHI-123"},
};
```

Rebuild:

```bash
make clean
make all
```

---

## ⚙️ 5. Compilation Errors

### Missing `pthread`

```bash
-pthread
```

### Missing `winsock2.h` (Windows)

```bash
-lws2_32
```

### Missing thread header

```bash
-std=c++11
```

---

## ✉️ 6. Message Not Delivered

Check server status:

```
Admin> status
```

Should show all campuses **Online**.

Check logs:

```
Routed message from Lahore to Karachi
```

---

## 💥 7. Segmentation Fault

Use debugging build:

```bash
g++ -g -o client campus_client.cpp -std=c++11 -pthread
gdb ./client
```

---

## ⌨️ 8. Input Issues (Menu Broken)

Add cleanup after input:

```cpp
cin.ignore(numeric_limits::max(), '\n');
```

---

## ❤️ 9. Heartbeat Failing

Check UDP 8081.

Add debug:

```cpp
safeLog("[DEBUG] Sending heartbeat…");
```

---

## 📡 10. Broadcast Not Received

Allow UDP 8082:

```bash
sudo ufw allow 8082/udp
```

---

# 🧪 Diagnostics Checklist

✔ Compilation successful
✔ Server shows TCP/UDP listening
✔ Client selects valid number
✔ Authentication successful
✔ Campuses online in `status`
✔ Heartbeats visible
✔ Broadcasts received
✔ Exits cleanly

---

# 🔍 Networking Test Commands

### Check Ports

**Linux/macOS**

```bash
netstat -an | grep 8080
```

**Windows**

```bash
netstat -an | findstr 8080
```

### Telnet Connectivity

```bash
telnet 127.0.0.1 8080
```

---

# ⚙️ Debug Mode

Add logs:

```cpp
safeLog("DEBUG: Received bytes=" + to_string(bytes));
```

Compile with debug flags:

```bash
make debug
```

---

# 🌟 Best Practices

✔ Start server before all clients
✔ Avoid Ctrl+C (use proper exit)
✔ Keep ports clean
✔ Test with 2 clients first
✔ Monitor server logs continuously

---

# ✅ System Working Checklist

A fully working system will show:

* TCP & UDP servers active
* Client authenticated
* Campus online status
* Routing logs for each message
* Heartbeat every 10 seconds
* Successful broadcast reach
* No crashes or segmentation faults

---

# 🖼 Bonus: Add this as your GitHub Project Header

```md
![NU Info Exchange Banner](https://i.imgur.com/Uk0H3UL.png)
