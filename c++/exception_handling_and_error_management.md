---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# Exception Handling and Error Management

## \[ Introduction ]

C++에서의 예외 처리는 프로그램의 안정성과 신뢰성을 높이는 중요한 요소입니다. 예외 처리를 통해 실행 시 발생할 수 있는 예기치 않은 오류를 적절하게 처리하고, 프로그램의 정상적인 흐름을 유지할 수 있습니다..

## \[ Try-Catch Blocks ]

`try-catch` 블록은 예외가 발생할 수 있는 코드를 감싸고, 예외 발생 시 이를 잡아내어 처리합니다. `try` 블록 안에서 코드를 실행하고, 예외가 발생하면 이에 해당하는 `catch` 블록이 실행됩니다.

```cpp
#include <iostream>
using namespace std;

int main() {
    try {
        throw runtime_error("에러 발생!"); // 예외 발생
    } catch (const runtime_error& e) {
        cout << "예외 처리: " << e.what() << endl;
    }
    return 0;
}
```

이 코드에서는 `runtime_error` 예외를 던지고, 이를 `catch` 블록에서 잡아내어 처리합니다.

## \[ Custom Exception Classes ]

C++에서는 사용자 정의 예외 클래스를 만들어 특정 오류 상황에 대한 예외를 정의할 수 있습니다. 이를 통해 더 세밀한 예외 처리가 가능해집니다.

```cpp
class MyException : public std::exception {
public:
    const char* what() const noexcept override {
        return "나만의 예외 발생!";
    }
};

int main() {
    try {
        throw MyException(); // 사용자 정의 예외 발생
    } catch (const MyException& e) {
        cout << "예외 처리: " << e.what() << endl;
    }
    return 0;
}

```

## \[ Best Practices ]1

예외 처리의 모범 사례에는 다음과 같은 것들이 포함됩니다:

* 가능하면 오류 상황을 예외로 전환하여 처리합니다.
* 예외는 구체적이고 설명이 포함된 메시지를 제공해야 합니다.
* 모든 예외를 잡아내려 하지 말고, 처리할 수 있는 예외만 처리합니다.
* 예외를 사용하여 프로그램의 흐름을 제어하는 것은 피합니다.

## \[ Exception Handling Examples in Communication ]

### 1. 소켓 연결 설정 중 예외 처리

통신 프로그램에서 소켓을 생성하고 연결을 설정하는 과정은 여러 오류가 발생할 수 있는 단계입니다. 이러한 오류에는 잘못된 주소, 네트워크 문제, 포트 충돌 등이 포함됩니다.

```cpp
#include <iostream>
#include <stdexcept>
#include <sys/socket.h>
#include <netinet/in.h>
#include <cstring>

void setupSocket() {
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd == -1) {
        throw std::runtime_error("소켓 생성 실패");
    }

    sockaddr_in addr;
    memset(&addr, 0, sizeof(addr));
    addr.sin_family = AF_INET;
    addr.sin_port = htons(8080);
    // 여기서 주소 설정을 잘못할 수 있음

    if (connect(sockfd, (struct sockaddr *)&addr, sizeof(addr)) < 0) {
        throw std::runtime_error("연결 실패");
    }

    // 데이터 전송/수신 로직
}

int main() {
    try {
        setupSocket();
    } catch (const std::exception& e) {
        std::cout << "예외 발생: " << e.what() << std::endl;
        // 예외 처리 로직
    }
    return 0;
}

```

### 2. 데이터 전송 중 예외 처리

데이터를 전송하는 과정에서도 여러 예외가 발생할 수 있습니다. 예를 들어, 네트워크 연결이 끊기거나 전송할 데이터가 너무 큰 경우 등입니다.

```cpp
void sendData(int sockfd, const char* data, size_t dataSize) {
    if (send(sockfd, data, dataSize, 0) < 0) {
        throw std::runtime_error("데이터 전송 실패");
    }
}

int main() {
    try {
        int sockfd = /* 소켓 설정 코드 */;
        const char* data = "Hello, World!";
        sendData(sockfd, data, strlen(data));
    } catch (const std::exception& e) {
        std::cout << "예외 발생: " << e.what() << std::endl;
        // 예외 처리 로직
    }
    return 0;
}

```

### 3. 수신 데이터 처리 중 예외 처리

수신한 데이터를 처리하는 과정에서도 오류가 발생할 수 있습니다. 예를 들어, 수신된 데이터가 예상 형식과 다르거나 파싱에 실패하는 경우입니다.

```cpp
void processData(const char* data) {
    // 데이터 파싱 및 처리
    if (/* 파싱 실패 */) {
        throw std::runtime_error("데이터 파싱 실패");
    }
}

int main() {
    try {
        char buffer[1024];
        int sockfd = /* 소켓 설정 코드 */;
        recv(sockfd, buffer, sizeof(buffer), 0);
        processData(buffer);
    } catch (const std::exception& e) {
        std::cout << "예외 발생: " << e.what() << std::endl;
        // 예외 처리 로직
    }
    return 0;
}

```

### 4. 실시간 통신에서의 예외 처리 전략

* **오류 코드 사용**: 함수가 성공 또는 실패를 나타내는 오류 코드를 반환하게 하여 예외 발생에 따른 성능 저하를 피할 수 있습니다.
* **상태 확인**: 통신 상태나 오류 상태를 주기적으로 확인하고, 이를 바탕으로 적절한 조치를 취합니다.

```cpp
#include <iostream>
#include <linux/can.h>
#include <linux/can/raw.h>
#include <sys/socket.h>
#include <unistd.h>

// SocketCAN 소켓을 설정하고 연결하는 함수
int setupSocketCAN() {
    int sockfd = socket(PF_CAN, SOCK_RAW, CAN_RAW);
    if (sockfd < 0) {
        // 소켓 생성 실패 처리
        return -1;
    }

    // 다른 SocketCAN 설정...
    
    return sockfd;
}

// CAN 메시지를 전송하는 함수
int sendCANMessage(int sockfd, const can_frame& frame) {
    int nbytes = write(sockfd, &frame, sizeof(frame));
    if (nbytes < 0) {
        // 메시지 전송 실패 처리
        return -1;
    }
    return 0;
}

int main() {
    int sockfd = setupSocketCAN();
    if (sockfd < 0) {
        std::cerr << "SocketCAN 설정 실패\n";
        return 1;
    }

    can_frame frame;
    // CAN 프레임 설정...
    
    if (sendCANMessage(sockfd, frame) < 0) {
        std::cerr << "CAN 메시지 전송 실패\n";
        close(sockfd);
        return 1;
    }

    // 정상적인 통신 처리...
    
    close(sockfd);
    return 0;
}

```

이 코드에서는 SocketCAN 소켓 설정과 메시지 전송 과정에서 발생할 수 있는 오류를 예외 처리 대신 오류 코드를 사용하여 처리합니다. `setupSocketCAN`과 `sendCANMessage` 함수는 실패 시 음수 값을 반환합니다. `main` 함수에서는 이러한 반환 값을 확인하여 오류 상황을 처리합니다.

## \[ Conclusion ]

C++에서 효과적인 오류 관리는 프로그램의 안정성과 신뢰성을 크게 향상시킵니다. 적절한 예외 처리를 통해 예기치 않은 오류로부터 프로그램을 보호하고, 오류 상황에서도 프로그램이 예상대로 작동하도록 보장합니다.
