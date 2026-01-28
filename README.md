# Distributed Multi-Server Search System (C/Unix)
# a group project 
##  Project Overview
A high-performance, distributed search engine architecture built from scratch in C. The system utilizes a multi-tier architecture to handle concurrent client requests and parallelize text searching across multiple backend servers using TCP/IP sockets.

## System Architecture



The system consists of three core components:
1.  **Client:** Interactive interface for users to submit search keywords.
2.  **Interface Server:** Acts as a load balancer/orchestrator. It receives client requests and uses **Multithreading (pthreads)** to query multiple search servers simultaneously.
3.  **Search Server:** The "worker" node. It uses **Process-level Parallelism (forking)** to handle multiple client connections and **Semaphores** to ensure thread-safe result aggregation.

##  Technical Highlights

### 1. Concurrency & Parallelism
* **Interface Layer:** Implemented a thread-per-query model using `pthread_create`. This allows the server to wait for responses from three different search servers at once, significantly reducing latency.
* **Search Layer:** Utilizes `fork()` to create child processes for every incoming connection, ensuring the main server remains responsive.

### 2. Synchronization & IPC
* **Semaphores:** Used `sem_t` to manage critical sections during the file-searching process, preventing data races when multiple threads write to the shared result buffer.
* **Signal Handling:** Implemented custom `SIGCHLD` handlers to prevent "zombie processes" and manage system resources efficiently.

### 3. Network Programming
* **Socket API:** Built on robust TCP Stream Sockets (`AF_INET`, `SOCK_STREAM`).
* **Custom Protocol:** Designed a simple text-based protocol for sending keywords and receiving aggregated search results from multiple `.txt` database files.

  ## 🚀 How to Run
1. **Compile the servers:**
   gcc server.c -o server -lpthread
   
   gcc interface.c -o interface -lpthread

   gcc client.c -o client

   make sure each file is running on different terminal
