# ⚡ **Address:**
- **`ffsockaddr_set_ipv4(ffsockaddr a, const void *ipv4, ffuint port)`**  
  Establece la dirección IPv4 y el puerto L4 en la estructura `ffsockaddr`. Si `ipv4` es NULL, se establece la dirección en `0.0.0.0`.
  
- **`ffsockaddr_set_ipv6(ffsockaddr a, const void *ipv6, ffuint port)`**  
  Establece la dirección IPv6 y el puerto L4 en la estructura `ffsockaddr`. Si `ipv6` es NULL, se establece la dirección en `::`.
  
- **`ffsockaddr_ip_port(const ffsockaddr a, ffuint *port)`**  
  Devuelve la dirección IP (IPv4 o IPv6) y el puerto L4 de la estructura `ffsockaddr`.

- **`ffaddrinfo_resolve(const char *name, int flags)`**  
  Traduce un nombre de host a una dirección de red, devolviendo un puntero a una estructura `ffaddrinfo` que contiene la información de la dirección.

- **`ffaddrinfo_free(ffaddrinfo *a)`**  
  Libera la memoria asignada a la estructura `ffaddrinfo`.

- **`ffaddrinfo_error(ffuint e)`**  
  Devuelve un mensaje de error correspondiente al código de error proporcionado.

---

# 🔨 **Creation:**
- **`ffsock_create(int domain, int type, int protocol)`**  
  Crea un socket de comunicación con el dominio, tipo y protocolo especificados. Devuelve un descriptor de socket o `FFSOCK_NULL` en caso de error.

- **`ffsock_create_tcp(int domain, int flags)`**  
  Crea un socket TCP con el dominio y las banderas especificadas. Devuelve un descriptor de socket o `FFSOCK_NULL` en caso de error.

- **`ffsock_create_udp(int domain, int flags)`**  
  Crea un socket UDP con el dominio y las banderas especificadas. Devuelve un descriptor de socket o `FFSOCK_NULL` en caso de error.

- **`ffsock_accept(ffsock listensk, ffsockaddr peer, int flags)`**  
  Acepta una conexión entrante en el socket de escucha `listensk` y devuelve un nuevo socket aceptado. El parámetro `peer` se utiliza para obtener la dirección del cliente.

- **`ffsock_accept_async(ffsock lsock, ffsockaddr peer, int flags, int domain, ffsockaddr local, ffkqtaskaccept *task)`**  
  Realiza la aceptación de la conexión de manera asincrónica.

- **`ffsock_close(ffsock sk)`**  
  Cierra el socket especificado.

---

# ⚙️ **Configuration:**
- **`ffsock_nonblock(ffsock sk, int nonblock)`**  
  Establece el modo no bloqueante en el socket especificado. Devuelve `0` en caso de éxito.

- **`ffsock_setopt(ffsock sk, int level, int name, int val)`**  
  Establece una opción en el socket especificado. Devuelve `0` en caso de éxito.

- **`ffsock_deferaccept(ffsock sk, int enable)`**  
  Establece la opción de "deferred accept" en el socket de escucha especificado. Devuelve `0` en caso de éxito.

- **`ffsock_getopt(ffsock sk, int level, int name, int *dst)`**  
  Obtiene el valor de una opción del socket especificado. Devuelve `0` en caso de éxito.

- **`ffsock_bind(ffsock sk, const ffsockaddr *addr)`**  
  Asocia un nombre al socket especificado. Devuelve `0` en caso de éxito.

- **`ffsock_connect(ffsock sk, const ffsockaddr *addr)`**  
  Inicia una conexión en el socket especificado. Devuelve `0` en caso de éxito.

- **`ffsock_connect_async(ffsock sk, const ffsockaddr *addr, ffkq_task *task)`**  
  Realiza la operación de conexión de manera asincrónica.

- **`ffsock_listen(ffsock listensk, ffuint maxconn)`**  
  Marca el socket especificado como un socket de escucha con un backlog máximo de conexiones pendientes. Devuelve `0` en caso de éxito.

---

# 📡 **I/O:**
- **`ffsock_recv(ffsock sk, void *buf, ffsize cap, int flags)`**  
  Recibe datos del socket especificado. Devuelve el número de bytes recibidos o un valor negativo en caso de error.

- **`ffsock_recvfrom(ffsock sk, void *buf, ffsize cap, int flags, ffsockaddr peeraddr)`**  
  Recibe datos y obtiene la dirección del remitente. Devuelve el número de bytes recibidos o un valor negativo en caso de error.

- **`ffsock_send(ffsock sk, const void *buf, ffsize len, int flags)`**  
  Envía datos a través del socket especificado. Devuelve el número de bytes enviados o un valor negativo en caso de error.

- **`ffsock_sendv(ffsock sk, ffiovec *iov, ffuint iov_n)`**  
  Envía datos a través del socket utilizando un vector de E/S. Devuelve el número de bytes enviados o un valor negativo en caso de error.

- **`ffsock_sendto(ffsock sk, const void *buf, ffsize len, int flags, const ffsockaddr peeraddr)`**  
  Envía datos a través del socket especificado a la dirección del destinatario. Devuelve el número de bytes enviados o un valor negativo en caso de error.

---

# 🕹️ **Async I/O:**
- **`ffsock_recv_async(ffsock sk, void *buf, ffsize cap, ffkq_task *task)`**  
  Recibe datos del socket de manera asincrónica. Devuelve el número de bytes recibidos o un valor negativo en caso de error.

- **`ffsock_recv_udp_async(ffsock sk, void *buf, ffsize cap, ffkq_task *task)`**  
  Recibe datos UDP de manera asincrónica.

- **`ffsock_recvfrom_async(ffsock sk, void *buf, ffsize cap, ffsockaddr peeraddr, ffkqtask *task)`**  
  Recibe datos y obtiene la dirección del remitente de manera asincrónica.

- **`ffsock_send_async(ffsock sk, const void *buf, ffsize len, ffkq_task *task)`**  
  Envía datos de manera asincrónica.

- **`ffsock_sendv_async(ffsock sk, ffiovec iov, ffuint iov_n, ffkq_task *task)`**  
  Envía datos utilizando un vector de E/S de manera asincrónica.

---

# 💾 **I/O vector:**
- **`ffiovec_set(ffiovec iov, const void *data, ffsize len)`**  
  Establece el búfer y la longitud en la estructura `ffiovec`.

- **`ffiovec_get(ffiovec *iov)`**  
  Devuelve un `ffslice` que representa el búfer y la longitud en la estructura `ffiovec`.

- **`ffiovec_shift(ffiovec *iov, ffsize n)`**  
  Desplaza el puntero de búfer y reduce la longitud en la estructura `ffiovec` en `n` bytes. Devuelve el número de bytes desplazados.

- **`ffiovec_array_shift(ffiovec iov, ffsize n, ffsize skip)`**  
  Desplaza el puntero de búfer y reduce la longitud en el array de estructuras `ffiovec` en `skip` bytes. Devuelve `1` si aún quedan elementos no vacíos en el array, `0` en caso contrario.
