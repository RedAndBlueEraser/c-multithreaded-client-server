# C Multithreaded Client-Server

An implementation of multithreaded client-server with TCP and IPv4 in pure C. It allows multiple clients to connect to and interact with the server simulataneously.

## How to use?

### Writing

The code provided here is a blank template of a multithreaded client-server with TCP and IPv4. The programs in its current state do nothing apart from connecting the client to the server and then exiting. **Any further features, functions or interactions will need to be implemented by you.**

This readme will help you make changes and add features to this template. The sections of code that are likely to be added or changed are commented with a `TODO`.

#### `server.c`

`server.c` is the code for the server. Basically, it starts a server and accepts connections from clients. Upon accepting a client connection, it dispatches a thread to interact with the client.

On line 143 in `server.c`, there is a TODO statement. This is where you would put code to interact with the client. Normally, you would use `write(new_socket_fd, _Message_, _Message Size_)` to send a message in `_Message_` to the client, and use `read(new_socket_fd, _Message_, _Message Size_)` to receive a message from the client and store it in `_Message_`.

If you need a dispatched thread to access data from the `main()` thread, you can either pass the data using thread arguments or use global variables to store the data.

To pass the data using thread arguments, you need to store additional data types in `struct pthread_arg_t`. Add these to line 22 in `server.c`, initialise them on line 116, and then you can access them on line 139 and use it in the dispatched thread.

If you store the data as global variables, you can directly access them in any thread, but the data is shared by all threads.

On line 153 in `server.c`, you can put on exit code, such as cleanup or logging code. This assumes that the program is exited by pressing Ctrl + C or sent a termination signal.

On lines 128 and 131 in `server.c`, there is commented out code regarding about closing the TCP socket before the server program exits. The commented out code is not reachable, as the server can only exit via `signal_handler()` in line 154. This means the socket is not closed by the program before it exits. You can usually ignore this as the operating system would usually close the socket when the program exits.

There is a small one-per-program-execution memory leak caused by line 99. This can be safely ignored.

#### `client.c`

On line 59 in `client.c`, there is a TODO statement. This is where you would put code to interact with the server. Normally, you would use `write(socket_fd, _Message_, _Message Size_)` to send a message in `_Message_` to the server, and use `read(socket_fd, _Message_, _Message Size_)` to receive a message from the server and store it in `_Message_`.

### Compiling

Compile your C programs by running:

```Shell
gcc -pthread -lnsl -lsocket server.c -o server
gcc -lnsl -lsocket client.c -o client
```

On some machines, the programs can be compiled without the `-lnsl` or `-lsocket` parameters, or will even raise errors when compiling with them. In that case, try to compile without the parameters.

### Running

Run your server program on the server by running:

```Shell
./server _Port number_
```

Run your client program on the client by running:

```Shell
./client _Server IP address_ _Server port number_
```

For example:

```Shell
$ ./server 27015 &
$ ./client 127.0.0.1 27015
```

If you don't specify the IP address or port numbers, then the program asks you to enter one:

```Shell
$ ./client
Enter Server Name: 127.0.0.1
Enter Port: 27015
```

The server program does nothing apart from accepting connections from clients. The client program connects to the server and then exits. **Any further features, functions or interactions will need to be implemented by you.**

## Warnings

- If you do use global variables to store data, be aware that the data is shared by all threads. Be careful of race conditions when you change the values in global variables - use mutex (mutual exclusion) or semaphores to manage the access to these variables.
- The template uses TCP as the transport layer and IPv4 as the internet layer. It is not a very suitable template for UDP. I have not investigated about using IPv6 as the internet layer, but it would probably need changes on lines 21, 33, 47 to 53 and 138 in `server.c` and on lines 21 and 42 to 48 in `client.c`.
- A one-per-program-execution memory leak is in the server program caused by line 99 in `server.c`. In most cases, you can ignore this. If you really want to remove the memory leak, move the `malloc()` statement after the `accept()` statement and change the code as necessary.
- The program doesn't close the TCP socket before it exits. In most cases, you can ignore this, as the operating system would usually close the socket after the program exits. If you really want to close the socket, make `int socket_fd` a global variable and close it in `signal_handler()`.

## Author

Harry Wong (RedAndBlueEraser) <RedAndBlueEraser@outlook.com>
