# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
server
```
#2c
import socket
import random

# ARP Table (IP → MAC)
arp_table = {
    "192.168.1.1": "00:1A:2B:3C:4D:5E",
    "192.168.1.2": "00:1A:2B:3C:4D:5F",
    "192.168.1.3": "00:1A:2B:3C:4D:60",
}

HOST = "127.0.0.1"
PORT = 6000

# Create UDP socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_socket.bind((HOST, PORT))

print(f"ARP/RARP Server running on {HOST}:{PORT}")

while True:
    data, addr = server_socket.recvfrom(1024)
    msg = data.decode().strip()

    if msg.lower() == "exit":
        print("Server shutting down...")
        break

    print(f"Received request: {msg}")

    response = ""

    if msg.startswith("ARP"):
        _, ip = msg.split()
        mac = arp_table.get(ip)
        if mac:
            response = f"ARP Reply: MAC for {ip} is {mac}"
        else:
            response = f"ARP Reply: No entry found for {ip}"

    elif msg.startswith("RARP"):
        _, mac_query = msg.split()
        found_ip = None
        for ip, mac in arp_table.items():
            if mac == mac_query:
                found_ip = ip
                break
        if found_ip:
            response = f"RARP Reply: IP for {mac_query} is {found_ip}"
        else:
            response = f"RARP Reply: No entry found for {mac_query}"

    else:
        response = "Invalid request format!"

    print(f"Sending: {response}")
    server_socket.sendto(response.encode(), addr)

server_socket.close()


```
client
```
#2c
import socket

HOST = '127.0.0.1'
PORT = 6000

client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

while True:
    print("\n=== ARP / RARP Simulation ===")
    print("1. ARP Request (IP → MAC)")
    print("2. RARP Request (MAC → IP)")
    print("3. Exit")
    
    choice = input("Enter your choice (1/2/3): ")

    if choice == "1":
        ip = input("Enter logical (IP) address: ")
        msg = f"ARP {ip}"
        client_socket.sendto(msg.encode(), (HOST, PORT))

    elif choice == "2":
        mac = input("Enter physical (MAC) address: ")
        msg = f"RARP {mac}"
        client_socket.sendto(msg.encode(), (HOST, PORT))

    elif choice == "3":
        client_socket.sendto("exit".encode(), (HOST, PORT))
        print("Client exiting...")


```
## OUPUT - ARP
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ec59889c-353a-422a-bffc-c452dd7e7a4f" />

## PROGRAM - RARP

## OUPUT -RARP
## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
