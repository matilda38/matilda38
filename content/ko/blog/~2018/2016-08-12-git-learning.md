---
title: "Git!"
date: 2016-08-12
---


프로젝트에서 버전관리를 사용하면 프로젝트의 모든 코드가 중앙의 저장소 Repository에 저장된다. 프로젝트의 스냅샷을 저장한다고 생각하면 된다.

프로젝트 Repo의 주소를 복사하여

> git clone (url)

하면 해당 저장소의 프로젝트를 로컬 저장소로 내려받을 수 있다.(check-out)

또는 내 Project를 원격 저장소로 업로드 하기 위해(check-in)

> git remote add (name) (url)

name은 보통 origin으로 한다. 원격저장소의 이름과 주소를 적어주면 원격저장소를 등록할 수 있고

등록된 원격저장소에 내 로컬 저장소에 있는 내용들(로컬 깃에 커밋완료 된것)

> git push origin (branch-name)

로 업로드할 수 있다.

[sourceTree로 배우기](http://greatgift.tistory.com/36)

[깃 따라하기](http://gitimmersion.com/lab_06.html)

[클래스 라이언](http://class.likelion.net/tutorials/8)

Git focuses on the changes to a file rather than the file itself. When you say git add file, you are not telling git to add the file to the repository. Rather you are saying that git should make note of the current state of that file to be committed later.

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   hello.rb

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.rb

Notice how hello.rb is listed twice in the status. The first change (adding a default) is staged and is ready to be committed. The second change (adding a comment) is unstaged. If you were to commit right now, the comment would not be saved in the repository.

Let’s try that.

GDG 세션에서 배운것도 활용하자.


1. Git Branch

> git branch

branch는 가지라는 뜻이다. 즉, Git에서도 개발 가지이다. 브랜치는 협업에서는 필수이다. 각각의 브랜치에 있을때는 각각의 브랜치 내용만 저장되기 때문에 편하다.

> git checkout -b nayoon/smsSend dev
dev브랜치 에서 nayoon폴더 밑 smsSend 브랜치를 새로 생성하여 이동

> git branch -d dev
dev 브랜치를 삭제한다

2. Git push

> git push --set-upstream origin nayoon/sms-auth
이러면 nayoon폴더 sms-auth branch를 기본 branch로 설정하고 push 할 수 있다.
다음부터 git push 로 하면 자동으로 nayoon/sms-auth로 설정된다


'''
현재 존재하는 브랜치로부터 새로운 브랜치를 파고 싶을때.

First change/checkout into the branch from where you want to create a new branch.For example if you have the following branches like:

master
dev
branch1
So if you want to create a new branch called "subbranch_of_b1" under the branch named "branch1" follow the steps:

## Checkout or change into "branch1"

*branch1로 checkout*
git checkout branch1

*b1의 sub branch인 "subbranch_of_b1"를 생성한다.(b 옵션은 브랜치 생성후 바로 checkout하기 위함이다.)*
git checkout -b subbranch_of_b1 branch1

The above will create a new branch called subbranch_of_b1 under the branch branch1 (Note that  branch1 in the above command isn't mandatory since the HEAD is currently pointing to it, you can precise it if you are on a different branch though)

Now after working with the subbranch_of_b1 you can commit and push or merge it locally or remotely.
'''
![git image](../assets/images/git.jpg)

[깃 특정 커밋의 상태로 돌아가기](https://backlogtool.com/git-guide/kr/stepup/stepup6_3.html)
[깃 기초 명령어](http://crynut84.github.io/2015-06-19/basic-command-git.html)

## Git Ignore 파일 추가하기
.gitignore파일로 git 저장소에 올리고 싶지 않은 파일들을 제외시킬 수 있다. 나의 경우엔

```
.idea/
*.o
bundle/
Gemfile
Gemfile_o
secret/
```
이런식으로 지정해줬는데 IDE 사용시 자동으로 추가되는 파일이나, 루비 젬파일들은 굳이 깃헙에 올려놓을 필요가 없으므로 제외시켰고, o같은 확장자를 가진 파일들은 대개 컴파일 과정에서 생기는 불필요한 파일이므로 제외시켜주었다. secret폴더에는 밖에 보이는 글말고 혼자 보고 싶은 글을 마크다운으로 작성해서 저장해놓기 위해 gitignore에 추가해주었다.

{% highlight bash%}
git rm -r –cached .
git add .
git commit -m “Updated .gitignore”
```

이런식으로 기존 cache를 지우고, gitignore가 업데이트 된것을 add 해준후, commit 해주면 gitignore가 반영된다.
이는 git이 traking하는 파일들을 초기화하는 작업과 같다. 즉, gitignore에 있는 파일/폴더들은 track하지 마라 이 뜻과 같다.

이미 add된 특정 파일만 untrack하기 위한 명령어는

**git rm --cached filename** 라고 한다. [stackoverflow](https://stackoverflow.com/questions/1139762/ignore-files-that-have-already-been-committed-to-a-git-repository)
