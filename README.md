# Socket-Chat-Project-
I will show how to build chat app by using Socket server-client 


Server:
---------

from socket import*
import threading


def worker(client):
    while True:

        data = client.recv(2048).decode()

        if data == "exit":
            client.close()
            clients.remove(client)
            break

        for item in clients:
         item.sendall(data.encode())



server= socket(AF_INET,SOCK_STREAM)
server.bind(("0.0.0.0",3333))
server.listen(100)
clients=[]


while True:
 client, addr= server.accept()
 clients.append(client)

 t=threading.Thread(target=worker,args=(client,))
 t.start()



client:
--------

from socket import *
import threading

def reader (client):
    while True:
     data = client.recv(2048).decode()
     print(data)


client = socket(AF_INET,SOCK_STREAM)
client.connect(("Your IP", 3333))
print("Connected to server")


t = threading.Thread(target=reader,args=(client,))
t.start()



while True:
    data = input("Client > ")
    client.sendall(data.encode())

    if data == "exit":
        client.close()
        break



