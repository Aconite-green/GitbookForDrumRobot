# Linux Programming Basics

## \[ Introduction to Shell Scripting ]

### Basic Syntax and Structure

**셸 스크립트**는 리눅스 시스템에서 자동화된 작업을 수행하는 데 사용됩니다. 셸 스크립트는 배시(Bash)와 같은 셸을 사용하여 작성되며, 명령어들을 순차적으로 실행시킵니다.

```bash
# !/bin/bash
echo "Hello, World!"
```

**첫 번째 줄**은 셸 스크립트가 실행될 셸을 지정합니다 (#!/bin/bash). #으로 시작하는 줄은 주석으로, 스크립트 실행에 영향을 미치지 않습니다. echo 명령어는 터미널에 텍스트를 출력합니다.

### Writing Your First Shell Script&#x20;

간단한 셸 스크립트를 작성하여 기본적인 작업을 수행해봅시다. 예를 들어, 현재 디렉토리의 파일 목록을 출력하는 스크립트는 다음과 같이 작성할 수 있습니다

```bash
# !/bin/bash echo "현재 디렉토리의 파일 목록:"
ls
```

{% hint style="info" %}
**Windows**에서는 쉘 스크립트와 유사한 역할을 하는 여러 스크립팅 옵션이 있습니다. 가장 대표적인 것은 '**Batch File(\*.bat, \*.cmd)'과 'PowerShell(\*.ps1)** 스크립트'입니다.
{% endhint %}

## \[ Introduction to C/C++ Programming in Linux ]

### Setting Up a Development Environment&#x20;

C/C++ 개발을 위해서는 **컴파일러와 텍스트 에디터**가 필요합니다. **GCC (GNU Compiler Collection)**는 리눅스에서 가장 일반적으로 사용되는 C/C++ 컴파일러입니다. 대부분의 리눅스 배포판에서는 **sudo apt install build-essential** 명령을 사용하여 GCC를 설치할 수 있습니다. 텍스트 에디터로는 Vim, Emacs, 또는 Visual Studio Code와 같은 현대적인 IDE를 사용할 수 있습니다.

### Compiling and Running C/C++ Programs

C/C++ 프로그램을 컴파일하고 실행하는 과정은 간단합니다. 예를 들어, hello.c라는 C 프로그램을 컴파일하고 실행하는 방법은 다음과 같습니다:

#### 1.hello.c 파일 작성:&#x20;

```cpp
#include <stdio.h>
int main() { printf("Hello, World!\n"); return 0; }
```

#### 2.컴파일:&#x20;

```
gcc hello.c -o hello 
```

#### 3.실행: ./hello

***

{% hint style="info" %}
* **컴파일**: 소스 코드를 기계어로 변환합니다 (`hello.c` -> `hello.o`).
* **링크**: 여러 오브젝트 파일을 결합하여 실행 가능한 파일을 만듭니다 (`hello.o` -> `hello`).
* **빌드**: 컴파일 및 링크 과정을 포함하여 최종 실행 파일을 만드는 전체 과정입니다.
{% endhint %}

{% embed url="https://kimvampa.tistory.com/27" %}

## \[ Basic Debugging and Tools ]

**Using GDB for Debugging GDB (GNU Debugger)**는 C/C++ 프로그램을 디버깅하기 위한 강력한 도구입니다. GDB를 사용하면 프로그램을 단계별로 실행하고, 변수 값을 확인하고, 메모리 상태를 검사할 수 있습니다.&#x20;

GDB를 사용하기 위해서는 다음과 같이 프로그램을 컴파일할 때 디버깅 정보를 포함시켜야 합니다 ( -g 옵션 추가)

```bash
gcc -g hello.c -o hello.
```

{% hint style="info" %}
### 디버그

디버그(Debug)는 프로그램 개발 과정에서 발생하는 **오류나 버그를 찾아내고 수정하는 과정**을 말합니다. 디버깅은 프로그램이 의도대로 정확하게 작동하도록 하는 데 **필수적인 단계**입니다.

* **디버거(Debugger)**: 디버거는 프로그램의 실행을 단계별로 중지하고, 변수의 값, 메모리 상태, 프로그램의 실행 흐름 등을 검사할 수 있게 해주는 도구입니다. 예를 들어, GDB, Visual Studio Debugger 등이 있습니다.
* **로깅(Logging)**: 프로그램에서 중요한 데이터나 상태 정보를 로그로 기록하여, 오류가 발생했을 때 로그를 분석하는 방법입니다.
* **단위 테스트(Unit Testing)**: 코드의 특정 부분(함수, 메서드 등)이 의도대로 작동하는지 확인하기 위해 작성하는 테스트 코드입니다. 오류를 조기에 발견하고 수정할 수 있습니다.
{% endhint %}

## \[ Essential Linux Programming Tools ]

리눅스는 개발자들에게 다양한 유용한 도구들을 제공합니다. 이러한 도구에는 다음이 포함됩니다:

<table><thead><tr><th width="147">Tool</th><th>Explanation</th></tr></thead><tbody><tr><td>make</td><td>복잡한 프로그램의 빌드를 자동화하는 도구</td></tr><tr><td>valgrind</td><td>메모리 누수와 같은 문제를 감지하는 도구</td></tr><tr><td>git</td><td>소스 코드 버전 관리를 위한 도구</td></tr></tbody></table>

### Tool 별 참고 사이트

{% embed url="https://80000coding.oopy.io/b553047b-42f6-4066-9f30-f4aef0b0503d" %}
Make
{% endembed %}

{% embed url="https://hiseon.me/c/valgrind-tutorial/" %}
Valgrind
{% endembed %}

{% embed url="https://donghak-dev.tistory.com/219" %}
Git
{% endembed %}
