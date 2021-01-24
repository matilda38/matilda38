---
title: "c# getter setter 속성"
date: 2017-04-03

---
Property(속성)을 정의 할때 쓰는 개념.
: 겟터 & 셋터
: Get & Set

public 으로 변수를 선언해버리면 외부에서 마음대로 값을 설정해버릴수있고 잘못된값을 넣을 수 있는 가능성 존재.
&
그렇다고 private으로 선언하면 외부에서 아예 값을 건드릴 수 없다. 값을 안전하게 변경하려면 대체 어떻게 해야 하는 것일까?


= Getter setter
```c#
private int width;
public int Width
{
  get {return width;}
  set
  {
    if(value > 0) {width = value;}
    else {Console.WriteLine("너비는 자연수로");}
  }
}
```

보통 일반적인 프레임워크에서 사용할 때는 이렇게 쓴다.

```c#
public int Width { get; set; }
public int Height { get; set; }
```
