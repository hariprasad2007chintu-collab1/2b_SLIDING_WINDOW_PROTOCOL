# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
```
import socket

s = socket.socket()
s.bind(('localhost', 12345))
s.listen(1)

print("Server waiting for connection...")

conn, addr = s.accept()
print("Connected to client:", addr)

while True:
    data = conn.recv(1024).decode()

    if not data or data == "END":
        break

    frames = data.strip().split(",")

    for frame in frames:
        print("Received Frame:", frame)
        conn.sendall(("ACK" + frame + "\n").encode())

conn.close()
s.close()
```
### CLIENT PROGRAM
```
import socket

s = socket.socket()
s.connect(('localhost', 12345))

n = int(input("Enter the total number of frames: "))
w = int(input("Enter the window size: "))

frames = [str(i) for i in range(1, n + 1)]

i = 0

while i < n:
    window = frames[i:i + w]

    print("\nSending frames:", window)

    s.sendall(",".join(window).encode())

    for frame in window:
        ack = s.recv(1024).decode()
        print("Received:", ack.strip())

    i += w
    print("Sliding window...\n")

s.sendall(b"END")
print("All frames sent successfully.")

s.close()
```
## OUPUT
```
Enter the total number of frames: 5
Enter the window size: 3

Sending frames: ['1', '2', '3']
Received: ACK1
Received: ACK2
Received: ACK3
Sliding window...

Sending frames: ['4', '5']
Received: ACK4
Received: ACK5
Sliding window...

All frames sent successfully.

Server waiting for connection...
Connected to client: ('127.0.0.1', 54321)

Received Frame: 1
Received Frame: 2
Received Frame: 3
Received Frame: 4
Received Frame: 5
```
## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
