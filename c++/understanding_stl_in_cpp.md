---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# Understanding STL in CPP

## \[ Introduction ]

C++의 **표준 템플릿 라이브러리(Standard Template Library, STL)**는 프로그래밍에 있어 필수적인 다양한 컨테이너, 반복자, 알고리즘을 제공합니다. 이 장에서는 STL의 핵심 구성 요소를 설명하고 그 중요성을 탐구합니다.

## \[ Containers ]

STL 컨테이너는 다양한 **데이터 구조를 구현**한 것으로, 데이터를 저장하고 관리하는 데 사용됩니다.

### 1. `std::vector`

* **특징**: 동적 배열로, 요소에 대한 빠른 접근이 가능하며, 끝에 요소를 추가하거나 제거하는 작업이 효율적입니다.
* **적합한 상황**:
  * 요소에 대한 임의 접근이 빈번하게 필요할 때
  * 데이터의 크기가 변동될 가능성이 있지만, 중간에 요소를 삽입하거나 삭제하는 경우는 드물 때
  * 예: 데이터의 리스트를 저장하고, 이들을 빈번히 순차적으로 읽어야 할 때

### 2. `std::list`

* **특징**: 이중 연결 리스트로, 어느 위치에나 요소를 추가하거나 삭제하는 데 효율적입니다.
* **적합한 상황**:
  * 리스트 중간에 요소를 자주 삽입하거나 삭제해야 할 때
  * 요소에 대한 빠른 임의 접근이 필요하지 않을 때
  * 예: 사용자의 입력에 따라 자주 수정되는 데이터 순서

### 3. `std::map`

* **특징**: 키-값 쌍을 저장하며, 키에 따라 자동으로 정렬됩니다.
* **적합한 상황**:
  * 키를 기반으로 요소를 저장하고 검색해야 할 때
  * 데이터가 정렬되어 있어야 할 때
  * 예: 단어와 그 정의를 저장하는 사전 같은 경우

### 4. `std::set`

* **특징**: 중복 없이 요소를 저장하며, 요소는 자동으로 정렬됩니다.
* **적합한 상황**:
  * 중복을 허용하지 않는 유일한 요소들의 집합이 필요할 때
  * 데이터의 정렬이 중요할 때
  * 예: 각각 다른 사용자의 ID 집합 관리

### 5. `std::unordered_map` 및 `std::unordered_set`

* **특징**: 해시 테이블을 기반으로 구현되어, 평균적으로 상수 시간 내에 데이터에 접근 가능합니다.
* **적합한 상황**:
  * 빠른 검색이 필요하고, 요소의 순서가 중요하지 않을 때
  * 예: 사용자 이름에 따른 데이터 검색

### 6. `std::queue`

* **특징**: `queue`는 **FIFO(First In, First Out) 구조를 가진 컨테이너**로, 요소가 추가된 순서대로 처리됩니다. `queue`는 요소를 뒤(back)에 추가하고, 앞(front)에서 제거하는 특징을 가지고 있습니다. 이는 데이터를 순차적으로 처리하는 데에 최적화된 구조입니다.
* **적합한 상황**:
  * **순차적 처리가 필요할 때**: 데이터가 도착한 순서대로 처리되어야 하는 상황에서 `queue`가 유용합니다. 이는 요소들이 순차적인 순서를 유지하는 것이 중요할 때 적합합니다.
  * **임의 접근이 필요하지 않은 경우**: `queue`는 임의 접근을 지원하지 않으며, 오직 끝에서 추가하고 앞에서 제거하는 작업만 가능합니다. 따라서 중간에 요소를 삽입하거나 접근하는 것이 필요하지 않은 상황에 적합합니다.

### 7. 예제 코드

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <set>
#include <list>
#include <unordered_map>
#include <unordered_set>
#include <queue>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    std::map<std::string, int> ageMap;
    std::set<int> set = {5, 4, 3, 2, 1};
    std::list<int> list = {10, 20, 30, 40, 50};
    std::unordered_map<std::string, int> uoMap;
    std::unordered_set<int> uoSet = {9, 7, 5, 3, 1};
    std::queue<int> q;

    ageMap["Alice"] = 30;
    ageMap["Bob"] = 25;
    uoMap["Charlie"] = 35;
    uoMap["Diana"] = 28;

    // 큐에 요소 추가
    q.push(6);
    q.push(7);
    q.push(8);

    // 벡터 출력
    std::cout << "Vector: ";
    for (int num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 맵 출력
    std::cout << "Map:\n";
    for (const auto& pair : ageMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }

    // 세트 출력
    std::cout << "Set: ";
    for (int num : set) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 리스트 출력
    std::cout << "List: ";
    for (int num : list) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 비순서 맵 출력
    std::cout << "Unordered Map:\n";
    for (const auto& pair : uoMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }

    // 비순서 세트 출력
    std::cout << "Unordered Set: ";
    for (int num : uoSet) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 큐 출력 및 요소 제거
    std::cout << "Queue: ";
    while (!q.empty()) {
        std::cout << q.front() << " ";
        q.pop();
    }
    std::cout << std::endl;

    return 0;
}

```

```bash
Vector: 1 2 3 4 5 
Map:
Alice: 30
Bob: 25
Set: 1 2 3 4 5 
List: 10 20 30 40 50 
Unordered Map:
Charlie: 35
Diana: 28
Unordered Set: 1 3 5 7 9 
Queue: 6 7 8 
```

## \[ Iterators ]&#x20;

반복자는 STL 컨테이너의 요소를 탐색하고 접근하는 데 사용되는 객체입니다.

* STL 컨테이너는 `begin()`과 `end()` 메소드를 통해 반복자를 제공합니다.
* 반복자를 사용하면 컨테이너의 요소에 일관된 방식으로 접근할 수 있습니다.

```cpp
std::vector<int> vec = {1, 2, 3, 4, 5};
for (auto it = vec.begin(); it != vec.end(); ++it) {
    std::cout << *it << " ";
}
```

{% hint style="info" %}
**`std::vector`** 외에도 **`std::list`, `std::set`, `std::map`, 그리고 `std::unordered_map`**과 같은 다른 컨테이너들에서도 반복자를 사용할 수 있습니다.
{% endhint %}

## \[ Algorithms ]

STL 알고리즘은 데이터 처리와 관련된 일반적인 작업을 수행하는 함수 템플릿입니다.

### 1. `sort`

* **사용 상황**: `sort` 알고리즘은 컨테이너의 요소를 **오름차순이나 내림차순으로 정렬할 때** 사용됩니다. 이는 데이터를 정렬된 상태로 유지해야 하는 경우에 유용하며, 검색, 최소/최대값 탐색, 데이터 집계 등의 작업을 보다 효율적으로 만들어줍니다.
* **예시**: 데이터베이스 쿼리 결과 정렬, 보고서에 사용될 데이터 정렬 등

### 2. `find`

* **사용 상황**: `find` 알고리즘은 **주어진 값과 일치하는 첫 번째 요소를 찾을 때 사용**됩니다. 이는 특정 값이나 객체의 존재 여부를 확인하거나, 해당 값의 위치를 찾아야 할 때 유용합니다.
* **예시**: 사용자 입력 값이 목록에 있는지 확인, 특정 조건을 충족하는 요소 탐색 등

### 3. `transform`

* **사용 상황**: `transform` 알고리즘은 컨테이너의 **각 요소에 함수나 연산을 적용**하여 결과를 다른 컨테이너에 저장하거나 기존 컨테이너를 수정할 때 사용됩니다. 이는 데이터 처리, 수정, 변환 작업에 적합합니다.
* **예시**: 데이터의 단위 변환, 조건에 따른 요소 값 변경 등

```cpp
#include <algorithm>
#include <vector>
#include <iostream>
#include <functional>

int main() {
    std::vector<int> vec = {4, 1, 3, 5, 2};

    // 벡터 정렬
    std::sort(vec.begin(), vec.end());

    // 정렬된 벡터 출력
    std::cout << "Sorted vector: ";
    for (int num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 값 3 탐색
    auto it = std::find(vec.begin(), vec.end(), 3);
    if (it != vec.end()) {
        std::cout << "Found value 3 at position: " << std::distance(vec.begin(), it) << std::endl;
    } else {
        std::cout << "Value 3 not found." << std::endl;
    }

    // 벡터의 각 요소에 10을 더함
    std::transform(vec.begin(), vec.end(), vec.begin(), [](int x) { return x + 10; });

    // 변환된 벡터 출력
    std::cout << "Transformed vector: ";
    for (int num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}

```

```bash
Sorted vector: 1 2 3 4 5
Found value 3 at position: 2
Transformed vector: 11 12 13 14 15
```

## \[ Conclusion ]

STL은 C++ 프로그래밍에서 **데이터를 효율적으로 처리하고 알고리즘을 구현**하는 데 필수적인 요소입니다. 컨테이너, 반복자, 알고리즘은 C++ 프로그래밍의 기본이며, 이들을 이해하고 사용하는 것은 개발자에게 매우 중요합니다. STL을 통해 개발자는 복잡한 데이터 구조와 알고리즘을 쉽게 구현하고, 코드의 재사용성과 효율성을 높일 수 있습니다.
