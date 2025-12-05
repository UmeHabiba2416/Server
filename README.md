Troubleshooting Guide
NU-Information Exchange System
🔧 Common Errors and Solutions
Error 1: "Invalid choice!" at Campus Selection
Problem: You entered text (like "Lahore") instead of a number.
What you did:
Enter campus number: Lahore ❌
What you should do:
Enter campus number: 1 ✅
Solution:
Enter only the NUMBER (1, 2, 3, 4, or 5)
Do NOT type the campus name
Example: For Lahore, type 1 and press Enter
Quick Reference:
1 = Lahore
2 = Karachi
3 = Peshawar
4 = Chiniot
5 = Multan
Error 2: "Failed to connect to server"
Possible Causes:
Server is not running
Wrong IP address
Port is blocked
Solution:
Step 1: Make sure server is running first
# Terminal 1: Start server ./server
Step 2: Wait for these messages
[2025-12-01 10:30:00] TCP Server listening on port 8080 [2025-12-01 10:30:00] UDP Server listening on port 8081
Step 3: Then start client
# Terminal 2: Start client ./client
Error 3: "Port already in use"
Error Message:
TCP Bind failed
Cause: Another process is using ports 8080 or 8081
Solution:
Linux/macOS:
# Find and kill process using port 8080 sudo lsof -ti:8080 | xargs kill -9 # Find and kill process using port 8081 sudo lsof -ti:8081 | xargs kill -9
Windows:
# Find process using port 8080 netstat -ano | findstr :8080 # Kill the process (replace PID with actual number) taskkill /PID <PID> /F
Error 4: "Authentication failed"
Possible Causes:
Wrong campus selected
Modified credentials in code
Server and client credentials don't match
Solution:
Check your credentials match:
Server credentials in central_server.cpp:
map<string, string> campusCredentials = { {"Lahore", "NU-LHR-123"}, {"Karachi", "NU-KHI-123"}, // ... etc };
Client should send exactly matching campus name and password.
Fix: Recompile both server and client after any changes:
make clean make all
Error 5: Compilation Errors
Error: "undefined reference to pthread_create"
Solution:
Add -pthread flag:
g++ -o client campus_client.cpp -std=c++11 -pthread
Error: "winsock2.h not found" (Windows)
Solution:
Add Windows socket library:
g++ -o client.exe campus_client.cpp -lws2_32 -std=c++11 -pthread
Error: "fatal error: thread: No such file or directory"
Solution:
Use C++11 or later:
g++ -o client campus_client.cpp -std=c++11 -pthread
Error 6: Messages Not Being Received
Symptoms:
Message sent from Lahore
Nothing appears at Karachi
Troubleshooting Steps:
Step 1: Check if both campuses are connected
# In server terminal Admin> status
Should show:
Campus: Lahore | Status: Online Campus: Karachi | Status: Online
Step 2: Check server logs
Look for messages like:
[2025-12-01 10:30:00] Message from Lahore: Lahore|Karachi|... [2025-12-01 10:30:00] Routed message from Lahore to Karachi
Step 3: If one campus shows "Offline"
That campus client needs to be restarted
Check network connection
Error 7: "Segmentation fault" or Crash
Possible Causes:
Buffer overflow
Null pointer access
Thread synchronization issue
Quick Fix:
# Recompile with debug symbols g++ -g -o client campus_client.cpp -std=c++11 -pthread # Run with debugger gdb ./client
Prevention:
Always check return values
Validate buffer sizes
Use mutex locks for shared data
Error 8: Client Input Validation Issues
Problem: After sending message, menu doesn't appear properly
Solution: Updated client code includes proper cin.ignore() calls
Workaround if still having issues:
// After any cin >> variable cin.ignore(numeric_limits<streamsize>::max(), '\n');
Error 9: Heartbeat Not Working
Symptoms:
Client connects but server doesn't show heartbeat
Status never updates
Check:
UDP port 8081 is not blocked
Firewall allows UDP traffic
Heartbeat thread is running
Debug:
Add this to client code to verify heartbeat is sending:
safeLog("[DEBUG] Sending heartbeat to " + SERVER_IP);
Error 10: Broadcast Not Received
Symptoms:
Admin sends broadcast
Clients don't receive it
Common Issues:
Wrong UDP port
Client UDP receiver should bind to port 8082
Check CLIENT_UDP_PORT constant
Firewall blocking
# Linux: Allow UDP sudo ufw allow 8082/udp
Socket not bound
Make sure receiveBroadcasts() thread is running
Check bind() return value
🎯 Quick Diagnostics Checklist
Run through this checklist when something isn't working:
[ ] Compilation successful (no errors or warnings)
[ ] Server started (shows "listening on port" messages)
[ ] Ports available (8080, 8081, 8082 not in use)
[ ] Client enters NUMBER (1-5, not campus name)
[ ] Authentication successful (client shows success message)
[ ] Server shows connection (check with status command)
[ ] Both campuses online (before sending messages)
[ ] Firewall allows traffic (TCP 8080, UDP 8081/8082)
📋 Testing Commands
Test Server is Running
# Linux/Mac netstat -an | grep 8080 netstat -an | grep 8081 # Windows netstat -an | findstr 8080 netstat -an | findstr 8081
Should show LISTENING state.
Test Client Connection
# Try connecting with telnet telnet 127.0.0.1 8080
If server is running, connection should succeed.
🔍 Debug Mode
Enable Detailed Logging
Server side:
Add more log statements:
safeLog("DEBUG: Received " + to_string(bytesReceived) + " bytes"); safeLog("DEBUG: Message content: " + string(buffer));
Client side:
safeLog("[DEBUG] Connecting to " + SERVER_IP + ":" + to_string(TCP_PORT)); safeLog("[DEBUG] Sending: " + message);
Compile with Debug Info
make debug # or g++ -g -DDEBUG -o client campus_client.cpp -std=c++11 -pthread
💡 Best Practices
Always start server first
./server # Terminal 1 # Wait for "listening" messages ./client # Terminal 2
Use status command frequently
Admin> status
Check logs when things fail
Server terminal shows all activity
Look for error messages
Test with 2 clients first
Before trying 5 clients
Verify basic messaging works
Graceful shutdown
Use menu option 3 (Exit)
Don't use Ctrl+C (kills threads abruptly)
📞 Still Having Issues?
Gather Information
# System info uname -a # Linux/Mac gcc --version # Compiler version netstat -tulpn | grep 808 # Check ports
Create Test Case
Start server
Copy server output
Start client
Copy client output
Document exact steps taken
Note error messages
Common Fixes Summary
Problem Quick Fix "Invalid choice" Enter NUMBER (1-5), not text "Connection refused" Start server first "Port in use" Kill existing process "Auth failed" Check credentials match Message not received Verify both online with status Compilation error Add -pthread and -std=c++11
✅ Verification Steps
Everything is working correctly when:
✅ Server starts without errors
✅ Client accepts number input (1-5)
✅ "Authentication successful!" appears
✅ Server shows campus connected
✅ Messages sent from one campus appear at another
✅ status command shows campuses online
✅ Heartbeat logs appear every 10 seconds
✅ Broadcasts reach all clients
✅ Can exit cleanly without crashes
Remember: Most issues are solved by:
Starting server before clients
Entering numbers, not text
Checking ports aren't in use
Verifying both campuses are connected before messaging
