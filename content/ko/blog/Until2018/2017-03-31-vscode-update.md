---
title: "vscode update 문제. coreCLR"
date: 2017-03-31

---

"Unable to attach to CoreCLR"
라는 에러가 떴다.

난 vscode + asp.NETCore로 작업하고 있었기 때문에 관련 에러를 검색했더니

'''

Workaround posted on [github](https://github.com/dotnet/coreclr/issues/10279) below. Confirmed works for me.

Here is a work around that worked for Gregg Miskelly:

Download https://dotnet.myget.org/F/dotnet-core/api/v2/package/runtime.osx.10.10-x64.Microsoft.NETCore.Runtime.CoreCLR/1.1.2-servicing-25123-01
Open the resulting file as a zip, and copy out runtimes/osx.10.10-x64/native/libdbgshim.dylib
Open a terminal
cd ~/.vscode/extensions/ms-vscode.csharp-1.8.0/.debugger
cp .
As I said above, but just to repeat here, I expect to have a real fix out soon. But hopefully this will unblock folks in the mean time.

'''
를 찾았고 이를 적용해서 풀었다.

기존에 ~/.vscode/extensions/ms-vscode.csharp-1.8.0/.debugger에 존재했던 libdbgshim.dylib 파일은 삭제 또는 libdbgshim-old.dylib로 바꾸고

새로운 libdbgshim.dylib를 mv 명령어로 mv file1 file2 (file1을 file2로 이동)을 이용해 옮겼다.

OSX 나 Asp.netcore상의 업데이트 문제로 갑작스럽게 문제가 발생한 것같다.
