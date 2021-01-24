---
title: "GO data type"
date: 2017-07-22

---

# Go의 데이터 타입

(Java 빨리 배워야되는데 내 udacity ㅠ..)

Data Types
데이터 타입은 변수에 어떤 값이 저장될 수 있는 지 저장하고, 제한한다. 해당 타입에 어떤 종류의 operation이 행해질 수 있는지 rule을 정하기도 한다.

Primitive Data Types
byte, int8, uint8, int16, uint16, float32 등 값(value)를 저장할 때 사용한다. C나 Java에서 short, long등을 사용하는 것과 달리 비트 수를 명확히 지정하므로 난 좀 더 직관적이라고 생각한다. 이 data type들에는 몇가지 규칙이 있는데, 일단 연산에서 서로 data type이 다르면 컴파일 에러가 발생한다. int(num2) 이런식으로 변환해주면 연산시 유용하다. 단, 실수형을 정수형으로 바꾸면 소수점을 버릴 수 있다.

중요한 점은 각 자료형마다 저장 가능한 단위이다.


uint8	부호 없는(unsigned) 8비트, 1바이트 정수	0 ~ 255
uint16	부호 없는 16비트, 2바이트 정수	0 ~ 65535
uint32	부호 없는 32비트, 4바이트 정수	0 ~ 4294967295(대략 10^9 * 4)
uint64	부호 없는 64비트, 8바이트 정수	0 ~ 18446744073709551615
int8	부호 있는(signed) 8비트, 1바이트 정수	-128 ~ 127
int16	부호 있는 16비트, 2바이트 정수	-32768 ~ 32767
int32	부호 있는 32비트, 4바이트 정수	-2147483648 ~ 2147483647
int64	부호 있는 64비트, 8바이트 정수	-9223372036854775808 ~ 9223372036854775807
byte	uint8과 크기가 동일, 바이트 단위로 저장할 때 사용

[출처](http://pyrasis.com/book/GoForTheReallyImpatient/Unit08)

당연히 해당 자료형이 담을 수 있는 것보다 큰 값을 대입하면 안된다. 예를 들어 input값이 10^10정도라고 한다면, uint64/int64는 써줘야 해당 값을 담을 수 있겠지.

var num2 byte = 0x32
byte에는 보통 16진수, 문자 값으로 저장한다. 소켓통신을 통해 바이너리를 주고 받거나, 바이너리 파일에서 읽고 쓸때 자주 사용한다.

HackerRank에서 푼 예제를 첨부한다.
{% highlight go%}
package main

import (
    "fmt"
    "os"
    "bufio"
    "strconv"
)

func main() {
    var i uint64 = 4
    var d float64 = 4.0
    var s string = "HackerRank "

    scanner := bufio.NewScanner(os.Stdin)
    // file등 input byte 를 읽을 때 편리한 Scanner 객체이다
   var sec uint64
   var dob float64
   var str string
   value := make([]string, 3)
   var count int = 0
   
   //scanner는 기본적으로 input을 split함으로써 여러개의 token을 만들어낸다. split function은 설정할 수 있다. default는 \n 이다.
   //scanner.Scan은 더 읽을 Token이 남았을 때 True를 반환한다. 다 읽으면 False를 반환한다
   for scanner.Scan() {
       value[count] = scanner.Text()
       count++
   }

   sec, _ = strconv.ParseUint(value[0], 10, 64)
   dob, _ = strconv.ParseFloat(value[1], 64)
   str = value[2]
   // Print the sum of both integer variables on a new line.
   fmt.Printf("%d\n", i+sec)

   // Print the sum of the double variables on a new line.
   fmt.Printf("%.1f\n", d+dob)

   // Concatenate and print the String variables on a new line
   fmt.Printf("%v", s+str)
   // The 's' variable above should be printed first.
{% endhighlight%}
