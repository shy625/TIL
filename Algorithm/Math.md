# Math

## Euclidean Algorithm (유클리드 호제법)

### 개념

두 자연수 또는 두 다항식의 최대공약수를 구하는 알고리즘이다.

두 자연수 a, b가 있을 때 (단, a > b), a를 b로 나눈 나머지를 r이라 하면 a와 b의 최대공약수는 b와 r의 최대공약수와 같다.

> `gcd(a, b) == gcd(b, r)`

위 성질을 이용해 b를 r로 나눈 나머지 r’을 구하고, r을 r’로 나눈 나머지 r’’을 구하는 과정을 반복하면 나머지가 0이 되는 순간의 나누는 수가 a, b의 최대공약수가 된다.

주어진 자연수까지의 모든 자연수로 두 자연수를 나누어보는 O(N) 알고리즘과 달리, 유클리드 호제법은 나머지로 나누면서 진행되기 때문에 O(logN)의 시간복잡도를 가진다.

위 과정을 두 다항식에도 동일하게 적용하여 두 다항식의 최대차수공약다항식을 구할 수 있다.

### 예시

두 자연수 1071과 1029가 있을 때, 유클리드 호제법을 적용하면,

- 1071 % 1029 = 42
- 1029 % 42 = 21
- 42 % 21 = 0

따라서 두 자연수 1071과 1029의 최대공약수는 21임을 알 수 있다.

### 코드

#### 반복

- Java

    ```java
    public int euclideanGCD(int a, int b) {
        int r = 0;
        while (r != 0) {
            r = a % b;
            a = b;
            b = r;
        }
        return a;
    }
    ```

#### 재귀

- Java

    ```java
    public int euclideanGCD(int a, int b) {
        if (b == 0) {
            return a;
        }
        return euclideanGCD(b, a % b);
    }
    ```

재귀의 경우 StackOverflowError의 위험이 있기 때문에 반복을 사용하는 것이 좋다.

### 참고

- [위키백과 - 유클리드 호제법](https://ko.wikipedia.org/wiki/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C_%ED%98%B8%EC%A0%9C%EB%B2%95)
- [나무위키 - 유클리드 호제법 (알고리즘으로서의 성능)](https://namu.wiki/w/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C%20%ED%98%B8%EC%A0%9C%EB%B2%95#s-3.4)