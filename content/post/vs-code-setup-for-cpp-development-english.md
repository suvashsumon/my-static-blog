---
title: "Vs Code Setup for Running Program Directly"
date: 2020-08-24T20:05:24+06:00
draft: true
---

## Introducton
I usualy use Jetbreins product to development work like project. But it is not eficient to use those ide for small project or single file. Because those ide are very heavy and bit slow. For this I need to think for else. I can solve this problem using VS Code. As I need to install compailer/interpretor, a small setup can provide me this.

## Prequieste
* Basic knowledge of this language you want to use. Because this post is not about language syntex.
* VS Code editor.
* Compailer/interpreter for this language.
* Code Runner extension for VS Code.
* Internet connection. *(Just for installetion)*

## VS Code and Compailer/interpreter installation
At first install VS Code and compailer/interpreter. You can take help from Google. Make sure that path of compailer/interpreter have been set to system. Open your terminal and check version of this compailer/interpreter. If you able to see version then path setup done successfully.

## Code Runner Installetion
Code Runner is a VS Code extension which alow to run code sinppet or file written in various language.

> C, C++, Java, JavaScript, PHP, Python, Perl, Perl 6, Ruby, Go, Lua,
> Groovy, PowerShell, BAT/CMD, BASH/SH, F# Script, F# (.NET Core),
> C# Script, C# (.NET Core), VBScript, TypeScript,
CoffeeScript, Scala, Swift, Julia, Crystal,
> OCaml Script, R, AppleScript, Elixir, Visual Basic .NET,
> Clojure, Haxe, Objective-C, Rust, Racket, AutoHotkey,
> AutoIt, Kotlin, Dart, Free Pascal, Haskell,
> Nim, D, Lisp, Kit, and custom command.

You can download Code Runner from this [link](#) or by serching in VS Code extension tab.

After installatin, restart VS Code.

## Now Test This Setup
Open or create a file in VS Code and write some code for testing. Example, for C++,

```cpp
#includes<iostream>
using namespace std;

int main()
{
	cout << "I am running inside VS Code";
}
```

There are four way to run a code:
* Right click in mouse and select `Run Code`.
* Use shortcut `Ctrl+Alt+N`.
* Press F1 and select `Run Code`.
* Press traingle (run button) from toolbar.

After run a code you see output in output tab. But there is a problem. Output tab is by defult read only. You cannot give input in it. So a program with user input can not run successfully in output tab.

```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
	string name;
	cout << "Enter your name : ";
	cin >> name;
	cout << "Hello, Mr. " << name;
}
```

![Nothing](/images/august20/vs-code-setup-for-all-type-programming.jpg)

But we can solve this problem if we can run program in terminal tab. Oh, there is a tips! If you want to stop a program before terminate then use shortcut `Ctrl+Alt+M`.


## Run Program in Terminal Tab
For this go to `File > Preference > Setting > Extension`. Then click to `Run Code Configuration`. After scrooling you found a checkbox named `Run in Terminal`. Check this box.

Or you can do it by go to `setting.json` file and adding this code :

```
"code-runner.runInTerminal": true
```

From now program run in terminal tab. And you able to run program in VS Code directly.
