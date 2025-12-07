Departmental Messaging System – Client/Server (C++ • Winsock)
<p align="center"> <img src="https://i.imgur.com/Uk0H3UL.png" width="100%" alt="Dark Banner"/> </p> <p align="center"> <img src="https://img.shields.io/badge/Language-C++17-1e1e1e?style=for-the-badge&logo=c%2B%2B&logoColor=00599C"> <img src="https://img.shields.io/badge/Platform-Windows-1e1e1e?style=for-the-badge&logo=windows&logoColor=white"> <img src="https://img.shields.io/badge/Networking-TCP-1e1e1e?style=for-the-badge&logo=protocols&logoColor=white"> <img src="https://img.shields.io/badge/Status-Stable-1e1e1e?style=for-the-badge&logo=vercel&logoColor=00ff99"> </p>
🌙 Overview

A multi-client, department-based messaging system developed using C++ (Winsock2) & Multi-Threading.
Each user belongs to a department, and messages are routed only to members of the same department.

Includes:

🖥️ Server (Authentication, routing, heartbeat check, admin console)

💻 Client (Messaging UI, message receiving thread, heartbeat sender)

🗂 Project Architecture
/Project
│
├── server.cpp
├── client.cpp
├── README.md
└── data/
      ├── users.txt
      ├── logs.txt
      └── departments.txt

⚡ Features
💼 Client

Login with employee ID + password

Messages broadcasted within same department

Real-time receiving (threaded)

Heartbeat every 5 seconds

Clean text-based UI

Logout + safe exit

🖥️ Server

Handles multiple client connections

Authenticates from users.txt

Tracks active users by department

Routes messages department-wise

Logs all activity

Heartbeat timeout detection

Admin console:

View active clients

View logs

Kick user

Shutdown server

💽 User Authentication Format

data/users.txt

1001,password123,HR
1002,abc123,IT
2001,test987,Finance


Format:

employeeID,password,department

🧰 Build & Run
▶️ Compile Server
g++ server.cpp -o server -lws2_32

▶️ Compile Client
g++ client.cpp -o client -lws2_32

▶️ Run
./server
./client


Run multiple clients at once.

❗ Common Errors & Fixes
🔧 1. Authentication Failed

User not found in users.txt.

Fix by adding:

1001,password123,HR

🌐 2. Client Cannot Connect to Server

Start server first:

TCP server listening...


Then run client.

🔒 3. Port Already in Use

Windows:

netstat -ano | findstr :8080
taskkill /PID <pid> /F

✉️ 4. Message Not Delivering

User must be:

✔ Authenticated
✔ Online
✔ Same department

❤️ 5. Heartbeat Not Working

Check sending code:

safeLog("[DEBUG] Sending heartbeat...");

💥 6. Crashes / Segmentation Fault

Recompile with debug:

g++ -g server.cpp -o server -lws2_32

🧪 Diagnostics Checklist

✔ Client authenticates
✔ Server logs routing
✔ Heartbeats visible
✔ Messages delivered to same department
✔ No timeouts
✔ Admin console working
✔ Client exit clean

📡 Useful Commands
Windows Port Check
netstat -an | findstr 8080

⚙️ Debug Mode (Optional)

Add inside code:

safeLog("DEBUG: Bytes received = " + to_string(bytes));

🌟 Best Practices

✔ Always start server first
✔ Don’t use Ctrl+C (use proper logout)
✔ Use valid credentials
✔ Run at least 2 clients for testing
✔ Keep port 8080 free

👥 Contributors

Member 1 – Authentication & File Handling

Member 2 – Server Logic & Routing

Member 3 – Client UI & Messaging (you)

🖼 Add This as Your GitHub Header
![Department Messaging Banner](https://github.com/UmeHabiba2416/Server/blob/main/Screenshot%202025-12-07%20185539.png
)
