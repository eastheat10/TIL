# 에라스토테네스의 체

[(백준알고리즘 1929번)](https://www.acmicpc.net/problem/1929)

수학에서 소수를 찾는 방법

> 소수: 1과 자기 자신 2개만 약수로 갖는 자연수(ex. 2, 3, 5, 7, 11, ...)

어떠한 수 n이 주어졌을 때 2부터 n까지 특정 배수를 지워나간다.(이미 지워진 수는 건너 뛴다.)

> 자기 자신은 지우지 않는다.

지워지지 않은 수가 소수이다.

![Sieve_of_Eratosthenes_animation](https://github.com/eastheat10/TIL/blob/main/Algorithm/Sieve_of_Eratosthenes_animation.gif)



```java
boolean [] prime = new boolean[n + 1];
prime[0] = prime[1] = true;		// true는 소수가 아닌 수

for(int i = 2; i * i <= n; i++) {
  if(prime[i])
    continue;
  for(int j = i * i; j <= n; j += i)
    prime[j] = true;
}

```

