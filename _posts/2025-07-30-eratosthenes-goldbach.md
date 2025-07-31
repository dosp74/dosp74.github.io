---
title: "[알고리즘] 소수 판정과 골드바흐 파티션 - 에라토스테네스의 체 활용하기"
date: 2025-07-30 15:24:00 +0900
last_mod: 2025-07-30
categories: [알고리즘]
tags: [소수 판정, 에라토스테네스의 체, 골드바흐 파티션, 백준, 6588번]
---

에라토스테네스의 체는 소수를 효율적으로 찾기 위한 알고리즘이다. 1부터 N까지의 자연수 중에서 소수를 찾고 싶을 때, 다음과 같은 과정을 통해 구한다.

## 알고리즘

1. 2부터 N까지의 모든 정수를 나열한다.
2. 2는 소수이므로 제외하고, 2의 배수를 모두 지운다.
3. 남아있는 수 중에서 가장 작은 수(3)는 소수이므로 제외하고, 3의 배수를 모두 지운다.
4. 이 과정을 √N까지 반복하면, 남아 있는 수는 모두 소수이다.

### 왜 √N까지만 반복할까?

n이 소수가 아니라면 n은 a x b 형태로 표현할 수 있다. 여기서 a와 b는 1과 n 사이의 자연수이다.<br>

이때 a <= b라고 가정하면 a x b = n이므로 항상 a <= √n이 된다. 왜냐하면 a, b 모두 √n보다 클 때 a > √n, b > √n -> a x b > n이 되므로 a x b = n이 성립할 수 없기 때문이다.<br>

따라서 n이 소수가 아니라면 반드시 √n 이하의 어떤 수로 나누어떨어져야 한다. 이는 2부터 √n까지의 수만 확인해서 나누어떨어지는 수가 없으면 n이 소수임을 의미한다.

예로, n = 37이면 √37은 6보다 살짝 큰 수이고, 2, 3, 4, 5, 6까지 나누었을 때 나누어떨어지는 수가 없기 때문에 37은 소수이다.

하나 더 예를 들어보자면 n = 35이면 √35는 6보다 살짝 작은 수이고, 2, 3, 4, 5까지 확인하면 된다. 이 경우는 5로 나누었을 때 나누어떨어지기 때문에 소수가 아니다.

좀 장황하게 설명한 것 같아서 요약하면, n이 소수가 아니라면 n의 약수 중 하나는 반드시 √n 이하이다. 이 이상까지 확인하면 불필요하게 연산을 늘리는 꼴이 되므로 비효율적이다.

## 실전

![Image](/assets/images/2025-07-30/2025-07-30-baekjoon-6588.png)

[https://www.acmicpc.net/problem/6588](https://www.acmicpc.net/problem/6588)

에라토스테네스의 체 알고리즘을 써서 소수들을 구하고 문제에서 요구하는 골드바흐 파티션을 구해보자. 골드바흐 파티션은 짝수를 두 소수의 합으로 나타내는 표현을 말한다.

[C++]

```cpp
#include <iostream>
#include <vector>
using namespace std;

const int MAX = 1000000;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);

    vector<bool> isPrime(MAX + 1, true);
    isPrime[0] = isPrime[1] = false;

    for (int i = 2; i * i <= MAX; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= MAX; j += i) {
                isPrime[j] = false;
            }
        }
    }

    int n;
    cin >> n;

    while (n != 0) {
        bool isGoldbach = false;

        for (int i = 1; i <= n / 2; i++) {
            if (isPrime[i] && isPrime[n - i]) {
                cout << n << " = " << i << " + " << n - i << "\n";
                isGoldbach = true;

                break;
            }
        }

        if (!isGoldbach) {
            cout << "Goldbach's conjecture is wrong.\n";
        }

        cin >> n;
    }

    return 0;
}
```

`vector<bool> isPrime(MAX + 1, true);` 인덱스를 수로 생각하기 위해 MAX에 1을 더한 값 만큼 메모리를 확보하고, 모든 수를 소수로 가정하고 시작한다.

`isPrime[0] = isPrime[1] = false;` 0과 1은 소수가 아니므로 false 처리한다.

`for (int i = 2; i * i <= MAX; i++)` √MAX까지만 확인하면 충분하다.

`for (int j = i * i; j <= MAX; j += i)` 소수 i의 배수들을 지우는 과정인데, i보다 작은 소수들의 배수는 이미 지워졌기 때문에 i \* i부터 시작한다. (좀 더 효율적)

`for (int i = 1; i <= n / 2; i++)` n = p + q를 만족하는 소수쌍 (p, q)에서 p <= q 조건을 추가하면 중복을 방지하고, 더 빠르게 골드바흐 파티션을 찾을 수 있다.

예를 들어 n = 10일 때 (3, 7)과 (7, 3)은 같은 파티션이므로 한 번만 센다. 따라서 i <= n / 2까지만 확인해도 충분하다.

<br>참고로, 에라토스테네스의 체 알고리즘은 O(nloglogn)의 시간 복잡도를 가진다고 한다.
