---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# Concurrency and Parallelism in CPP

## \[ Introduction ]

C++의 동시성과 병렬성은 프로그램이 여러 작업을 동시에 실행하거나, 여러 코어에서 작업을 분산시켜 처리하는 기능을 의미합니다. 이는 프로그램의 반응성을 높이고, 멀티코어 프로세서의 이점을 최대한 활용할 수 있게 합니다.

{% hint style="info" %}
아래 링크의 설명을 읽고 오시면 도움이 됩니다 :)
{% endhint %}

{% embed url="https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-multi-process-multi-thread" %}

## \[ Multithreading ]

`std::thread`는 C++11에서 도입된 멀티스레딩을 지원하는 클래스입니다. 여러 스레드를 생성하여 병렬로 작업을 수행할 수 있습니다.

```cpp
#include <iostream>
#include <thread>
#include <linux/can.h>
#include <linux/can/raw.h>
#include <sys/socket.h>
#include <unistd.h>

void canReadThread(int sockfd) {
    can_frame frame;
    while (read(sockfd, &frame, sizeof(frame)) > 0) {
        // CAN 프레임 처리
        std::cout << "CAN ID: " << frame.can_id << std::endl;
    }
}

int main() {
    int sockfd = /* SocketCAN 소켓 설정 코드 */;
    std::thread readThread(canReadThread, sockfd);
    readThread.join(); // 스레드가 종료될 때까지 대기
    close(sockfd);
    return 0;
}

```

이 예제에서 `canReadThread` 함수는 SocketCAN 소켓으로부터 CAN 프레임을 읽어 처리하는 작업을 수행합니다. 별도의 스레드에서 이 작업을 수행함으로써 메인 스레드의 다른 작업에 방해받지 않고 CAN 메시지를 처리할 수 있습니다.

{% hint style="info" %}
멀티스레딩(`std::thr[ead`): 병렬 처리가 필요하고, CPU 또는 I/O 집약적인 작업을 독립적으로 수행할 때 유용합니다. 예를 들어, SocketCAN을 사용하여 지속적으로 데이터를 수신하거나 전송하는 경우에 적합합니다.
{% endhint %}

## \[ Async Programming ]

`std::async`와 `std::future`는 C++11에서 도입된 비동기 프로그래밍을 위한 도구입니다. 이들을 사용하여 비동기 작업을 시작하고 결과를 기다릴 수 있습니다.

```cpp
#include <iostream>
#include <future>
#include <fstream>
#include <string>

std::string readFileAsync(const std::string& filename) {
    std::ifstream file(filename);
    return std::string((std::istreambuf_iterator<char>(file)),
                        std::istreambuf_iterator<char>());
}

int main() {
    std::future<std::string> result = std::async(readFileAsync, "canframe.txt");
    // 다른 작업 수행 ...

    std::string fileContent = result.get(); // 파일 내용을 기다림
    std::cout << "파일 내용: " << fileContent << std::endl;
    return 0;
}

```

이 예제에서 `readFileAsync` 함수는 파일을 비동기적으로 읽습니다. `std::async`를 사용하여 파일 읽기 작업을 시작하고, 메인 스레드는 다른 작업을 계속 진행할 수 있습니다. 파일 읽기 작업이 완료되면, 그 결과는 `std::future` 객체를 통해 메인 스레드에 전달됩니다.

{% hint style="info" %}
비동기 프로그래밍(`std::async`): 작업의 결과가 즉시 필요하지 않고, 백그라운드에서 실행되어야 하는 경우에 유용합니다. 예를 들어, 파일 읽기, 데이터베이스 쿼리, 웹 요청 등의 작업을 메인 스레드의 흐름을 방해하지 않고 수행할 때 적합합니다.
{% endhint %}

## \[ Thread Safety ]

스레드 안전성은 멀티스레딩 환경에서 데이터의 일관성과 무결성을 유지하는 것을 의미합니다. 이를 위해 동기화 메커니즘(예: 뮤텍스, 세마포어)을 사용합니다.

### **뮤텍스 (Mutex)**

* **정의 및 용도**: 뮤텍스는 상호 배제(Mutual Exclusion)의 줄임말로, 한 번에 하나의 스레드만 특정한 섹션(크리티컬 섹션)의 코드를 실행하도록 하는 동기화 메커니즘입니다. 주로 공유 자원에 대한 접근을 제어하는 데 사용됩니다.
* **동작 원리**: 뮤텍스는 잠금(locking)과 잠금 해제(unlocking)의 두 가지 상태를 가집니다. 스레드가 공유 자원에 접근하려면 먼저 뮤텍스를 잠그고(획득하고), 작업을 완료한 후에는 뮤텍스를 잠금 해제(반환)해야 합니다.
* **특징**: 뮤텍스는 오너십이라는 개념을 가지고 있습니다. 즉, 잠금을 획득한 스레드만이 잠금을 해제할 수 있습니다.

```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx; // 뮤텍스 정의

void printEven(int num) {
    if (num % 2 == 0) {
        mtx.lock(); // 뮤텍스 잠금
        std::cout << num << " is even." << std::endl;
        mtx.unlock(); // 뮤텍스 잠금 해제
    }
}

int main() {
    std::thread t1(printEven, 2);
    std::thread t2(printEven, 3);
    t1.join();
    t2.join();
    return 0;
}



```

이 예제에서는 두 개의 스레드가 `printEven` 함수를 동시에 호출합니다. 뮤텍스는 한 번에 하나의 스레드만이 출력을 수행하도록 보장합니다.

### **세마포어 (Semaphore)**

* **정의 및 용도**: 세마포어는 카운팅을 기반으로 하는 동기화 메커니즘입니다. 주로 여러 스레드가 동시에 리소스나 파일 등에 접근하는 것을 제어하는 데 사용됩니다.
* **동작 원리**: 세마포어는 내부 카운터를 가지고 있으며, 이 카운터는 리소스에 접근할 수 있는 스레드의 수를 제한합니다. 카운터 값이 0이면 모든 리소스가 사용 중이며, 0보다 크면 사용 가능한 리소스가 있음을 나타냅니다.
* **특징**: 세마포어는 오너십이 없습니다. 어떤 스레드든지 세마포어를 증가시키거나 감소시킬 수 있습니다.

```cpp
#include <iostream>
#include <thread>
#include <semaphore.h>

std::counting_semaphore<2> sem(2); // 최대 2개의 스레드가 동시에 접근 가능한 세마포어

void accessResource(int id) {
    sem.acquire(); // 세마포어 획득
    std::cout << "Thread " << id << " is accessing the resource." << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(1)); // 리소스 사용을 시뮬레이션
    sem.release(); // 세마포어 반환
}

int main() {
    std::thread threads[4];
    for (int i = 0; i < 4; ++i) {
        threads[i] = std::thread(accessResource, i);
    }
    for (auto& t : threads) {
        t.join();
    }
    return 0;
}

```

이 코드는 `std::counting_semaphore`를 사용하여 세마포어 기반의 동기화를 구현하는 예시입니다. 최대 2개의 스레드가 동시에 리소스에 접근할 수 있으며, 네 개의 스레드가 순차적으로 리소스에 접근하여 작업을 수행합니다.

## \[ SocketCAN과 멀티스레딩의 결합 ]

SocketCAN은 리눅스 기반 시스템에서 CAN 네트워크 인터페이스와의 통신을 위한 표준 소켓 인터페이스입니다. 멀티스레딩과 함께 사용할 경우, **여러 스레드에서 동시에 CAN 메시지를 보내거나 받을 수 있습니다.** 이는 특히 실시간 데이터 처리나 **동시에 여러 CAN 장치를 제어해야 하는 상황에서 유용**합니다.

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

이 코드에서는 `readCAN`과 `writeCAN` 함수를 별도의 스레드에서 실행하여 CAN 메시지를 동시에 읽고 쓸 수 있습니다. `std::mutex`를 사용하여 **소켓에 대한 접근을 동기화**함으로써 데이터 경쟁을 방지합니다.

## \[ Conclusion ]

**C++에서의 동시성과 병렬성은 프로그램의 성능과 반응성을 향상시키는 중요한 수단**입니다. 멀티스레딩, 비동기 프로그래밍, 스레드 안전성은 복잡한 작업을 효율적으로 처리하기 위해 필수적인 요소입니다. 이러한 기술을 활용함으로써, 멀티코어 프로세서의 잠재력을 최대한 활용하고, 더 빠르고 안정적인 프로그램을 개발할 수 있습니다.
