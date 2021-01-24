---
title: "가상화 기술"
date: 2017-03-11

---

알다시피, 클라우드 컴퓨팅의 기반 핵심 기술은 “가상화” 기술이다. Azure에 관한 포스팅을 시작하기 전에, 가상화 기술에 대해서 어느 정도의 지식을 갖고 Azure를 시작하기 바라는 마음에서 포스팅을 계획하였다.

가상화(Virtualization)에 대한 글을 쓰려다 Operating System Concepts 이라는 예전 운영체제 과목 교재를 꺼내게 되었다. 흠 일단 Computing Environments라는 카테고리를 흥미롭게 읽었는데, 역시 책을 읽기 잘했다.

그리고! 영어 책을 읽길 잘했다.


*참고.

Mobile Computing

Computing 환경이 smart-phone이나 tablet인 경우의 computing 을 “모바일 컴퓨팅”이라고 한다. 기존에 가벼운 무게와 휴대성을 위해 screen size나 메모리 용량, 전체적 기능성을 포기했던 것과 달리 최근 모바일 기기들은 오히려 데스크탑에서 힘든 기능을 제공하기도 한다. Physical feature, 즉 하드웨어적 특성이 다르나, GPS, 가속기계, 회전성에서 모바일 기기들은 Unique Feature를 가진다. Navigation 기능을 제공하는 Application이나, 움직임을 감지하여 모바일 기기를 흔들고 기울이며 새로운 인터페이스를 제공하는 게임도 가능하고, 증강현실(Augmented Reality) 어플리케이션에서도 유용하게 쓰일 수 있다. 이런 어플리케이션들은 노트북이나 데스크탑 시스템에서는 개발 되기 어렵다.
 모바일 기기들은 보통 IEEE 표준 802.11 무선 네트워크나 cellular data 네트워크를 이용한다. 메모리의 경우 64gb정도를 제공하는데, 1TB 메모리 용량을 가진 데스크탑이 흔한것과 비교하면 작은 편이다. 프로세서 또한 전원 사용 문제 때문에 작고 느리거나, 보통 데스크탑보다 적은 코어를 가진 프로세서를 사용하는 편이다.
 Apple의 IOS와 Google의 Android가 대표적인 모바일 컴퓨팅 운영체제이고 IOS의 경우 Apple 의 기기에서만 동작하게 디자인 되어있다.

# 가상화(Virtualization)

가상화는 간단하게 말하자면, IT 자원(Resource)을 추상화한 것이다. 여기서 자원은 네트워크, 저장소, 서버등이 될 수 있다. 즉, 물리적인 디바이스에 관계없이 완전히 기능적인 측면, 마치 Application을 사용하듯이 컴퓨팅 자원을 사용할 수 있다는 것이다. 책의 표현을 빌리자면, 운영체제(Operating System, 줄여서 OS) 위에 운영체제들이 마치 일반 응용 프로그램(Application)인 것 처럼 동작(run)할 수 있도록 하는 기술이다. 하지만, 방금 말한 책의 표현은 가상화의 일부이고, 밑에 깔리게 되는 호스트 운영체제 없이 하드웨어, 하이퍼바이저, 그리고 하이퍼바이저 위의 운영체제들만 동작하는 가상화도 존재한다.

내가 참고한 책에서는 가상화(Virtualization)과 에뮬레이션(Emulation)의 개념이 함께 설명되어있는데, 에뮬레이션은 Source CPU와 Target CPU의 타입이 다를때 쓰인다.

예를 들어, Apple이 IBM Power CPU에서 Intel x86 CPU로 CPU를 바꿨을 때, Rosetta라는 emulation 기능을 포함했다고 한다. CPU의 아키텍쳐가 달라졌기 때문에, 기존의 IBM CPU로 컴파일 되도록 디자인된 어플리케이션들이 Intel CPU에서도 실행될 수 있도록 하는 것이다. IBM CPU 기준으로 작성된 모든 Machine-level Instruction들이 target CPU인 Intel CPU의 동등한(Equivalent)한 함수(function)로 한줄 한줄 번역(translate)되어야 하기에 굉장히 무거웠을(heavy) 것이다.

Emulation은 Interpretation이라고 하는, 컴퓨터 언어가 complie 되지 않고, high-level 형태나 중간 형태로 translated 되는 경우에 쓰인다. 대표적으로 자바는 Interpreted 언어이다. Interpretation은 high-level language가 native-CPU instruction으로 번역될 때, 해당 언어가 native하게 동작하는 이론적인 가상 머신을 emulate하는 emulation의 한 형태이다. 따라서 Java Virtual Machine, JVM이 쓰인다. 이렇다보니 당연히 Java는 C보다 느리지만, Platform Independence를 가질 수 있다.

예전에 포켓몬 에뮬레이터를 이용하여 포켓보이용 포켓몬스터 게임이나 피카츄 배구를 실행해본 것이 기억난다. 즉, 해당 플랫폼 또는 아키텍처에서 실행 불가능한 응용 프로그램을 실행가능하도록 만들기 위해 Emulator가 해당 플랫폼에 맞는 Machine Level 언어로 번역하여 해당 플랫폼에 돌아갈 수 있는 Application으로 구현한것이다.

내가 다뤘던 오픈소스 가상화 솔루션 KVM의 경우에는 QEMU라는 하드웨어 에뮬레이터를 사용한다. Virtual Machine들은 에뮬레이트된 하드웨어를 실제 하드웨어처럼 인식하여 사용하는것이다.

정리하자면,

에뮬레이션 해당 OS를 위해 디자인되지 않은 플랫폼(아키텍처)에서 해당 OS를 실행할때. / 예를 들어, MAC OS X를 위해 디자인 된 MacBook Pro에서 Window 용 프로그램을 실행할때 사용 할 수 있다.

다시 가상화로 가자면, 에뮬레이션과 달리 가상화에서는 CPU가 같은 두개의 OS는 함께 구동 될 수 있다.

![그림1](https://nayoonhwang.blob.core.windows.net/newcontainer/%E1%84%80%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B71.png)

위와 같이 hardware가 존재하고 그 위에 Virtual Machine Monitor(또는 Manager)가 구동이 된다. VMM은 CPU, 메모리등 하드웨어 자원을 가상화 하여, 한개의 Physical Hardware 위에 여러 개의 Virtual Machine이 구동되는 것을 가능하게 한다. 그림에 나와 있듯이 VMM은 Hypervisor라고도 한다. Virtual Machine들이 Hardware 자원을 요청할때, VMM(Hypervisor)에게 API call을 날리고 Hypervisor가 Hardware에 직접 접근하여 요청을 수행하여 결과물을 Return 해준다. 각각의 VM들은 격리(Isolated) 되어 있고 각 VM들에게 가상CPU(vCPU), 가상 메모리, 가상 네트워크등 컴퓨팅 자원을 가상화하여 제공하기 때문에 다른 VM들과 마치 다른 환경에 있는 것 같은, 각 VM이 각자의 Physical 환경에서 구동되는 것 같은 느낌을 준다. 이런 가상화 기술을 통해 VM을 쓰면서, 물리 머신과의 차이점을 느끼지 못하게 만들 수 있다.

자, 그럼 이 가상화 기술이 어떻게 클라우드 컴퓨팅에 적용되어있는지 살펴보자.

클라우드 컴퓨팅은 컴퓨팅, 저장소, 어플리케이션까지 네트워크를 통한 서비스로 제공하는 컴퓨팅의 한 형태이다. 이러한 기능은 가상화를 기반으로 만들어졌다. 가상화의 논리적인 확장이라고 볼 수 있는 셈이다.

흔히 클라우트 컴퓨팅은 아래 3가지로 구분할 수 있다. IT 리소스를 네트워크를 통해 제공한다는 점에서는 모두 같지만, 어디까지 제공하느냐, 그 범위에 따라 나뉜다. 쉽게 말하면, 밑으로 갈수록 클라우드 제공자가 더 갖춰놓은 환경이라는 것이다. 반대로 위로 갈수록, 사용자 자율이 더 커진다.

![그림1](https://nayoonhwang.blob.core.windows.net/newcontainer/marker-azure-partner_nxtarx.png)
*지금 이것도 azure 가상 저장소 Blob을 통해 업로드한 후 가져온 그림이다.


IAAS(Infrastructure-As-A-Service) : 가장 작은 범위를 제공하며, 우리에게 친숙한 서비스이다. 가상 머신(VM), 서버, 저장소등 IT 인프라를 제공한다. Microsoft Azure VM Ubuntu 14.04 하나를 구매했다고 하자. 인터넷으로 언제든지 내가 구매한 Ubuntu에 접근하여 마치 내 컴퓨터인것 처럼 사용할 수 있다. 그리고 내가 사용한 만큼만 지불하면 된다. VM에 사용자의 용도에 맞는 환경을 사용자가 직접 셋팅하고 자유롭게 사용할 수 있다.


PAAS(Platform-As-A-Service): 소프트웨어 응용프로그램을 개발, 제공, 관리하기 위한 소프트웨어 환경(software stack)이 갖춰져서 서비스 되는 형태. 예를 들어, Azure Web App을 이용하여 .NET Framework 프로젝트를 비쥬얼 스튜디오 또는 깃, 깃허브와 같은 원격저장소를 통해 업로드하면, 개발자가 배포(Deploy)를 신경쓰지 않아도 된다. Azure가 알아서 배포 환경을 셋팅하고 배포하기 때문이다.

SAAS(Software-As-A-Service): 말 그대로 인터넷으로 소프트웨어 응용프로그램을 사용하는 것으로, Office365, 일정(calendar)등의 예를 들 수 있고, 이는 기존의 응용프로그램들이 통째로 클라우드로 옮겨진 형태라고 보면 된다. 언제 어디서든 접근이 가능하다는 클라우드의 장점이 더해진것이다.

![그림2](https://nayoonhwang.blob.core.windows.net/newcontainer/image_3.png)

