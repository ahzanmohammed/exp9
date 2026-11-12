
import socket
import threading
import os

def handle_client(conn):
    filename = conn.recv(1024).decode()

    if os.path.isfile(filename):
        with open(filename, 'r', encoding='utf-8') as file:
            content = file.read()

        conn.send(content.encode())

    else:
        conn.send("File not found".encode())

    conn.close()


server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("0.0.0.0", 8080))
server.listen(5)

print("File Server Running...")

while True:
    conn, addr = server.accept()
    print("Connected:", addr)

    threading.Thread(target=handle_client, args=(conn,)).start()




import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server_ip = input("Enter server IP: ")
client.connect((server_ip, 8080))

filename = input("Enter filename: ")

client.send(filename.encode())

data = client.recv(4096).decode()

print("File Content:")
print(data)

client.close()




