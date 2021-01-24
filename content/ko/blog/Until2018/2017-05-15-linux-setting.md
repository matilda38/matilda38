---
title: "항상 까먹는 linux 초기설정."
date: 2017-05-15

---

이제는 익숙한 ubuntu 깔기.

usb에 ubuntu를 다운받고, Booting Disk로 만든다(흔히 Burn이라고 부른다. 한국말로는 굽는다고 한다)

>Booting Mode
1. UEFI
2. LEGACY
몇 가지 자료를 찾아보니. UEFI에 최적화된 디바이스의 경우 UEFI 부팅이 default 값으로 되어있다고 한다. 아까 UEFI 부팅으로는 되고 LEGACY USB Storage 부팅으로는 안되었던 것이 디바이스 + OS 에러인듯하나, 요즘에는 대부분 UEFI 부팅으로 한다고 하니, 참고해야겠다. UEFI가 조금 더 빠른 부팅방식이라고 한다.

Ubuntu 14.04 버전 설치 완료 후. root로 로그인을 시도했으나, root 권한은 기존에 설정해둔 것이 없다면 password가 없는 상태이다.
따라서 password 를 설정해주어야 하는데, sudo 명령으로 설정할 수 있다.

'''
sudo passwd root
'''
적절한 password를 설정해주고 로그인 완료~! 알다싶이 passwd (USER)이면 해당 user의 비밀번호를 수정하는 것이다.
