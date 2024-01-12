---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# Concurrency and Parallelism in CPP

## Introduction

C++의 동시성과 병렬성은 프로그램이 여러 작업을 동시에 실행하거나, 여러 코어에서 작업을 분산시켜 처리하는 기능을 의미합니다. 이는 프로그램의 반응성을 높이고, 멀티코어 프로세서의 이점을 최대한 활용할 수 있게 합니다.

## Multithreading

`std::thread`는 C++11에서 도입된 멀티스레딩을 지원하는 클래스입니다. 여러 스레드를 생성하여 병렬로 작업을 수행할 수 있습니다.

```cpp
#include <iostream>
#include <thread>

void threadFunction() {
    std::cout << "스레드 실행\n";
}

int main() {
    std::thread t(threadFunction);
    t.join(); // 스레드가 종료될 때까지 대기
    return 0;
}
```

```bash
스레드 실행
```

이 예제에서는 `threadFunction`이라는 함수를 별도의 스레드에서 실행합니다. `std::thread` 객체 `t`는 이 함수를 병렬로 실행하고, `join`을 호출하여 스레드가 종료될 때까지 메인 스레드가 대기합니다.

## Async Programming

`std::async`와 `std::future`는 C++11에서 도입된 비동기 프로그래밍을 위한 도구입니다. 이들을 사용하여 비동기 작업을 시작하고 결과를 기다릴 수 있습니다.

```cpp
#include <iostream>
#include <future>

int asyncFunction() {
    return 42;
}

int main() {
    std::future<int> result = std::async(asyncFunction);
    std::cout << "결과: " << result.get() << std::endl;
    return 0;
}
```

```bash
결과: 42
```

이 예제에서는 `asyncFunction`을 비동기적으로 실행하고, 그 결과를 `future` 객체를 통해 받습니다.

## Thread Safety

스레드 안전성은 멀티스레딩 환경에서 데이터의 일관성과 무결성을 유지하는 것을 의미합니다. 이를 위해 동기화 메커니즘(예: 뮤텍스, 세마포어)을 사용합니다.

```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
int sharedResource = 0;

void increment() {
    std::lock_guard<std::mutex> lock(mtx);
    ++sharedResource;
}

int main() {
    std::thread t1(increment);
    std::thread t2(increment);
    t1.join();
    t2.join();
    std::cout << "공유 자원: " << sharedResource << std::endl;
    return 0;
}
```

```bash
공유 자원: 2
```

## SocketCAN과 멀티스레딩의 결합

SocketCAN은 리눅스 기반 시스템에서 CAN 네트워크 인터페이스와의 통신을 위한 표준 소켓 인터페이스입니다. 멀티스레딩과 함께 사용할 경우, 여러 스레드에서 동시에 CAN 메시지를 보내거나 받을 수 있습니다. 이는 특히 실시간 데이터 처리나 동시에 여러 CAN 장치를 제어해야 하는 상황에서 유용합니다.

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <linux/can.h>
#include <linux/can/raw.h>
#include <sys/socket.h>
#include <unistd.h>

std::mutex can_mutex;

void readCAN(int sockfd) {
    can_frame frame;
    while (true) {
        std::unique_lock<std::mutex> lock(can_mutex);
        int nbytes = read(sockfd, &frame, sizeof(frame));
        if (nbytes > 0) {
            // CAN 메시지 처리...
        }
        lock.unlock();
    }
}

void writeCAN(int sockfd, const can_frame& frame) {
    std::unique_lock<std::mutex> lock(can_mutex);
    write(sockfd, &frame, sizeof(frame));
    lock.unlock();
}

int main() {
    int sockfd = /* SocketCAN 설정 코드 */;
    
    std::thread readThread(readCAN, sockfd);
    std::thread writeThread(writeCAN, sockfd, /* CAN 프레임 데이터 */);

    readThread.join();
    writeThread.join();

    close(sockfd);
    return 0;
}
```

이 코드에서는 `readCAN`과 `writeCAN` 함수를 별도의 스레드에서 실행하여 CAN 메시지를 동시에 읽고 쓸 수 있습니다. `std::mutex`를 사용하여 소켓에 대한 접근을 동기화함으로써 데이터 경쟁을 방지합니다.

## Conclusion

C++에서의 동시성과 병렬성은 프로그램의 성능과 반응성을 향상시키는 중요한 수단입니다. 멀티스레딩, 비동기 프로그래밍, 스레드 안전성은 복잡한 작업을 효율적으로 처리하기 위해 필수적인 요소입니다. 이러한 기술을 활용함으로써, 멀티코어 프로세서의 잠재력을 최대한 활용하고, 더 빠르고 안정적인 프로그램을 개발할 수 있습니다.
