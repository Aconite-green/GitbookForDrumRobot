# Hardware Communication in Linux

## Serial Communication
시리얼 통신은 리눅스 시스템에서 하드웨어 장치와 통신하는 기본적인 방법 중 하나입니다.
- Configuring and Using Serial Ports

리눅스에서 시리얼 포트를 설정하고 사용하기 위해 여러 도구와 명령어가 사용됩니다. setserial, stty, minicom 등의 도구가 이에 해당합니다.

시리얼 포트 구성
시리얼 포트를 구성하는 예시 명령어는 다음과 같습니다:

stty -F /dev/ttyS0 speed 9600 cs8 -cstopb -parenb

이 명령어는 /dev/ttyS0 시리얼 포트를 9600 baud rate으로 설정합니다.

시리얼 통신을 위한 도구 사용
minicom은 리눅스에서 널리 사용되는 시리얼 통신 도구입니다. 다음은 minicom을 설정하는 방법입니다:

sudo minicom -s

이 명령어를 실행한 후, 시리얼 포트 설정을 위해 메뉴를 사용합니다.

- Tools and Commands for Serial Communication

리눅스에서는 screen, kermit, picocom과 같은 다양한 시리얼 통신 도구를 사용할 수 있습니다. 예를 들어, screen을 사용하여 시리얼 포트에 접속하는 방법은 다음과 같습니다:

screen /dev/ttyS0 9600

이 명령어는 /dev/ttyS0 시리얼 포트에 9600 baud rate으로 접속합니다.


## GPIO Control
- Accessing and Controlling GPIO Pins

리눅스에서 GPIO 핀에 접근하고 제어하는 것은 sysfs 인터페이스 또는 새로운 GPIO character device 인터페이스를 통해 이루어집니다.

GPIO 핀 설정 예시
GPIO 핀을 제어하기 위한 기본적인 방법은 다음과 같습니다:

echo 24 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio24/direction
echo 1 > /sys/class/gpio/gpio24/value

이 명령어들은 GPIO 24번 핀을 출력 모드로 설정하고, 해당 핀에 높은 전압(1)을 출력합니다.

- Practical Examples of GPIO Usage

실제 애플리케이션에서 GPIO 핀을 사용하는 예는 LED 제어, 버튼 입력 읽기 등이 있습니다. 예를 들어, 버튼 입력을 감지하여 LED를 켜고 끄는 기본적인 코드는 다음과 같습니다:

# 버튼이 눌렸는지 확인
if [ $(cat /sys/class/gpio/gpio17/value) -eq 1 ]; then
    echo 1 > /sys/class/gpio/gpio24/value
else
    echo 0 > /sys/class/gpio/gpio24/value
fi

이 스크립트는 GPIO 17번 핀(버튼 연결)의 상태를 확인하여, GPIO 24번 핀(LED 연결)에 전압을 출력합니다.

## I2C and SPI Protocols
- Overview of I2C and SPI

I2C: Inter-Integrated Circuit의 약자로, 직렬 통신 프로토콜입니다. 주로 저속, 짧은 거리 통신에 사용됩니다.
SPI: Serial Peripheral Interface의 약자로, 빠른 데이터 전송을 위한 직렬 통신 프로토콜입니다.

- Communicating with I2C/SPI Devices
리눅스에서 I2C나 SPI 장치와 통신하기 위해 필요한 단계는 다음과 같습니다:

I2C 장치와의 통신 예시
I2C 장치와 통신하기 위한 예시 코드는 다음과 같습니다:

i2cset -y 1 0x48 0x00 0x00 b

이 명령은 I2C 버스 1에 있는 주소 0x48의 장치에 데이터를 씁니다.

SPI 장치와의 통신 예시
SPI 통신을 위한 예제는 다음과 같습니다:

spiwrite -D /dev/spidev0.0 -b 8 -s 500000 -v 0x0f

이 명령은 /dev/spidev0.0 SPI 장치에 데이터를 씁니다.

이러한 방법들은 리눅스 시스템에서 다양한 하드웨어 장치와 통신할 때 기본적인 방법들입니다. 이를 통해 리눅스 기반 시스템에서 센서 데이터를 읽거나, 모터를 제어하고, 다양한 종류의 하드웨어와 상호작용할 수 있습니다. 이러한 통신 방법들은 리눅스 임베디드 시스템을 개발하고 제어하는 데 있어 핵심적인 역할을 합니다.