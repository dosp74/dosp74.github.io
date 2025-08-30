---
title: "[C++] STL stack"
date: 2025-08-31 01:30:00 +0900
last_mod: 2025-08-31
categories: [C++]
tags: [C++, STL, stack, 백준, 10799번]
---

C++ STL의 stack은 **나중에 들어온 원소가 가장 먼저 나가는 LIFO(Last In, First Out) 구조를 구현한 컨테이너 어댑터(container adapter)** 이다.<br>

`stack` 자체는 컨테이너가 아니며, vector, list, deque와 같은 실제 컨테이너 위에서 stack처럼 동작하도록 만든 인터페이스라고 생각하면 된다.<br>

stack[2]와 같은 임의 접근은 불가능하며, 오직 맨 위(top) 원소를 통해서만 접근이 가능하다.

## 주요 멤버 함수

| 함수명  | 설명                          |
| ------- | ----------------------------- |
| push(x) | 원소 x를 stack의 맨 위에 추가 |
| pop()   | 맨 위 원소 제거 (반환값 X)    |
| top()   | 맨 위 원소 반환               |
| empty() | 스택이 비어 있는지 확인       |
| size()  | 원소 개수 반환                |

<br>

pop()과 top()을 사용할 때는 항상 empty()로 비어 있지 않은지 확인해야 한다. 만약 비어 있는 상태에서 pop()과 top()을 사용한다면 런타임 에러가 발생한다.

## 사용 예시

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> s;

    s.push(10);
    s.push(20);
    s.push(30);

    cout << "현재 top: " << s.top() << "\n";

    s.pop();
    cout << "pop 후 top: " << s.top() << "\n";

    if (!s.empty()) {
        cout << "stack 크기: " << s.size() << "\n";
    }

    while (!s.empty()) {
        cout << s.top() << " ";
        s.pop();
    }

    return 0;
}
```

🔽 출력 :

```plaintext
현재 top: 30
pop 후 top: 20
stack 크기: 2
20 10
```

## 실전

![Image](/assets/images/2025-08-31/2025-08-31-baekjoon-10799.png)

[https://www.acmicpc.net/problem/10799](https://www.acmicpc.net/problem/10799)

문제를 이해하고 잘려진 조각의 총 개수를 구해보자.

```cpp
#include <iostream>
#include <string>
#include <stack>
using namespace std;

int main() {
    string bars;
    cin >> bars;

    stack<char> s;
    int bar = 0;
    int total = 0;

    for (int i = 0; i < bars.size(); i++) {
        if (bars[i] == '(') {
            if (!s.empty() && s.top() == '(') {
                bar++;
            }

            s.push('(');
        }
        else { // bars[i] == ')'
            if (!s.empty() && s.top() == '(') { // 레이저인 경우
                total += bar;
            }
            else if (!s.empty() && s.top() == ')') { // 쇠막대기가 끝나는 경우
                total++;
                bar--;
            }

            s.push(')');
        }
    }

    cout << total;

    return 0;
}
```
