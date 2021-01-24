---
title: "git rebase"
date: 2017-07-18

---
# Git

* git rebase -i HEAD~n
ex) git rebase -i HEAD~4
rebase는 이루어진 커밋들에 drop, squash, edit등의 수정작업을 하기 위함이다. 예시로 든 명령은 Head가 가르키고 있는 커밋부터 4개까지 rebase 하겠다는 것인데.


```bash
  pick 1df44db python dashinsert
  pick 3409e57 20170630 friking week
  pick 2dcbf08 .gitignore update
  pick 126181c .
  # Rebase f406725..126181c onto f406725 (4 commands)
  #
  # Commands:
  # p, pick = use commit
  # r, reword = use commit, but edit the commit message
  # e, edit = use commit, but stop for amending
  # s, squash = use commit, but meld into previous commit
  # f, fixup = like "squash", but discard this commit's log message
  # x, exec = run command (the rest of the line) using shell
  # d, drop = remove commit
  #
  # These lines can be re-ordered; they are executed from top to bottom.
  #
  # If you remove a line here THAT COMMIT WILL BE LOST.
  #
  # However, if you remove everything, the rebase will be aborted.
  #
  # Note that empty commits are commented out
{% endhighlight%}

이런식으로 나타난다. 이때 pick이라고 되어있는 commit에 command를 입력해주면, 즉,

```bash
pick 3409e57 20170630 friking week
pick 2dcbf08 .gitignore update
s 126181c .
pick c1c9582 ..
{% endhighlight%}
이렇게 해주면 .이라고 커밋메세지가 써있는 commit은 squash되어 이전의 커밋과 합쳐진다.

```bash
pick 1df44db python dashinsert
pick 3409e57 20170630 friking week
pick 9308320 .gitignore update
reword 47fc658 gitrebase experiment
{% endhighlight%}
또는 이렇게 커밋메세지를 변경해줄수도 있다

* git rm --cached .idea/vcs.xml

```bash
git rm --cached _posts/2017-06-30-friking-week.md
git commit -m "remove an article"
git push
{% endhighlight%}

git에서 추적(stage)하지 않고, ignore할 수 있는 방법에는 .gitignore에 입력해주는 방법도 있지만, git rm을 이용하는 방법도 있다. 이때 --cached 옵션을 이용하면 실제로 파일을 삭제하지 않고, stage만 피하게 해줄 수 있다. git rm만 해주면 실제로 파일이 삭제되므로 주의해야한다.
예를 들어, 이 블로그에서도 좀 개인적인 부끄러운 일들을 담은 markdown들을 unstage하고 싶을 때 저 방법을 써주면 된다. 이미 remote repo에 올라갔지만 commit 하고 push해주면 변경사항이 remote에도 반영된다.
