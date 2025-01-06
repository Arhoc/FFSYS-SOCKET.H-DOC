# 🌐 **Address:**

### Resolve a host name to an IPv4 address
```c
// Resolve a host name to an IPv4 address
ffsockaddr addr;
ffsockaddr_set_ipv4(&addr, "192.168.1.100", 80);  // Set the IPv4 address and port 80
// Now you can use addr to bind, connect, or other operations.
```

### Resolve a host name to an IPv6 address
```c
// Resolve a host name to an IPv6 address
ffaddrinfo *res = ffaddrinfo_resolve("example.com", 0);
if (res) {
    for (const ffaddrinfo *i = res; i != NULL; i = i->ai_next) {
        if (i->ai_family == AF_INET6) {
            const struct sockaddr_in6 *a6 = (const void*)i->ai_addr;
            // Use a6->sin6_addr to get the IPv6 address
            printf("Resolved IPv6 address: %s
", inet_ntop(AF_INET6, &a6->sin6_addr, buffer, sizeof(buffer)));
        }
    }
    // Don't forget to free the memory allocated for the resolved info
    ffaddrinfo_free(res);
}
```

---

# 🛠️ **Creation:**

### Create a TCP socket
```c
// Create a TCP socket with IPv4 address family and default flags
ffsock sock = ffsock_create_tcp(AF_INET, 0);
if (sock == FFSOCK_NULL) {
    // Handle error if the socket creation failed
    perror("Failed to create TCP socket");
    return -1;
}

// Now the socket is ready for further operations (bind, listen, connect, etc.)
```

### Accept a connection on a listening socket
```c
ffsockaddr peer;
ffsock accepted = ffsock_accept(listen_sock, &peer, 0);
if (accepted == FFSOCK_NULL) {
    // Handle error if connection acceptance fails
    perror("Failed to accept connection");
} else {
    // Successfully accepted a connection, you can now communicate with the client
    printf("Accepted connection from %s:%d
", inet_ntoa(peer.sin_addr), ntohs(peer.sin_port));
}
```

### Close a socket
```c
// Close the socket when you're done with it
ffsock_close(sock);  // Ensure to close the socket to release resources
```

---

# ⚙️ **Configuration:**

### Set a socket to non-blocking mode
```c
// Set the socket to non-blocking mode (important for asynchronous operations)
ffsock_nonblock(sock, 1);
```

### Set the SO_REUSEADDR option on a socket
```c
// Set the SO_REUSEADDR option to allow reusing local addresses
ffsock_setopt(sock, SOL_SOCKET, SO_REUSEADDR, 1);
```

### Bind a socket to a local address
```c
// Bind the socket to a specific local address and port
ffsockaddr local_addr;
ffsockaddr_set_ipv4(&local_addr, "0.0.0.0", 8080);  // Bind to all interfaces on port 8080
ffsock_bind(sock, &local_addr);
```

### Connect a socket to a remote address
```c
// Connect the socket to a remote address
ffsockaddr remote_addr;
ffsockaddr_set_ipv4(&remote_addr, "192.168.1.100", 80);  // Connect to a server at IP 192.168.1.100 on port 80
ffsock_connect(sock, &remote_addr);
```

### Listen for incoming connections on a socket
```c
// Set up the socket to listen for incoming connections
ffsocklisten(listen_sock, SOMAXCONN);  // SOMAXCONN specifies the maximum connection queue
```

---

# 📡 **I/O:**

### Receive data from a socket
```c
// Receive data from a socket into a buffer
char buffer[1024];
ffssize bytes_received = ffsock_recv(sock, buffer, sizeof(buffer), 0);
if (bytes_received < 0) {
    // Handle error
    perror("Failed to receive data");
} else {
    // Process received data
    printf("Received %ld bytes: %s
", bytes_received, buffer);
}
```

### Send data to a socket
```c
// Send data to a socket
const char* data = "Hello, world!";
ffssize bytes_sent = ffsock_send(sock, data, strlen(data), 0);
if (bytes_sent < 0) {
    // Handle error
    perror("Failed to send data");
} else {
    // Successfully sent the data
    printf("Sent %ld bytes: %s
", bytes_sent, data);
}
```

### Receive data from a socket with the sender's address
```c
// Receive data and get the address of the sender
ffsockaddr peer;
ffssize bytes_received = ffsock_recvfrom(sock, buffer, sizeof(buffer), 0, &peer);
if (bytes_received < 0) {
    // Handle error
    perror("Failed to receive data");
} else {
    // Process received data along with sender's address
    printf("Received %ld bytes from %s:%d
", bytes_received, inet_ntoa(peer.sin_addr), ntohs(peer.sin_port));
}
```

---

# 🕹️ **Async I/O:**

### Receive data from a socket asynchronously
```c
// Receive data asynchronously from a socket
ffkq_task task;
ffssize bytes_received = ffsock_recv_async(sock, buffer, sizeof(buffer), &task);
if (bytes_received < 0 && errno == EINPROGRESS) {
    // The operation is in progress, wait for it to complete
    printf("Data reception is in progress...
");
} else {
    // Data has been received
    printf("Received %ld bytes: %s
", bytes_received, buffer);
}
```

### Send data to a socket asynchronously
```c
// Send data asynchronously to a socket
ffkq_task task;
ffssize bytes_sent = ffsock_send_async(sock, data, strlen(data), &task);
if (bytes_sent < 0 && errno == EINPROGRESS) {
    // The operation is in progress, wait for it to complete
    printf("Data sending is in progress...
");
} else {
    // Data has been sent
    printf("Sent %ld bytes: %s
", bytes_sent, data);
}
```

---

# 💾 **I/O Vector:**

### Send data using a vector of buffers
```c
// Send data using a vector of buffers (scatter-gather I/O)
ffiovec iov[2];
ffiovec_set(&iov[0], "Hello, ", 7);  // First part of the data
ffiovec_set(&iov[1], "world!", 6);  // Second part of the data
ffssize bytes_sent = ffsock_sendv(sock, iov, 2);  // Send both buffers
if (bytes_sent < 0) {
    // Handle error
    perror("Failed to send data using vector");
} else {
    // Successfully sent the data
    printf("Sent %ld bytes using vector
", bytes_sent);
}
```
