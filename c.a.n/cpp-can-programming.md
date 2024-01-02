---
cover: ../.gitbook/assets/dd9d76e1-0c22-41ac-b8d7-1b352b3a9fc6.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# CAN Programming

## \[ SocketCAN Lib]

<div data-full-width="true">

<figure><img src="../.gitbook/assets/img.gif" alt=""><figcaption><p>Basic Socket Communication</p></figcaption></figure>

</div>

SocketCAN 패키지는 **리눅스용 CAN(Controller Area Network) 프로토콜의 구현체**입니다. CAN은 자동화, 임베디드 장치 및 자동차 분야에서 널리 사용되는 네트워킹 기술입니다. 리눅스 기반의 다른 CAN 구현체들이 문자 장치를 기반으로 했던 반면, SocketCAN은 Berkeley 소켓 API와 리눅스 네트워크 스택을 사용하며, CAN 장치 드라이버를 네트워크 인터페이스로 구현합니다. **CAN 소켓 API는 네트워크 프로그래밍에 익숙한 프로그래머들이 쉽게 CAN 소켓 사용 방법을 배울 수 있도록** TCP/IP 프로토콜과 가능한 한 유사하게 설계되었습니다.



### 예제: CAN 메시지 송수신

```cpp
#include <linux/can.h>
#include <linux/can/raw.h>
#include <sys/socket.h>
#include <unistd.h>

int main() {
    int s; // CAN raw socket
    struct sockaddr_can addr;
    struct can_frame frame;
    struct ifreq ifr;

    // 소켓 생성
    s = socket(PF_CAN, SOCK_RAW, CAN_RAW);

    // 인터페이스 설정
    strcpy(ifr.ifr_name, "can0");
    ioctl(s, SIOCGIFINDEX, &ifr);

    addr.can_family = AF_CAN;
    addr.can_ifindex = ifr.ifr_ifindex;

    // 소켓 바인딩
    bind(s, (struct sockaddr *)&addr, sizeof(addr));

    // CAN 프레임 송신
    frame.can_id = 0x123;
    frame.can_dlc = 2;
    frame.data[0] = 0x11;
    frame.data[1] = 0x22;
    
    write(s, &frame, sizeof(frame));
    //read(s, &frame, sizeof(frame));
    
    // 소켓 닫기
    close(s);

    return 0;
}
```

## \[ CAN Utils ]

## \[ 내용 및 이미지 출처 ]

{% embed url="https://www.beyondlogic.org/example-c-socketcan-code/" %}
