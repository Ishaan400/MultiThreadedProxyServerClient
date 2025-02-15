<h1>Multi Threaded Proxy Server with and without Cache</h1>

This project is implemented using `C` and Parsing of HTTP referred from <a href = "https://github.com/vaibhavnaagar/proxy-server"> Proxy Server </a>


##### Introduction

##### Basic Working Flow of the Proxy Server:
![](https://github.com/Lovepreet-Singh-LPSK/MultiThreadedProxyServerClient/blob/main/pics/UML.JPG)

##### How did we implement Multi-threading?
- Used Semaphore instead of Condition Variables and pthread_join() and pthread_exit() function. 
- pthread_join() requires us to pass the thread id of the the thread to wait for. 
- Semaphore’s sem_wait() and sem_post() doesn’t need any parameter. So it is a better option. 


##### Motivation/Need of Project
- To Understand → 
  - The working of requests from our local computer to the server.
  - The handling of multiple client requests from various clients.
  - Locking procedure for concurrency.
  - The concept of cache and its different functions that might be used by browsers.
- Proxy Server do → 
  - It speeds up the process and reduces the traffic on the server side.
  - It can be used to restrict user from accessing specific websites.
  - A good proxy will change the IP such that the server wouldn’t know about the client who sent the request.
  - Changes can be made in Proxy to encrypt the requests, to stop anyone accessing the request illegally from your client.
 
##### OS Component Used ​
- Threading
- Locks 
- Semaphore
- Cache (LRU algorithm is used in it)

##### Limitations ​
- If a URL opens multiple clients itself, then our cache will store each client’s response as a separate element in the linked list. So, during retrieval from the cache, only a chunk of response will be send and the website will not open
- Fixed size of cache element, so big websites may not be stored in cache. 

##### How this project can be extended? ​
- This code can be implemented using multiprocessing that can speed up the process with parallelism.
- We can decide which type of websites should be allowed by extending the code.
- We can implement requests like POST with this code.


# Note :-
- Code is well commented. For any doubt you can refer to the comments.


## How to Run

```bash
$ git clone https://github.com/Ishaan400/MultiThreadedProxyServerClient.git
$ cd MultiThreadedProxyServerClient
$ make all
$ ./proxy <port no.>
```
`Open http://localhost:port/https://www.cs.princeton.edu/`

# Note:
- This code can only be run in Linux Machine. Please disable your browser cache.
- To run the proxy without cache Change the name of the file (`proxy_server_with_cache.c to proxy_server_without_cache.c`) MakeFile.



# Converting a Multithreaded Proxy Server from C to C++ with Minimal Changes

There is not much work to be done to change the codebase into c++. Here are some recommendations

---

## 1. File Extensions

- Rename `.c` files to `.cpp`.
- Optionally, rename `.h` files to `.hpp`.

---

## 2. Include Statements

- Replace C headers with C++ counterparts:
  ```cpp
  #include <cstdio>
  #include <cstdlib>
  #include <cstring>
  #include <cerrno>
  ```
- Add necessary C++ headers:
  ```cpp
  #include <iostream>
  #include <string>
  ```

---

## 3. Namespace Usage

- Add `using namespace std;` or prefix standard library elements with `std::`.

---

## 4. String Handling

- Optionally replace `char*` with `std::string` for safer memory management:
  ```cpp
  char buffer[BUFFER_SIZE];
  strcpy(buffer, "Hello");

  // Optional C++ update
  std::string buffer = "Hello";
  ```

---

## 5. Dynamic Memory Allocation

- In C++, you can still use `malloc` and `free`, as C++ maintains backward compatibility with C. However, using `new` and `delete` is recommended:
  ```cpp
  char* str = (char*)malloc(size);
  free(str);

  // Optional C++ update
  char* str = new char[size];
  delete[] str;
  ```

---

## 6. Multithreading

- POSIX threads still work in C++:
  ```cpp
  pthread_t thread_id;
  pthread_create(&thread_id, nullptr, handle_client, (void*)&client_sock);
  pthread_detach(thread_id);
  ```
- Optional C++ update:
  ```cpp
  std::thread t(handle_client, client_sock);
  t.detach();
  ```

---

## 7. Synchronization

- `pthread_mutex_t` works as usual:
  ```cpp
  pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
  pthread_mutex_lock(&mutex);
  pthread_mutex_unlock(&mutex);
  ```
- Optional C++ update:
  ```cpp
  std::mutex mutex;
  mutex.lock();
  mutex.unlock();
  ```

---

## 8. Error Handling

- Traditional error handling with `fprintf` remains valid:
  ```cpp
  if (error) {
      fprintf(stderr, "Error: %s\n", strerror(errno));
      exit(1);
  }
  ```
- Optional C++ exception handling:
  ```cpp
  if (error) {
      std::cerr << "Error: " << strerror(errno) << std::endl;
      throw std::runtime_error("Operation failed");
  }
  ```

---

## 9. I/O Operations

- `printf` and `fprintf` work as usual:
  ```cpp
  printf("Connected to %s\n", address);
  ```
- Optional C++ update:
  ```cpp
  std::cout << "Connected to " << address << std::endl;
  ```

---

## 10. Constants

- `#define` works in C++ but `const` or `constexpr` is safer:
  ```cpp
  #define BUFFER_SIZE 1024

  // Optional C++ update
  const int BUFFER_SIZE = 1024;
  ```

---

## 11. Function Prototypes

- No change required, but inline functions are possible:
  ```cpp
  inline void some_function() {
      // Implementation
  }
  ```

---

## 12. Type Casting

- C-style casts work in C++, but C++ casts are safer:
  ```cpp
  int* p = (int*)malloc(sizeof(int));

  // Optional C++ update
  int* p = static_cast<int*>(malloc(sizeof(int)));
  ```

---

## 13. Structures

- `struct` works the same in C++:
  ```cpp
  struct Cache {
      // members
  };
  ```
- Optional C++ class for encapsulation:
  ```cpp
  class Cache {
  public:
      // public members
  private:
      // private members
  };
  ```

---

## 14. Main Function

- The `main` signature remains valid:
  ```cpp
  int main(void) {
      // ...
  }
  ```
- Optional C++ update:
  ```cpp
  int main() {
      // ...
  }
  ```

---

By following these steps, you can convert a C-based multithreaded proxy server to C++ with minimal effort while taking advantage of modern C++ features.



