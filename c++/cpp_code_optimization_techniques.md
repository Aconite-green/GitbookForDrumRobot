---
cover: >-
  ../.gitbook/assets/images_kimku1018_post_43d1fa08-3a92-40c7-84cc-ceefbe3be879_스크린샷
  2021-09-08 오후 7.48.45.png
coverY: 0
---

# CPP Code Optimization Techniques

## \[ Introduction ]

C++ 코드 최적화는 프로그램의 실행 속도를 빠르게 하고, 메모리 사용을 최소화하는 과정을 포함합니다. 최적화는 시스템의 자원을 효율적으로 활용하고, 성능을 극대화하기 위해 필수적입니다.

## \[ Performance Optimization ]

### 1. 루프 최적화

루프 최적화의 주된 목적은 루프 내에서 수행되는 연산의 수를 줄여 프로그램의 실행 시간을 단축시키는 것입니다. 이는 다음과 같은 방법으로 달성할 수 있습니다:

**불필요한 연산 제거**: 루프 내에서 반복적으로 수행되지 않아도 되는 연산을 루프 밖으로 이동시킵니다.

```cpp
for (int i = 0; i < n; ++i) {
    result += data[i] * factor; // factor가 변하지 않는다면 루프 밖으로 이동
}
```

이 예시에서, `factor`는 **루프 내에서 변하지 않는 값**입니다. 따라서, 매 반복마다 `factor`에 대한 연산을 수행할 필요가 없습니다. 이 연산을 루프 밖으로 이동시키면, 루프의 각 반복에서 수행되는 **연산 수가 줄어들어 전체적인 성능이 향상됩니다.**

**루프 언롤링(loop unrolling)**: 루프 언롤링은 루프의 반복 횟수를 줄이고 각 반복에서 수행되는 작업의 양을 늘려서 프로그램의 실행 속도를 향상시키는 최적화 기법입니다. 이는 루프 오버헤드를 감소시키고, CPU 파이프라이닝 및 캐시 활용을 개선하는 데 도움이 됩니다.

```cpp
// 원래 루프
for (int i = 0; i < n; ++i) {
    process(data[i]);
}

// 언롤링된 루프
for (int i = 0; i < n; i += 4) {
    process(data[i]);
    process(data[i + 1]);
    process(data[i + 2]);
    process(data[i + 3]);
}

```

이 예시에서 원래 루프는 `n`번 반복되지만, 언롤링 후에는 `n/4`번만 반복됩니다. 이렇게 하면 각 반복에서 루프 조건 검사와 증감 연산의 횟수가 줄어들어 성능이 향상됩니다.

{% hint style="warning" %}
루프 언롤링은 코드 크기를 증가시킬 수 있으므로 적절한 수준에서 이루어져야 합니다. **과도한 언롤링은 캐시 효율을 떨어뜨리고 코드의 가독성을 저하시킬 수 있습니다.**
{% endhint %}

***

### 2. 컴파일러 자동 최적화 (Makefile 사용 시)

C++에서 `makefile`을 사용하여 코드를 컴파일 할 때, 컴파일러 최적화 옵션을 활용하여 자동으로 코드를 최적화할 수 있습니다.

**g++ 컴파일러의 최적화 옵션**

* **`-O1`, `-O2`, `-O3`**: 성능 최적화 수준을 설정합니다. `-O2`는 균형 잡힌 최적화를 제공하며, `-O3`는 더 공격적인 최적화를 수행합니다.
* **`-Os`**: 코드 크기를 최소화하면서도 성능을 향상시킵니다.
* **`-Ofast`**: 표준을 벗어나는 최적화를 포함하여 최대한의 성능을 추구합니다.

```makefile
CC=g++
CFLAGS=-O2

all: program

program: main.o utils.o
    $(CC) $(CFLAGS) -o program main.o utils.o

main.o: main.cpp
    $(CC) $(CFLAGS) -c main.cpp

utils.o: utils.cpp
    $(CC) $(CFLAGS) -c utils.cpp

clean:
    rm -f *.o program
```

이 `makefile`은 `-O2` 최적화 옵션을 사용하여 `main.cpp`와 `utils.cpp` 파일을 컴파일합니다.

{% hint style="info" %}
### 공격적인 최적화 (`-O3`)

* **특징**: `-O3`는 최대한의 성능 향상을 목표로 하며, 이를 위해 코드 크기와 컴파일 시간을 희생할 준비가 되어 있어야 합니다.
* **적용되는 최적화**:
  * `-O2`에서 적용되는 모든 최적화
  * 더 공격적인 인라이닝 및 루프 변환
  * 벡터화(여러 데이터에 대한 연산을 동시에 수행)
  * 추가적인 플로팅 포인트 최적화
* **용도**: 성능에 매우 민감한 애플리케이션(예: 고성능 컴퓨팅, 과학적 계산)에 사용됩니다. 하지만, **컴파일 시간이 길어지고 실행 파일 크기가 커질 수 있으며**, 특정 상황에서는 예기치 않은 부작용이 발생할 수 있습니다.
{% endhint %}

***

### 3. 알고리즘 최적화

<details>

<summary>1. 데이터 구조 최적화: 해시 테이블 사용</summary>

```cpp
// 상황: 웹 서버에서 효율적인 사용자 조회를 위해 사용될 수 있음
#include <unordered_map>
#include <string>
#include <iostream>

int main() {
    std::unordered_map<std::string, int> userAge;
    userAge["Alice"] = 30;
    userAge["Bob"] = 25;

    // 빠른 조회
    std::cout << "Alice's age: " << userAge["Alice"] << std::endl;
}

```

</details>

<details>

<summary>2. 동적 프로그래밍: 피보나치 수열</summary>

```cpp
// 상황: 계산이 복잡한 수학적 연산을 최적화하는 데 사용될 수 있음
#include <iostream>
#include <vector>

int fibonacci(int n, std::vector<int>& memo) {
    if (n <= 1) return n;
    if (memo[n] == 0) {
        memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);
    }
    return memo[n];
}

int main() {
    int n = 10;
    std::vector<int> memo(n + 1, 0);
    std::cout << "Fibonacci of " << n << " is " << fibonacci(n, memo) << std::endl;
}

```



</details>

<details>

<summary>3. 분할 정복: 퀵 정렬</summary>

```cpp
// 상황: 대규모 데이터 세트의 정렬에서 사용될 수 있음
#include <iostream>
#include <vector>

void quickSort(std::vector<int>& arr, int left, int right) {
    int i = left, j = right;
    int pivot = arr[(left + right) / 2];
    while (i <= j) {
        while (arr[i] < pivot) i++;
        while (arr[j] > pivot) j--;
        if (i <= j) {
            std::swap(arr[i], arr[j]);
            i++; j--;
        }
    }
    if (left < j) quickSort(arr, left, j);
    if (i < right) quickSort(arr, i, right);
}

int main() {
    std::vector<int> data = {9, 7, 5, 11, 12, 2};
    quickSort(data, 0, data.size() - 1);
    for (int n : data) std::cout << n << " ";
}

```



</details>

<details>

<summary>4. 탐욕 알고리즘: 다익스트라 알고리즘</summary>

```cpp
// 다익스트라 알고리즘: 네트워크 라우팅, 게임 내 최단 경로 찾기 등에 사용될 수 있음
#include <iostream>
#include <vector>
#include <limits>
#include <set>

const int MAX = std::numeric_limits<int>::max();

void dijkstra(const std::vector<std::vector<int>>& graph, int src) {
    int V = graph.size(); // 그래프의 정점 수
    std::vector<int> dist(V, MAX); // 각 정점까지의 최단 거리를 저장하는 배열
    std::set<std::pair<int, int>> set; // 정렬된 (거리, 정점) 쌍을 저장하는 세트

    dist[src] = 0; // 시작 정점의 거리는 0
    set.insert({0, src}); // 시작 정점을 세트에 삽입

    while (!set.empty()) {
        int u = set.begin()->second; // 세트의 첫 번째 정점 (최단 거리 정점)
        set.erase(set.begin()); // 세트에서 정점 제거

        // 인접한 모든 정점을 확인
        for (int v = 0; v < V; v++) {
            // 경로가 있고, 최단 거리가 갱신되는 경우
            if (graph[u][v] && dist[u] != MAX && dist[u] + graph[u][v] < dist[v]) {
                if (dist[v] != MAX) {
                    set.erase(set.find({dist[v], v}));
                }
                dist[v] = dist[u] + graph[u][v];
                set.insert({dist[v], v});
            }
        }
    }

    // 결과 출력
    for (int i = 0; i < V; i++)
        std::cout << "Distance from " << src << " to " << i << " is " << dist[i] << std::endl;
}

int main() {
    std::vector<std::vector<int>> graph = {
        {0, 4, 0, 0, 0, 0, 0, 8, 0},
        {4, 0, 8, 0, 0, 0, 0, 11, 0},
        {0, 8, 0, 7, 0, 4, 0, 0, 2},
        {0, 0, 7, 0, 9, 14, 0, 0, 0},
        {0, 0, 0, 9, 0, 10, 0, 0, 0},
        {0, 0, 4, 0, 10, 0, 2, 0, 0},
        {0, 0, 0, 14, 0, 2, 0, 1, 6},
        {8, 11, 0, 0, 0, 0, 1, 0, 7},
        {0, 0, 2, 0, 0, 0, 6, 7, 0}
    };
    dijkstra(graph, 0);
}
``

```



이 코드에서는 다익스트라 알고리즘을 사용하여 주어진 그래프에서 시작 정점부터 다른 모든 정점까지의 최단 거리를 계산합니다. 탐욕 알고리즘의 일종인 다익스트라 알고리즘은 각 단계에서 가장 비용이 낮은 경로를 선택함으로써, **전체적인 최단 경로를 찾아냅니다**.

* `std::vector<std::vector<int>> graph`: 이중 벡터를 사용하여 그래프의 인접 행렬을 나타냅니다.
* `std::vector<int> dist(V, MAX)`: 각 정점까지의 최단 거리를 저장합니다. 초기에는 모든 거리를 최대값(`MAX`)으로 설정합니다.
* `std::set<std::pair<int, int>> set`: 정점과 그 정점까지의 거리를 쌍으로 저장하는 세트입니다. 이 세트는 항상 최단 거리 정점을 먼저 처리하도록 도와줍니다.

</details>

<details>

<summary>5. 메모이제이션: 복잡한 재귀 함수 최적화</summary>

```cpp
// 메모이제이션: 재귀적 알고리즘의 효율을 높이는 데 사용될 수 있음
#include <iostream>
#include <unordered_map>

int complexRecursion(int n, std::unordered_map<int, int>& memo) {
    if (n <= 1) return n; // 기본 경우
    if (memo.find(n) == memo.end()) {
        // 결과가 저장되지 않은 경우, 계산 후 저장
        memo[n] = complexRecursion(n - 1, memo) + complexRecursion(n - 2, memo);
    }
    return memo[n]; // 저장된 결과 반환
}

int main() {
    std::unordered_map<int, int> memo;
    int result = complexRecursion(10, memo); // 10번째 값을 계산
    std::cout << "Result: " << result << std::endl;
}

```

</details>

<details>

<summary>6. 벡터화 및 병렬화: SIMD 지시어 사용</summary>

```cpp
// 벡터화 및 병렬화: 대량의 데이터 연산에 사용될 수 있음
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> data = {1, 2, 3, 4, 5};

    // 벡터화: 데이터의 각 요소에 동시에 연산을 적용
    std::transform(data.begin(), data.end(), data.begin(), [](int x) {
        return x * 2; // 각 요소를 두 배로 증가
    });

    // 병렬화된 결과 출력
    for (auto& n : data) {
        std::cout << n << " ";
    }
    std::cout << std::endl;
}
```

이 코드에서는 `std::transform`을 사용하여 벡터의 각 요소를 두 배로 증가시킵니다. **이는 데이터의 벡터화 및 병렬 처리를 예시로 보여줍니다**. 이 기법은 이미지 처리, 대규모 수치 연산, 실시간 데이터 처리 등에서 효과적으로 사용됩니다.

</details>

## \[ Memory Optimization ]

메모리 최적화는 프로그램의 메모리 사용량을 줄이는 것을 목표로 합니다.

```cpp
#include <iostream>
#include <vector>

int main() {
    // std::vector를 사용하여 동적 배열 구현
    std::vector<int> data;

    // 예상되는 최대 크기만큼 미리 메모리 할당
    data.reserve(1000); // 데이터 구조 최적화 및 메모리 할당 최적화

    // 데이터 추가: reserve()로 인해 빈번한 메모리 재할당을 방지
    for (int i = 0; i < 500; ++i) {
        data.push_back(i);
    }

    // 데이터 사용 및 출력
    for (int value : data) {
        std::cout << value << " ";
    }
    std::cout << std::endl;

    return 0;
}

```

**설명**: 이 코드에서는 `std::vector`를 사용하여 동적 배열을 구현합니다. `reserve` 메소드를 사용하여 예상되는 최대 크기만큼 미리 메모리를 할당함으로써, `vector`의 자동 크기 조정에 따른 메모리 재할당을 방지합니다. 이는 데이터 추가 시 성능을 향상시키고 메모리 사용을 최적화합니다.

## \[ Profiling Tools ]

\
프로파일링 도구는 프로그램의 성능 분석에 필수적인 도구입니다. 이 도구들은 프로그램의 실행 시간, 메모리 사용량, 함수 호출 횟수 등 다양한 성능 지표를 측정하고 분석하는 데 사용됩니다. 이를 통해 성능 병목 현상을 식별하고, 최적화할 영역을 파악할 수 있습니다.

**1. gprof**

* **특징**: gprof는 GNU 컴파일러 컬렉션(GCC)의 일부로 제공되는 프로파일링 도구입니다.
* **사용 방법**:
  * 프로그램을 컴파일할 때 `-pg` 옵션을 사용하여 프로파일링 정보를 포함시킵니다.
  * 프로그램을 실행하여 프로파일링 데이터를 생성합니다.
  * `gprof` 명령어를 사용하여 프로파일링 데이터를 분석합니다.
* **분석**: gprof는 함수 호출 횟수, 실행 시간 등을 보고합니다. 이를 통해 프로그램의 성능 병목 지점을 식별할 수 있습니다.

```bash
gcc -pg -o myprogram myprogram.c
./myprogram
gprof myprogram gmon.out > analysis.txt
```

\
프로파일링 도구는 프로그램의 성능 분석에 필수적인 도구입니다. 이 도구들은 프로그램의 실행 시간, 메모리 사용량, 함수 호출 횟수 등 다양한 성능 지표를 측정하고 분석하는 데 사용됩니다. 이를 통해 성능 병목 현상을 식별하고, 최적화할 영역을 파악할 수 있습니다.

#### 프로파일링 도구 사용 예시

**1. gprof**

* **특징**: gprof는 GNU 컴파일러 컬렉션(GCC)의 일부로 제공되는 프로파일링 도구입니다.
*   **사용 방법**:

    * 프로그램을 컴파일할 때 `-pg` 옵션을 사용하여 프로파일링 정보를 포함시킵니다.
    * 프로그램을 실행하여 프로파일링 데이터를 생성합니다.
    * `gprof` 명령어를 사용하여 프로파일링 데이터를 분석합니다.

    ```bash
    bashCopy codegcc -pg -o myprogram myprogram.c
    ./myprogram
    gprof myprogram gmon.out > analysis.txt
    ```
* **분석**: gprof는 함수 호출 횟수, 실행 시간 등을 보고합니다. 이를 통해 프로그램의 성능 병목 지점을 식별할 수 있습니다.

**2. Valgrind**

* **특징**: Valgrind는 메모리 누수, 메모리 관리 오류, 스레드 오류 등을 검출하는 데 사용됩니다.
* **사용 방법**:
  * 별도의 컴파일 옵션 없이, 일반적으로 컴파일한 프로그램을 Valgrind와 함께 실행합니다.
  * Valgrind 도구 중 하나인 `memcheck`를 주로 사용하여 메모리 관련 문제를 분석합니다.

```bash
gcc -o myprogram myprogram.c
valgrind --tool=memcheck --leak-check=yes ./myprogram
```

* **분석**: Valgrind는 메모리 할당 및 해제, 사용되지 않는 메모리 영역 등에 대한 자세한 정보를 제공합니다.

{% hint style="info" %}
**gprof**는 실행 시간과 함수 호출을 분석하는 데 적합하며, **Valgrind**는 메모리 관련 문제를 진단하는 데 유용합니다. 이러한 도구들을 통해 프로그램의 성능을 체계적으로 분석하고 개선할 수 있습니다.
{% endhint %}

## Conclusion

C++에서의 코드 최적화는 다면적인 접근이 필요합니다. 효율적인 알고리즘과 데이터 구조의 선택, 메모리 사용의 최적화, 그리고 성능 측정 및 분석을 위한 프로파일링 도구의 활용이 중요합니다. 이러한 전략들은 코드의 성능을 향상시키고, 리소스 사용을 최적화하여 전반적인 애플리케이션의 효율성을 높이는 데 기여합니다.
