
# 1
 The steps involved in establishing a socket on the client side are as follows:

- Create a socket with the **socket()** system call
- Connect the socket to the address of the server using the **connect()** system call
- Send and receive data. There are a number of ways to do this, but the simplest is to use the **read()** and **write()** system calls.

# 2
The steps involved in establishing a socket on the server side are as follows:
- Create a socket with the **socket()** system call
- Bind the socket to an address using the **bind()** system call. For a server socket on the Internet, an address consists of a port number on the host machine.
- Listen for connections with the **listen()** system call
- Accept a connection with the **accept()** system call. This call typically blocks until a client connects with the server.
- Send and receive data

# 3
### Address Domain:
- Ip address: 8bits:8bits:8bits:8bits
- ports: 16bits, lower numbers are reserved in UNIX system
### Socket Type:
- Stream sockets: 
    - Stream sockets treat communications as a continuous stream of characters
    - Stream sockets use **TCP** (Transmission Control Protocol), which is a reliable, stream oriented protocol
- Datagram sockets: Datagram sockets have to read entire messages at once
    - datagram sockets use **UDP** (Unix Datagram Protocol), which is unreliable and message oriented.

# 4
To allow the server to handle multiple simultaneous connections, we make the following changes to the code:

Put the accept statement and the following code in an infinite loop.
After a connection is established, call fork() to create a new process.
The child process will close sockfd and call dostuff, passing the new socket file descriptor as an argument. When the two processes have completed their conversation, as indicated by dostuff() returning, this process simply exits.
The parent process closes newsockfd. Because all of this code is in an infinite loop, it will return to the accept statement to wait for the next connection.
Here is the code.
~~~ C
 while (1) {
     newsockfd = accept(sockfd, 
           (struct sockaddr *) &cli_addr, &clilen);
     if (newsockfd < 0) 
         error("ERROR on accept");
     pid = fork();
     if (pid < 0)
         error("ERROR on fork");
     if (pid == 0)  {
         close(sockfd);
         dostuff(newsockfd);
         exit(0);
     }
     else close(newsockfd);
 } /* end of while */
 ~~~

Code and Instructions are from http://www.cs.rpi.edu/~moorthy/Courses/os98/Pgms/socket.html