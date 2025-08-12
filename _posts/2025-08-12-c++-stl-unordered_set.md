---
title: "[C++] STL unordered_set"
date: 2025-08-12 17:53:00 +0900
last_mod: 2025-08-12
categories: [C++]
tags: [C++, STL, unordered_set]
---

C++ STL의 unordered_set은 중복되지 않는 원소들을 저장하는 해시 기반 컨테이너이다. 다만, 이름 그대로 순서를 보장하지는 않는다.

## 주요 특징

- 중복된 원소는 저장되지 않는다. 예를 들어, 같은 값을 두 번 넣으려고 하면 두 번째 삽입은 무시된다.
- 순서가 없다. 즉, 컨테이너의 원소들은 정렬되지 않는다.
- 내부적으로 해시 테이블을 사용해 평균 O(1) 시간에 삽입, 삭제, 탐색이 가능하다.
- 인덱스 접근이 불가능하며 반복자(iterator)로만 접근 가능

## 주요 멤버 함수

| 함수명      | 설명                                    |
| ----------- | --------------------------------------- |
| insert(val) | val 삽입 (중복일 경우 무시)             |
| erase(val)  | val 삭제                                |
| find(val)   | val의 iterator 반환 (없으면 end() 반환) |
| count(val)  | val의 존재 여부 반환 (0 또는 1)         |
| size()      | 원소 개수 반환                          |
| empty()     | 집합이 비어 있는지 확인                 |
| clear()     | 모든 원소 제거                          |

## 사용 예시

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    unordered_set<int> us;

    us.insert(10);
    us.insert(20);
    us.insert(30);
    us.insert(10); // 중복되므로 무시됨

    if (us.find(20) != us.end()) {
        cout << "20 존재\n";
    }

    us.erase(20);

    for (int v : us) {
        cout << v << " "; // 순서 보장 X
    }

    return 0;
}
```

🔽 출력 :

```plaintext
20 존재
30 10
```

### 정리

빠른 삽입, 삭제, 탐색이 필요하고 순서가 중요하지 않다면 unordered_set이 유리하다. 해시 기반이므로 평균적으로 O(1)의 성능을 발휘하며 정렬된 상태 유지가 필요하면 set을 사용하자.
