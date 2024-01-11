---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# Template Programming in CPP

## \[ Introduction ]

C++ 템플릿 프로그래밍은 **타입에 독립적인 코드를 작성할 수 있게 해주는 강력한 기능**입니다. 템플릿을 사용하면 다양한 데이터 타입에 대해 동일한 로직을 수행하는 함수나 클래스를 단일 코드로 정의할 수 있습니다. 이는 코드 중복을 줄이고 유지 보수를 용이하게 만들어줍니다..

## \[ Function Templates ]

함수 템플릿은 다양한 타입의 인자를 받을 수 있는 **범용 함수를 정의할 때 사용**됩니다. 템플릿 함수는 하나의 함수 선언으로 **여러 타입의 인자에 대응할 수 있어** 코드의 재사용성을 높입니다.

```cpp
#include <iostream>
using namespace std;

template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    cout << add<int>(10, 20) << endl; // 정수 덧셈
    cout << add<double>(3.14, 2.75) << endl; // 실수 덧셈
    return 0;
}
```

```bash
30
5.89
```

이 코드에서 `add` 함수는 정수, 실수 등 다양한 타입에 대해 **동일한 덧셈 연산**을 수행할 수 있습니다.

## \[ Class Templates ]

클래스 템플릿은 **타입에 독립적인 클래스를 정의하는 데 사용**됩니다. 이를 통해 다양한 데이터 타입을 저장하거나 처리하는 클래스를 하나의 클래스 템플릿으로 정의할 수 있습니다.

```cpp
template <typename T>
class Box {
private:
    T content;
public:
    void setContent(T newContent) {
        content = newContent;
    }
    T getContent() {
        return content;
    }
};

int main() {
    Box<int> intBox;
    intBox.setContent(123);

    Box<string> stringBox;
    stringBox.setContent("Hello World");

    cout << intBox.getContent() << endl;
    cout << stringBox.getContent() << endl;
    return 0;
}
```

```bash
123
Hello World
```

여기서 `Box` 클래스는 어떤 타입의 데이터도 저장할 수 있습니다. `int`나 `string` 타입의 데이터를 처리하는 데 동일한 `Box` 클래스를 사용할 수 있습니다.

## \[ Template Specialization ]

템플릿 특수화는 **특정 타입에 대해 일반 템플릿과 다른 구현을 제공**할 때 사용됩니다. 이는 특정 타입에 대해 최적화된 동작을 구현할 수 있게 해줍니다.

```cpp
template <typename T>
class Printer {
public:
    void print(T value) {
        cout << "일반 값: " << value << endl;
    }
};

template <>
class Printer<string> {
public:
    void print(string value) {
        cout << "문자열: " << value << endl;
    }
};

int main() {
    Printer<int> intPrinter;
    intPrinter.print(123);

    Printer<string> stringPrinter;
    stringPrinter.print("Hello");
    return 0;
}

```

```bash
일반 값: 123
문자열: Hello
```

## \[ Conclusion ]

템플릿 프로그래밍은 C++에서 중요한 부분이며, **재사용 가능하고 유지 보수하기 쉬운 코드를 작성하는 데 큰 도움**이 됩니다. 템플릿을 사용함으로써 타입에 안전하고 유연하며 효율적인 코드를 구현할 수 있습니다.
