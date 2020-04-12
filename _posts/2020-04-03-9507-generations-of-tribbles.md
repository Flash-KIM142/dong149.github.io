---
title: 9507번 Generations of Tribbles
layout: post
categories:
- 동적계획법
- dp
- 알고리즘
part: algorithm
---

피보나치 문제를 조금 변형한 dp 문제다. 간단하게 손 풀기로 문제를 하나 풀어보았다.


```c++
// Generations of Tribbles
#include <iostream>
using namespace std;

int t, n;
unsigned long long res;
unsigned long long dp[68] = {0};
int main()
{
    cin >> t;
    dp[0] = 1;
    dp[1] = 1;
    dp[2] = 2;
    dp[3] = 4;
    while (t--)
    {
        cin >> n;
        if (dp[n])
        {
            cout << dp[n] << endl;
            continue;
        }
        for (int i = 4; i <= n; i++)
        {
            dp[i] = dp[i - 1] + dp[i - 2] + dp[i - 3] + dp[i - 4];
        }
        cout << dp[n] << endl;
    }
    return 0;
}
```
