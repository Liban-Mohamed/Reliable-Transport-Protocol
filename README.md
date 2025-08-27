
README – Individual assignment: DRTP File Transfer Application
------------------------------------------------------------------


File-name: application.py


What is this program and how it runs?
----------------------------------------

This program is a UDP based file transfer application that implements a simple reliable data transfer protocol. This protocol called DRTP (DATA2410 Reliable Transport Protocol) ensures that data is reliably delivered in order without missing data or sending duplicates.

This program does not use TCP but instead it builds reliability manually on top of UDP by:
1. Implementing a 3-way handshake.
2. Using custom packet headers and flags.
3. Managing acknowledgements and retransmissions.
4. Handling connection teardown gracefully.
5. Implementing Go-Back-N protocol for reliability.

It can run the same Python file in either:
1. Client mode to send a file.
2. Server mode to receive a file.

How to Run the program
-------------------------

The program has the following flags:

-s : --server : Start in server mode
-c : --client : Start in client mode
-i : --ip : IP address to use, default: 10.0.1.2
-p : --port : Port number, default: 8088
-f : --file : File to send 
-w : --window : Sliding window size, default: 3
-d : --discard : Sequence number to discard once (for testing retransmission)


Example Commands to run on the terminal
------------------------------------------
On Server side: python3 application.py -s -i 10.0.1.2 -p 8080 -d 11

On Client side: python3 application.py -c -f file.txt -i 10.0.1.2 -p 8080 -w 5

Ensure sure the IP and port match on both server and client.


Short explanation on how it works
-----------------------------------
Connection establishment:

1. Client sends SYN.  
2. Server replies with SYN-ACK. 
3. Client sends ACK.

Data Transfer process:

1. The data the client sends is split into 992-byte chunks.
2. Each chunk is packed with an 8-byte header making them exactly 1kb.
3. Client sends packets using a sliding window with custom window sizes ranging from 1 to 15.
4. Server receives and sends ACKs.
5. Timeout-based Go-Back-N retransmission (400ms).

Connection Teardown process:

Sender sends a packet with the FIN flag ON. 
The receiver responds with a FIN and ACK flags set. 
The connection is then terminated.


What to expect when running It
--------------------------------
Server output:

SYN packet is received
SYN-ACK packet is sent
ACK packet is received
Connection established
...
The throughput is 0.39 Mbps
Connection Closes


Client output:
SYN packet is sent
SYN-ACK packet is received
ACK packet is sent
Connection established
...
FIN ACK packet is received
Connection Closes


Final output
--------------
- Throughput is printed at the end
- Final throughput is shown in Mbps
- Server writes file as 'received_file'


