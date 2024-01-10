---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# Memory Management in CPP

## Introduction

C++에서 메모리 관리는 프로그래머에게 큰 책임을 부여합니다. 효율적인 메모리 관리는 프로그램의 성능을 최적화하고, 메모리 누수나 다른 메모리 관련 문제를 방지하는 데 중요합니다. C++은 저수준 메모리 관리 기능을 제공하며, 이를 통해 프로그래머는 메모리를 보다 세밀하게 제어할 수 있습니다.

## \[ Static Memory Allocation ]

### 특징

1. **성능**: 컴파일 시간에 메모리 크기가 결정되므로, 런타임에 메모리 할당과 해제에 따른 오버헤드가 없습니다.
2. **간결성**: 메모리 관리가 덜 복잡하며, 프로그래머가 메모리 할당과 해제를 신경 쓸 필요가 없습니다.
3. **안정성**: 메모리 할당 실패의 가능성이 적으며, 메모리 누수 위험이 없습니다.

### **사용 상황**

* 데이터의 크기가 고정되어 있고, 프로그램의 전체 실행 기간 동안 필요한 경우에 사용합니다.
* 예: 고정된 크기의 배열, 간단한 로컬 변수들.

```cpp
#include <iostream>

int main() {
    int arr[5] = {1, 2, 3, 4, 5}; // 정적 배열 할당
    for (int i = 0; i < 5; i++) {
        std::cout << arr[i] << " ";
    }
    return 0;
}
```

## \[ Dynamic Memory Allocation ]

### 특징

1. **유연성**: 런타임에 메모리 크기를 결정할 수 있어, 필요에 따라 메모리를 할당하고 해제할 수 있습니다.
2. **데이터 크기 변동 가능**: 프로그램 실행 중 데이터의 크기가 변경되는 경우에 유용합니다.
3. **생명 주기 관리**: 동적 할당된 메모리는 함수 호출이 끝나도 사라지지 않으며, 프로그램의 다른 부분에서 사용할 수 있습니다.

### **사용 상황**

* 데이터의 크기가 런타임에 결정되거나 변경될 수 있는 경우에 사용합니다.
* 예: 사용자 입력에 따라 크기가 결정되는 배열, 다양한 크기의 데이터를 다루는 프로그램.

```cpp
#include <iostream>

int main() {
    int size;
    std::cin >> size; // 사용자로부터 배열 크기 입력 받음
    int* arr = new int[size]; // 동적 배열 할당

    for (int i = 0; i < size; i++) {
        arr[i] = i;
        std::cout << arr[i] << " ";
    }

    delete[] arr; // 동적 할당된 메모리 해제
    return 0;
}
```

{% hint style="info" %}
#### 메모리 누수?

1. **메모리 자원의 낭비**: 프로그램이 필요하지 않은 메모리를 계속 점유하게 되면, 시스템에서 사용 가능한 메모리 양이 줄어듭니다. 이는 다른 프로그램이나 시스템의 다른 부분에서 메모리를 필요로 할 때 **충분한 메모리를 제공하지 못하는 상황을 초래**할 수 있습니다.
2. **성능 저하**: 시스템의 사용 가능한 메모리가 줄어들면, 시스템은 디스크 기반의 가상 메모리(swap)를 더 많이 사용하게 됩니다. 이는 **프로그램의 실행 속도를 느리게 하고, 전반적인 시스템 성능을 저하**시킬 수 있습니다.
3. **안정성 문제**: 메모리 누수가 심해지면 시스템에서 사용할 수 있는 메모리가 부족해질 수 있습니다. 이는 결국 프로그램이나 시스템 전체의 충돌로 이어질 수 있으며, **데이터 손실이나 시스템 다운**과 같은 심각한 문제를 유발할 수 있습니다.
4. **장기적인 실행 문제**: 특히 서버와 같이 오랜 시간 동안 계속 실행되는 애플리케이션에서 메모리 누수는 점진적으로 시스템 리소스를 고갈시켜, 결국 서비스가 중단되거나 재시작해야 하는 상황을 만들 수 있습니다.
{% endhint %}

## \[ Smart Pointers]&#x20;

{% hint style="info" %}
스마트 포인터는 자동으로 메모리를 관리해주는 객체입니다. C++11 이후, `std::unique_ptr`와 `std::shared_ptr` 같은 스마트 포인터가 도입되었습니다.
{% endhint %}

### `std::unique_ptr`

`unique_ptr`은 객체에 대한 독점적인 소유권을 가지며, 하나의 `unique_ptr`만이 특정 객체를 소유할 수 있습니다. 이 포인터는 소멸될 때 자동으로 메모리를 해제합니다.

**사용 사례**

* **독점적 소유권 관리**: 어떤 객체가 오직 하나의 소유자에 의해서만 관리되어야 하는 경우에 적합합니다.
* **리소스의 이동**: `unique_ptr`은 복사될 수 없지만 이동할 수 있습니다. 따라서 소유권을 이동시키는 것이 가능합니다.

```cpp
#include <iostream>
#include <memory>

class Calculator {
public:
    Calculator(int value) : value_(value) {}

    void add(int number) {
        value_ += number;
    }

    int getValue() const {
        return value_;
    }

private:
    int value_;
};

int main() {
    // unique_ptr로 Calculator 객체를 생성하고 초기값 10을 설정
    std::unique_ptr<Calculator> calcPtr = std::make_unique<Calculator>(10);

    // calcPtr을 사용하여 덧셈 연산 수행
    calcPtr->add(5); // 현재 값: 15

    // 소유권 이동
    std::unique_ptr<Calculator> calcPtr2 = std::move(calcPtr);

    // calcPtr2를 사용하여 추가 덧셈 연산 수행
    calcPtr2->add(10); // 현재 값: 25

    // 최종 결과 출력
    std::cout << "Final value: " << calcPtr2->getValue() << std::endl;

    // calcPtr2가 소멸되면서 Calculator 객체의 메모리 자동 해제
    return 0;
}

```

### `std::shared_ptr`

`shared_ptr`은 참조 카운팅을 사용하여 여러 포인터가 동일한 객체를 공유할 수 있습니다. 참조 카운트가 0이 되면 객체는 자동으로 해제됩니다.

**사용 사례**

* **공유 소유권 관리**: 여러 부분에서 동일한 객체에 대한 접근이 필요한 경우에 적합합니다.
* **순환 참조 방지**: `weak_ptr`과 함께 사용하여 순환 참조 문제를 해결할 수 있습니다.

```cpp
#include <iostream>
#include <memory>

class Calculator {
public:
    Calculator(int value) : value_(value) {}

    void add(int number) {
        value_ += number;
    }

    int getValue() const {
        return value_;
    }

private:
    int value_;
};

int main() {
    // shared_ptr로 Calculator 객체를 생성하고 초기값 10을 설정
    std::shared_ptr<Calculator> calcPtr = std::make_shared<Calculator>(10);

    // 동일한 객체를 가리키는 또 다른 shared_ptr 생성
    std::shared_ptr<Calculator> calcPtr2 = calcPtr;

    // 첫 번째 포인터를 사용하여 덧셈 연산 수행
    calcPtr->add(5); // 현재 값: 15

    // 두 번째 포인터를 사용하여 덧셈 연산 수행
    calcPtr2->add(10); // 현재 값: 25

    // 최종 결과 출력
    std::cout << "Final value: " << calcPtr->getValue() << std::endl;

    return 0; // shared_ptr들이 소멸되면서 Calculator 객체의 메모리 자동 해제
}

```

### `std::weak_ptr`

**순환 참조(circular reference)는 두 객체가 서로를 `std::shared_ptr`를 통해 참조할 때 발생하는 문제입니다**. 이런 상황에서는 각 객체의 참조 카운트가 절대 0이 되지 않아, 자원이 제대로 해제되지 않고 메모리 누수가 발생합니다. `std::weak_ptr`는 이 문제를 해결하는 데 사용됩니다. `weak_ptr`은 `shared_ptr`과 달리 참조 카운트를 증가시키지 않으므로 순환 참조를 방지할 수 있습니다.

#### 순환 참조의 예

두 클래스 `A`와 `B`가 서로를 `std::shared_ptr`를 사용하여 참조하고 있는 상황을 생각해봅시다.

```cpp
#include <iostream>
#include <memory>

class B; // 전방 선언

class A {
public:
    std::shared_ptr<B> bPtr; // A 클래스는 B 클래스에 대한 shared_ptr을 가지고 있다.
    ~A() { std::cout << "A 소멸\n"; }
};

class B {
public:
    std::shared_ptr<A> aPtr; // B 클래스는 A 클래스에 대한 shared_ptr을 가지고 있다.
    ~B() { std::cout << "B 소멸\n"; }
};

int main() {
    auto a = std::make_shared<A>(); // A의 참조 카운트 = 1
    auto b = std::make_shared<B>(); // B의 참조 카운트 = 1

    a->bPtr = b; // B의 참조 카운트 = 2 (b와 a->bPtr에 의해)
    b->aPtr = a; // A의 참조 카운트 = 2 (a와 b->aPtr에 의해)

    return 0; // main 함수 종료, a와 b의 shared_ptr 소멸
              // 하지만 A와 B의 객체는 소멸되지 않음
              // A의 참조 카운트 = 1 (b->aPtr에 의해)
              // B의 참조 카운트 = 1 (a->bPtr에 의해)
              // 순환 참조로 인해 메모리 누수 발생
}

```

이 코드에서는 `A`와 `B` 객체가 서로를 참조하므로 순환 참조가 발생하고, 이로 인해 메모리 누수가 발생합니다. `main` 함수가 종료되더라도 `A`와 `B`의 소멸자는 호출되지 않습니다.

#### `std::weak_ptr`를 사용한 순환 참조 방지

`weak_ptr`를 사용하여 이 문제를 해결할 수 있습니다. 한 클래스에서 다른 클래스를 참조할 때 `shared_ptr` 대신 `weak_ptr`을 사용합니다.

```cpp
#include <iostream>
#include <memory>

class B; // 전방 선언

class A {
public:
    std::weak_ptr<B> bPtr; // weak_ptr 사용
    ~A() { std::cout << "A 소멸\n"; }
};

class B {
public:
    std::shared_ptr<A> aPtr;
    ~B() { std::cout << "B 소멸\n"; }
};

int main() {
    auto a = std::make_shared<A>(); // A의 참조 카운트 = 1
    auto b = std::make_shared<B>(); // B의 참조 카운트 = 1

    a->bPtr = b; // B의 참조 카운트 변경 없음 (weak_ptr)
    b->aPtr = a; // A의 참조 카운트 = 2 (a와 b->aPtr에 의해)

    // 함수 종료 시
    return 0; // a와 b 소멸. A의 참조 카운트 = 1, B의 참조 카운트 = 0.
              // B가 소멸되면서 B의 weak_ptr도 해제됨. 이제 A의 참조 카운트 = 0.
              // A와 B 모두 정상적으로 소멸.
}

```

## \[ Memory Leak Prevention ]

메모리 누수는 할당된 메모리가 적절히 해제되지 않아 발생하는 문제입니다. 메모리 누수를 방지하기 위해 다음과 같은 전략을 사용합니다:

### `delete` 사용

`delete` 연산자는 C++에서 동적으로 할당된 메모리를 해제하는 데 사용됩니다. `new` 연산자로 할당된 메모리는 프로그램이 명시적으로 `delete`를 사용하여 해제하지 않으면 자동으로 해제되지 않습니다. 따라서 메모리 누수를 방지하기 위해 사용합니다.

```cpp
#include <iostream>

int main() {
    int* ptr = new int(10); // 동적 메모리 할당
    std::cout << *ptr << std::endl; // 값 사용
    delete ptr; // 메모리 해제
    ptr = nullptr; // 포인터 초기화
}
```

### 스마트 포인터 사용

* **`std::unique_ptr`**: 객체에 대한 단일 소유권을 제공합니다. 객체가 더 이상 필요 없을 때 자동으로 메모리를 해제합니다. 주로 객체의 독점적 소유가 필요할 때 사용됩니다.
* **`std::shared_ptr`**: 참조 카운팅을 기반으로 하는 공유 소유권을 제공합니다. 여러 `shared_ptr`이 동일한 객체를 가리킬 수 있으며, 마지막 `shared_ptr`이 사라질 때 객체가 해제됩니다. 객체를 여러 곳에서 공유해야 할 때 사용됩니다.
* **`std::weak_ptr`**: `shared_ptr`의 순환 참조 문제를 해결하기 위해 사용됩니다. `weak_ptr`은 객체에 대한 참조를 유지하지만, 참조 카운트에 영향을 주지 않습니다.

### RAII(Resource Acquisition Is Initialization) 패턴 활용

RAII 패턴은 객체의 생성자에서 자원을 획득하고, 소멸자에서 자원을 해제하는 프로그래밍 기법입니다. 이 패턴은 자원 누수를 방지하고, 예외 발생 시에도 안전한 자원 관리를 보장합니다.

```cpp
#include <iostream>
#include <vector>

class MyClass {
public:
    MyClass() { std::cout << "자원 획득\n"; }
    ~MyClass() { std::cout << "자원 해제\n"; }
    // 기타 클래스 멤버 함수 및 데이터
};

int main() {
    {
        MyClass obj; // MyClass 생성
        // obj 사용
    } // obj 소멸되면서 자원 자동 해제
    return 0;
}

```

## \[ Conclusion ]

C++에서 효과적인 메모리 관리는 프로그램의 성능과 안정성을 크게 향상시킵니다. 동적 메모리 할당, 스마트 포인터의 사용, 메모리 누수 방지 전략은 C++ 개발자가 반드시 숙지해야 하는 중요한 개념입니다. 이를 통해 메모리 관련 문제를 예방하고, 효율적으로 자원을 관리할 수 있습니다.
