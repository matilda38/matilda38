---
title: "c++로 피보나치 짜기"
date: 2017-04-04

---

mission: c++로 피보나치 짜기.

c# 책으로 공부하다가, 피보나치 짜는 문제가 있어서 얼마 전 많이 말아먹은(^^) 현대차 인적성이 떠올랐다. 기본적인 스택/큐/피보나치도 까먹어서 결국 맛있게 말아먹은 시험! 유후!

아무튼, c#은 FiboNacci 클래스를 만들고 정적 변수와 메소드를 만들어서 Program 클래스의 Main 함수에서 부르는 방식으로 처리할 수 있었다. Memo를 위해 Dictionary 자료형을 이용하면 된다.

{% highlight csharp %}
//Rextester.Program.Main is the entry point for your code. Don't change it.
//Compiler version 4.0.30319.17929 for Microsoft (R) .NET Framework 4.5
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

namespace Rextester
{
    public class FiboNacci
    {
        public static Dictionary<int, long>memo = new Dictionary<int,long>();
        public static long fibo(int n){
          if(n<0) return 0;
          if(n==1) return 1;
          if(memo.ContainsKey(n))
            return memo[n];
          else{
            long value = fibo(n-1) + fibo(n-2);
            memo[n] = value;
            return value;
          }
        }
    }
    public class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("FiboNacci");
            Console.WriteLine(FiboNacci.fibo(120));
        }
    }
}
```

c#의 Dictionary 자료형이 어떤 자료형에 대응되는가 (c++에서) 를 찾아보다 보니 map 자료형을 찾을 수 있었다. 헤더에 추가해주고,
{% highlight cplusplus %}
#include<iostream>
#include<cstdlib>
#include<map>

using namespace std;
class Fibonacci
{
public:
  static map<int, long> fibomemo;
  static long fibo(int n);
};

//정적 멤버(static member) 변수는 클래스 외부에서 정의해주어야한다.
map<int, long> Fibonacci::fibomemo;

long Fibonacci::fibo(int n)
{
  if(n < 0) return 0;
  if(n == 1) return 1;

  if(fibomemo.count(n) > 0)
  {
    return fibomemo[n];
  }
  else
  {
    long value = fibo(n-1)+fibo(n-2);
    Fibonacci::fibomemo[n] = value;
    return value;
  }
}

int main()
{
    int num;
    cin >> num;
    cout << Fibonacci::fibo(num);

    return 0;
}

```

혹시 차이점이 눈에 확들어오지 않는다면, class 부분이 짧아진걸 일단 볼 수 있다. 기본 로직은 정확히 동일하지만, c++의 경우 클래스에서 정적 멤버(변수/메소드)를 선언해주고, **클래스 외부에서 정적 멤버를 정의해주어야한다.**

정적 멤버는 모든 객체가 공유하는 멤버다. 멤버 변수/멤버 함수들은 객체마다 하나씩 존재했었고, 객체는 저마다 자신의 멤버를 사용했다. 즉, 보통의 멤버들은 객체가 생성되면서 같이 생성되기 때문에 따로 정의해줄 필요가 없다. 그러나 정적 멤버 변수는 오직 하나만 생성되기 때문에 별도로 정의해주어야 한다.

하지만 정적 멤버 변수와 함수는 클래스에 오직 *하나만* 존재하기 때문에 모든 객체들은 하나의 정적멤버를 같이 사용하게 된다. 따라서 클래스 자체에 관련된 정보/전체 객체와 관련된 정보를 보관하고 다루는 데 유용하게 쓸 수 있다.

*참고: 정적 멤버 함수에서는 클래스의 일반 멤버 변수에 접근 할 수 없다. 어떤 객체의 변수를 의미하는 지 알 수 없기 때문이다.*

아무튼, 가장 큰 차이점은 클래스 외부에 정적 멤버 변수를 정의해주고 써야한다는 점이다. class 명세에서는 선언만 했지 정의해주진 않았기 때문이다. 즉, c#에서는 클래스에서 선언, 정의까지 해주는 데 반해, c++에서는 외부에서 정의해주어야 한다는 점이라고 할 수 있다.

이걸 몰라서 꽤 고생했다.

또 다른 차이점은, c#에서는 객체를 생성할 때,

> public static Dictionary<int, long>memo = new Dictionary<int,long>();
List<string> stringlist = new List<string>();

이런식으로 new 키워드를 쓰지만, c++에서는

> map<int, long> Fibonacci::fibomemo;
vector<int> v;

만으로 객체를 생성가능하다.

이제 학교 가야지.

생각해보니 c++에서의 new 키워드는 new-delete 세트로 동적할당에 사용된다. c언어의 malloc-free 기능이라고 생각하면 된다. (힙 영역 저장)
