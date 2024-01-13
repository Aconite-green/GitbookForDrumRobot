---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# Naming Conventions in CPP

## \[ Introduction ]

* **가독성 향상**: 명확하고 일관된 네이밍 컨벤션은 코드를 더 읽기 쉽게 만듭니다.
* **유지보수 용이성**: 좋은 네이밍은 코드의 목적과 기능을 쉽게 이해할 수 있도록 하여, 유지보수를 용이하게 합니다.
* **협업 효율성**: 일관된 네이밍 규칙을 따르면 팀원 간의 소통이 원활해집니다.

## \[ Variable and Function Naming ]

### Variable Naming

변수는 데이터의 값을 저장하며, 그 이름은 저장된 데이터의 의미를 반영해야 합니다.

#### **규칙**

* **명사 사용**: 변수는 보통 명사나 명사구를 사용해 이름을 지정합니다.
* **의도 명확성**: 변수 이름은 변수가 저장하고 있는 데이터의 목적이나 의도를 명확하게 표현해야 합니다.
* **간결성과 명확성**: 변수 이름은 간결하면서도 충분한 정보를 제공해야 합니다. 너무 긴 이름은 피하되, 변수의 역할을 쉽게 이해할 수 있도록 해야 합니다.

#### **예시**

* `int age;` - 나이를 저장하는 정수형 변수
* `double totalPrice;` - 총 가격을 저장하는 실수형 변수
* `bool isCompleted;` - 작업 완료 여부를 나타내는 불린 변수

### Function Naming

함수는 특정 작업을 수행하는 코드의 블록으로, 그 이름은 수행하는 작업을 나타내야 합니다.

#### **규칙**

* **동사 사용**: 함수 이름은 동사 또는 동사구를 사용해 작업을 설명합니다.
* **수행 작업의 명확성**: 함수 이름은 함수가 수행하는 작업이나 반환하는 결과를 명확하게 표현해야 합니다.
* **일관성 유지**: 프로젝트 내에서 일관된 네이밍 규칙을 사용해야 합니다. 예를 들어, 'get'을 사용하여 값을 반환하는 함수, 'set'을 사용하여 값을 설정하는 함수 등의 규칙이 있을 수 있습니다.

#### **예시**

* `void calculateTotal();` - 총합을 계산하는 함수
* `int getMaxValue();` - 최대값을 반환하는 함수
* `bool checkValidity();` - 유효성을 검사하고 결과를 불린 값으로 반환하는 함수

## \[ Class and Interface Naming ]

### class Naming

클래스 네이밍은 객체지향 프로그래밍에서 특히 중요합니다. 클래스는 데이터와 이를 처리하는 메서드를 함께 묶은 구조체로, 프로그램의 기본 단위를 형성합니다. 클래스의 이름은 그 역할과 기능을 명확히 반영해야 하며, 일관된 규칙을 따라야 합니다.

#### **규칙**

* **명사 또는 명사구 사용**: 클래스는 객체의 템플릿을 정의하므로, 일반적으로 명사 또는 명사구를 사용합니다.
* **대문자로 시작**: 클래스 이름은 대문자로 시작하는 것이 일반적입니다. 이는 클래스를 변수나 함수와 구별하는 데 도움이 됩니다.
* **의도와 목적 명확성**: 클래스 이름은 해당 클래스의 목적이나 역할을 명확하게 표현해야 합니다.

#### **예시**

* `class Car`: '자동차'라는 개념을 나타내는 클래스. 'Car'라는 명사를 사용하여 자동차의 특성과 기능을 갖는 객체를 표현합니다.
* `class NetworkManager`: 네트워크 관리 기능을 수행하는 클래스. 'NetworkManager'라는 명사구를 사용하여 네트워크 관련 작업을 관리하는 역할을 나타냅니다.
* `class UserInterface`: 사용자 인터페이스와 관련된 기능을 제공하는 클래스. 'UserInterface'라는 명사구를 사용하여 사용자와의 상호작용을 처리하는 역할을 나타냅니다.

### Interface Naming

인터페이스 네이밍 컨벤션은 특히 객체지향 프로그래밍에서 중요한 역할을 합니다. C++과 같은 언어에서 인터페이스는 클래스 간의 계약을 정의하고, 특정 클래스가 반드시 구현해야 하는 메서드의 시그니처를 명시합니다. 인터페이스의 이름을 지을 때 명명 규칙을 따르면, 코드의 가독성과 이해도를 높일 수 있습니다.

#### 인터페이스 네이밍 규칙

1. **'I'로 시작하는 규칙**: 이 규칙에서 인터페이스의 이름은 대문자 'I'로 시작합니다. 이는 인터페이스임을 명확히 나타내며, 인터페이스와 구현 클래스를 쉽게 구분할 수 있게 해줍니다.
   * **예시**: `IShape`, `IDatabaseConnector`
2. **'Interface'로 끝나는 규칙**: 이 규칙에서 인터페이스의 이름은 'Interface'로 끝납니다. 이는 인터페이스임을 명확히 하며, 'I'로 시작하는 규칙보다 더 명시적입니다.
   * **예시**: `ShapeInterface`, `DatabaseConnectorInterface`

```cpp
class IShape {
public:
    virtual void draw() = 0;
    virtual void move(int x, int y) = 0;
};

class Circle : public IShape {
public:
    void draw() override {
        // 원 그리기 구현
    }

    void move(int x, int y) override {
        // 원 이동 구현
    }
};

```

이 예제에서 `IShape`는 인터페이스를 나타내며, `Circle` 클래스는 `IShape` 인터페이스를 구현합니다. `IShape`의 'I' 접두어는 이것이 인터페이스임을 나타냅니다.\


## \[ Example ]

```cpp
#include <iostream>
#include <vector>
#include <string>

// 인터페이스 네이밍: 'I'로 시작
class IShape {
public:
    virtual void draw() = 0;
    virtual ~IShape() = default;
};

// 클래스 네이밍: 대문자로 시작하는 명사
class Circle : public IShape {
private:
    int radius;
    std::string color;

public:
    // 생성자: 대문자로 시작하는 명사
    Circle(int initRadius, std::string initColor) : radius(initRadius), color(initColor) {}

    // 메서드 네이밍: 동사로 시작
    void draw() override {
        std::cout << "Drawing a circle of radius " << radius << " and color " << color << std::endl;
    }

    // Getter 메서드 네이밍: 'get'으로 시작
    int getRadius() const {
        return radius;
    }

    std::string getColor() const {
        return color;
    }
};

// 함수 네이밍: 동사로 시작
void processShapes(const std::vector<IShape*>& shapes) {
    for (auto shape : shapes) {
        shape->draw();
    }
}

int main() {
    Circle circle(10, "red");
    std::vector<IShape*> shapes;
    shapes.push_back(&circle);

    // 함수 호출
    processShapes(shapes);

    return 0;
}

```

* **`IShape` 클래스**: 인터페이스로서 'I'로 시작합니다. 이는 인터페이스임을 나타내는 네이밍 컨벤션을 따릅니다.
* **`Circle` 클래스**: 명사를 사용하며 대문자 C로 시작합니다. 이는 클래스 네이밍 컨벤션에 부합합니다.
* **`draw`, `getRadius`, `getColor` 메서드**: `draw`는 동사를 사용하여 작업을 나타내며, `getRadius`와 `getColor`는 'get'으로 시작하여 접근자(accessor) 메서드임을 나타냅니다.
* **`processShapes` 함수**: 함수 이름은 동사로 시작합니다. 이는 함수가 수행하는 작업을 명확히 나타내는 네이밍 컨벤션을 따릅니다.
* **변수 네이밍**: `radius`, `color` 등은 명사를 사용하며 변수의 목적을 명확하게 표현합니다.

## \[ Conclusion ]

C++ 프로그래밍에서 네이밍 컨벤션은 코드의 가독성, 유지보수성, 그리고 협업의 효율성을 크게 향상시키는 중요한 요소입니다. 명확하고 일관된 네이밍 규칙은 다음과 같은 이점을 제공합니다:

1. **코드의 이해도 향상**: 일관된 네이밍 컨벤션을 사용하면, 코드를 처음 접하는 개발자도 빠르게 이해하고, 프로젝트에 기여할 수 있습니다.
2. **유지보수의 용이성**: 명확한 네이밍은 코드의 의도와 기능을 쉽게 파악할 수 있게 하여, 유지보수 과정을 간소화합니다.
3. **효과적인 협업 지원**: 팀 내에서 일관된 네이밍 규칙을 공유하면, 협업 시 발생할 수 있는 오해를 줄이고, 효율적인 커뮤니케이션을 지원합니다.
4. **에러 감소**: 의미 있는 네이밍은 실수를 줄이고, 잠재적인 버그의 발견을 용이하게 합니다.

네이밍 컨벤션의 적용은 단순히 '좋은 습관'을 넘어서, 프로젝트의 성공적인 구현과 지속 가능한 성장을 위한 핵심적인 전략입니다. 따라서, 개발 초기 단계에서부터 명확하고 일관된 네이밍 컨벤션을 수립하고, 이를 지속적으로 준수하는 것이 중요합니다.

\
