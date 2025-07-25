---
title: "[C++] STL vector"
date: 2025-07-25 14:10:00 +0900
last_mod: 2025-07-25
categories: [C++]
tags: [C++, STL, vector]
---

## STL

C++ STL(Standard Template Library)은 자료구조와 알고리즘을 C++의 템플릿 형태로 제공하는 표준 라이브러리로, 효율적이고 재사용 가능한 코드를 작성할 수 있도록 돕는다. STL은 크게 컨테이너(Container), 반복자(Iterator), 알고리즘(Algorithm)으로 구성되어 있으며, 이 중에서 컨테이너는 데이터를 저장하고 관리하는 역할을 한다.

## vector

C++ STL의 vector는 동적 배열(dynamic array)을 구현한 컨테이너로, 가장 널리 사용되는 STL 컨테이너 중 하나이다. std::vector는 배열처럼 연속된 메모리 공간에 데이터를 저장하며, 크기가 자동으로 조절되는 특징을 가지고 있다.

### 기본 정의

```cpp
#include <vector>

std::vector<int> v; // int형을 저장하는 벡터
```

### 주요 특징

| 특징               | 설명                                                      |
| ------------------ | --------------------------------------------------------- |
| 동적 배열          | 크기를 동적으로 조절 (push_back, resize 등)               |
| 임의 접근 지원     | v[i], v.at(i) 사용 가능 (random access O(1))              |
| 연속된 메모리 구조 | 배열처럼 메모리에 연속적으로 저장되어 캐시 효율이 좋음    |
| 자동 메모리 관리   | 내부적으로 new, delete 수행, 직접 메모리 관리할 필요 없음 |
| 타입 안전성        | 템플릿 기반으로 타입에 따라 동작                          |

### 주요 멤버 함수

| 함수명           | 설명                                 |
| ---------------- | ------------------------------------ |
| push_back(value) | 맨 뒤에 value 삽입                   |
| pop_back()       | 맨 뒤 요소 삭제                      |
| size()           | 현재 벡터의 원소 개수 반환           |
| capacity()       | 현재 저장 가능한 최대 원소 개수 반환 |
| resize(n)        | 크기를 n으로 조정                    |
| clear()          | 모든 원소 제거                       |
| empty()          | 비어 있으면 true 반환                |
| at(i)            | 인덱스 i번째 원소 반환 (범위 검사 O) |
| operator[]       | 인덱스 접근 (범위 검사 X)            |
| begin(), end()   | 반복자 반환                          |
| insert()         | 중간 삽입                            |
| erase()          | 중간 삭제                            |
| swap()           | 다른 벡터와 교환                     |
| shrink_to_fit()  | capacity를 size에 맞게 줄임          |

### 예시 코드

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;

    // push_back
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);

    // size, capacity
    cout << "size: " << v.size() << ", capacity: " << v.capacity() << "\n";

    // operator[], at()
    cout << "v[1] = " << v[1] << ", v.at(1) = " << v.at(1) << "\n";

    // resize
    v.resize(5); // 뒤에 0으로 채워짐
    cout << "After resize(5): ";
    for (int n : v) {
        cout << n << " ";
    }
    cout << "\n";

    // insert
    v.insert(v.begin() + 2, 99); // 3번째 위치에 99 삽입
    cout << "After insert(2, 99): ";
    for (int n : v) {
        cout << n << " ";
    }
    cout << "\n";

    // erase
    v.erase(v.begin() + 3); // 4번째 요소 제거
    cout << "After erase(3): ";
    for (int n : v) {
        cout << n << " ";
    }
    cout << "\n";

    // pop_back
    v.pop_back(); // 맨 뒤 요소 제거
    cout << "After pop_back(): ";
    for (int n : v) {
        cout << n << " ";
    }
    cout << "\n";

    // empty
    cout << "Is vector empty? " << (v.empty() ? "Yes" : "No") << "\n";

    // swap
    vector<int> other = {100, 200};
    v.swap(other);
    cout << "After swap with {100, 200}: ";
    for (int n : v) {
        cout << n << " ";
    }
    cout << "\n";

    // shrink_to_fit
    v.resize(1); // size 줄이기
    v.shrink_to_fit(); // capacity 줄이기
    cout << "After resize(1) and shrink_to_fit(): size = " << v.size() << ", capacity = " << v.capacity() << "\n";

    // clear
    v.clear();
    cout << "After clear(): size = " << v.size() << ", empty = " << (v.empty() ? "Yes" : "No") << "\n";

    return 0;
}
```

🔽 출력 :

```plaintext
size: 3, capacity: 4
v[1] = 20, v.at(1) = 20
After resize(5): 10 20 30 0 0
After insert(2, 99): 10 20 99 30 0 0
After erase(3): 10 20 99 0 0
After pop_back(): 10 20 99 0
Is vector empty? No
After swap with {100, 200}: 100 200
After resize(1) and shrink_to_fit(): size = 1, capacity = 1
After clear(): size = 0, empty = Yes
```

### 내부 동작: 메모리 재할당

capacity()는 일반적으로 size()보다 크다.
push_back() 시 capacity()를 초과하면 2배로 크기를 늘린다.
이때 기존 데이터를 새로운 메모리로 복사해야 하므로 O(n)의 시간이 소요된다.<br>

따라서 많은 삽입이 예상되면 **reserve()** 를 통해 미리 메모리를 확보할 수 있다.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int N;
    cin >> N;

    vector<int> v;

    for (int i = 0; i < N; i++) {
        int number;
        cin >> number;
        v.push_back(number);
    }
}
```

보통 입력 받을 때, 알아서 크기가 늘어나므로 이런 식으로 코드를 많이 짠다.
이때,

```cpp
vector<int> v;
v.reserve(10000);
```

대충 N의 크기를 예측해 reserve() 함수를 미리 사용하면 성능 향상을 도모해볼 수도 있다.<br>

주의할 점으로는 중간 삽입/삭제가 O(n)이라는 점이다. 중간에 공간이 필요하거나 비게 되면 그만큼 뒤에 있는 요소들을 옮겨야 하기 때문에 다른 자료구조가 더 적합할 수 있다. (ex. list)

### 요약

| 장점                       | 단점                              |
| -------------------------- | --------------------------------- |
| 임의 접근 속도 빠름 (O(1)) | 중간 삽입/삭제 느림 (O(n))        |
| 메모리 연속적, 캐시 친화적 | capacity 초과 시 재할당 비용 발생 |
