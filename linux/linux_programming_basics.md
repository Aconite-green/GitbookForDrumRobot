# Linux Programming Basics

## Introduction to Shell Scripting
- Basic Syntax and Structure

셸 스크립트는 리눅스 시스템에서 자동화된 작업을 수행하는 데 사용됩니다. 셸 스크립트는 배시(Bash)와 같은 셸을 사용하여 작성되며, 명령어들을 순차적으로 실행시킵니다.


#!/bin/bash
# 이곳은 주석입니다
echo "Hello, World!"

첫 번째 줄은 셸 스크립트가 실행될 셸을 지정합니다 (#!/bin/bash).
#으로 시작하는 줄은 주석으로, 스크립트 실행에 영향을 미치지 않습니다.
echo 명령어는 터미널에 텍스트를 출력합니다.

- Writing Your First Shell Script
간단한 셸 스크립트를 작성하여 기본적인 작업을 수행해봅시다. 예를 들어, 현재 디렉토리의 파일 목록을 출력하는 스크립트는 다음과 같이 작성할 수 있습니다:

#!/bin/bash
echo "현재 디렉토리의 파일 목록:"
ls


## Introduction to C/C++ Programming in Linux
- Setting Up a Development Environment
C/C++ 개발을 위해서는 컴파일러와 텍스트 에디터가 필요합니다. GCC (GNU Compiler Collection)는 리눅스에서 가장 일반적으로 사용되는 C/C++ 컴파일러입니다. 대부분의 리눅스 배포판에서는 sudo apt install build-essential 명령을 사용하여 GCC를 설치할 수 있습니다. 텍스트 에디터로는 Vim, Emacs, 또는 Visual Studio Code와 같은 현대적인 IDE를 사용할 수 있습니다.
- Compiling and Running C/C++ Programs

C/C++ 프로그램을 컴파일하고 실행하는 과정은 간단합니다. 예를 들어, hello.c라는 C 프로그램을 컴파일하고 실행하는 방법은 다음과 같습니다:

1.hello.c 파일 작성:
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}

2.컴파일: gcc hello.c -o hello
3.실행: ./hello

C++의 경우에는 g++ 컴파일러를 사용합니다.

## Basic Debugging and Tools

- Using GDB for Debugging
GDB (GNU Debugger)는 C/C++ 프로그램을 디버깅하기 위한 강력한 도구입니다. GDB를 사용하면 프로그램을 단계별로 실행하고, 변수 값을 확인하고, 메모리 상태를 검사할 수 있습니다. GDB를 사용하기 위해서는 다음과 같이 프로그램을 컴파일할 때 디버깅 정보를 포함시켜야 합니다: gcc -g hello.c -o hello.

- Essential Linux Programming Tools
리눅스는 개발자들에게 다양한 유용한 도구들을 제공합니다. 이러한 도구에는 다음이 포함됩니다:

make: 복잡한 프로그램의 빌드를 자동화하는 도구입니다.
valgrind: 메모리 누수와 같은 문제를 감지하는 도구입니다.
strace: 시스템 호출과 시그널을 추적하는 도구입니다.
git: 소스 코드 버전 관리를 위한 도구입니다.
이러한 도구들은 리눅스 개발 환경의 핵심적인 부분이며, 개발자가 보다 효율적으로 코드를 작성하고 문제를 해결하는 데 도움을 줍니다. 리눅스에서 프로그래밍을 할 때 이러한 도구들을 숙지하고 활용하는 것은 매우 중요합니다.