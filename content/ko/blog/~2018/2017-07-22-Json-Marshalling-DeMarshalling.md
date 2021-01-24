---
title: "GO interface와 JSON"
date: 2017-07-22

---

Go 함수들에서 interface{}를 parameter로 받는 일은 꽤 흔한 일이다. 가장 대표적인 예가

>func Println(a ...interface{}) (n int, err error)

이는 interface 여러개를 input으로 받고, 해당 interface의 타입에 맞게 출력을 해준다.
```go
var a = "hello"
var c = 5

fmt.Println(a)//string 타입 a
fmt.Println(c)//int 타입인 c
{% endhighlight%}

>$go run practice.go
hello
5

run 하면 위와 같이 string, int 값을 Println에 넣었을 때 알아서 적합한 형태로 출력해준다. 함수내에서 interface{}파라미터로 들어온 값의 타입과 값을 판단하여 출력해주기 때문. interface에 관한 예제를 몇 개 더 해보자.
```go
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}
```
func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}
```
func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
{% endhighlight%}
위 예제는 error에 관한 건데, 일반적으로 go programming에서는 error를 리턴하는 경우가 흔하다.
fmt 패키지가 Println을 통해 err 객체를 출력할때, Error 인터페이스를 구현하고 있는 지 찾는다고 한다.

> type error interface {
    Error() string
}

여기서도 마찬가지로 err 객체가 Error 인터페이스를 구현하고 있는지, Error 메소드를 가지는 지 찾아보면,
```
func (e *MyError) Error() string {}
```
MyError 구조체는 이 메소드를 가진다.(composition) 따라서, 위 메소드가 호출된다.
```go
package main

import (
	"fmt"
)
type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string{
	return fmt.Sprintf("cannot Sqrt negative number : %g", e)
}

func Sqrt(x float64) (float64, error) {
	if x < 0{
	  return 0, ErrNegativeSqrt(x)
	}
	return 0, nil
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}
{% endhighlight%}
Exercise 예제를 하나 더 보면, ErrNegativeSqrt 라는 이름의 타입을 하나 정의한다. 그럼, sqrt(-2)를 호출했을때, if문에 의해 ErrNegativeSqrt 타입의 객체를 반환하게 된다. fmt가 Println을 수행할 때, 위 객체의 Error 인터페이스를 찾고, Error 메소드를 구현하고 있으므로, 해당 메소드를 출력해주면 된다.

## Json Marshall and UnMarshall
참고) [json by example](https://gobyexample.com/json)

인터페이스를 공부하게 된 계기가 되어준게 JSON string으로의 인코딩이었다. Go data structure 타입의 객체를 Json으로 인코딩하여 API Response를 날려줘야하므로, standard library인 encoding/json을 이용해야했다.
json.Marshal(var interface{}) 함수는 interface{}형태의 파라미터를 받아 해당 변수의 type, value값을 판단하여 json 형태로 encode해준다.

json.Unmarshal은 당연히 그 반대과정인 decoding을 수행하는데, 예제에도 나와있듯 decode된 데이터를 담을 dat이라는 변수를 선언해준다. 이 변수는 string key에 interface{} value값을 가지는데, byt로 들어온 json string 값을 decode하여 dat의 자료구조에 적합하게 집어넣어준다. interface{}로 선언한 까닭에 여러가지 값이 value로 들어갈 수 있다.

```go
var dat map[string]interface{}
//Here’s the actual decoding, and a check for associated errors.
//go 에서는 이런식으로 error를 체크해주는것이 좋다.
    if err := json.Unmarshal(byt, &dat); err != nil {
        panic(err)
    }
    fmt.Println(dat)
{% endhighlight%}

참고 예제에 나와있듯, decode된 객체의 값들을 사용하기 위해서 적합한 값으로 cast해줘야한다. (interface{}로 value를 받았으므로)     
```go
num, ok := dat["num"].(float64)
if ok{
    fmt.Println(num)
}
{% endhighlight%}
nested data 구조에 접근하기 위해서는 연속적인 cast를 필요로 할 수 있다.

```go
  strs := dat["strs"].([]interface{})
  str1 := strs[0].(string)
  fmt.Println(str1)
{% endhighlight%}
예상되는 타입으로 cast하고 해당값을 자유롭게 go 에서 사용할 수 있다.

참고) go tour
요약) ok 없이 interface의 type assertion을 수행하면, 실제로 변환이 불가능할땐 panic이 발생한다. ok가 있으면 interface가 해당 타입을 가지면, true 아니면 false가 되고 t는 해당 타입 T의 zero value가 되고 panic이 발생하지 않는다.
