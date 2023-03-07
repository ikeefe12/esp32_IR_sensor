# esp32_IR_sensor - Heat Seeking Drone Project

This repository holds the arduino code that is loaded onto the ESP32 board to get the IR data from the sensor and send it over wifi to a local port.

The majority of this code was provided by Boston Electronics upon purchasing the sensor and application board from them. Out of the box, the code streamed data over wifi to a GUI they provide. The protocol for streaming data was too fast and unreliable for our use case in this project.

# New Data Streaming Protocol
The ESP32 board will connect to Capstone Router with IP_ADDRESS = 192.168.0.120 and PORT_NUMBER = 30444.

In order to recieve data from the ESP32 Board follow these steps (code is in Python):

1) Connect to socket
    `client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM);
    client_socket.connect((IP_ADDRESS, PORT_NUMBER))`

2) Send a request to bind to device:
    `client_socket.send(b"Bind HTPA series device")`
    
3) Check that ESP32 is correctly bound by reading the response:
    `data = client_socket.recv(1024);
    print(data)`
    
4) Send message to configure ESP32 to send thermal data in temperature form (Kelvins):
    `client_socket.send(b"K")`
    
5) Whenever you want the most recent data from the ESP32 board, send a trigger for the next frame:
    `client_socket.send(b"N")`
    
6) To recieve the frame, read 21 packets of size 761 from the socket. The first byte of each packet is the packet number, this can be discarded unless we start seeing packages come in out of order. In depth example code can be found at this link (https://github.com/ikeefe12/CNN_Model_Development/blob/main/connect_to_sensor.py)

