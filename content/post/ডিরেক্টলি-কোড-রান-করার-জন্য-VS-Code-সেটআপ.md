---
title: "ডিরেক্টলি কোড রান করার জন্য VS Code সেটআপ"
date: 2020-08-24T13:24:21+06:00
draft: false
tags: ["IDE", "VS Code Setup", "Code Editor"]
categories: ["VS Code", "VS Code সেটআপ", "Code Editor"]
---

## ভূমিকা
আমি জেটব্রিন্স এর প্রোডাক্ট ব্যবহার করে অভ্যস্ত। প্রোজেক্ট টাইপ কাজের জন্য আমি সেগুলোই ব্যবহার করি। তবে ছোট প্রজেক্ট বা সিংগেল ফাইল নিয়ে কাজ করার জন্য জেটব্রিন্স এর আইডিই ব্যবহার করা আসলে মশা মারতে কামান দাগার মত ব্যাপার। কেননা, এই আইডিইগুলো বেশ ভারী আর কিছুটা স্লো। সে ক্ষেত্রে আসলে কোন বিকল্প ভাবতেই হয়। আমরা এই সমস্যা সমাধান করতে পারি VS Code এর মাধ্যমে। যেহেতু কম্পাইলার বা ইন্টারপ্রেটার ইন্সটল করতেই হয়, তাই সামান্য কিছু সেটআপ দিয়ে এই সুবিধাটুকু নিতে পারি।

## যা যা লাগবে
* যে ল্যাঙ্গুয়েজ ব্যবহার করতে চান সেটি সম্পর্কে মৌলিক দক্ষতা থাকতে হবে। কেননা আমি এই আর্টিকেলে কোন ল্যাঙ্গুয়েজের সিনট্যাক্স শেখাবো না।
* VS Code এডিটর।
* প্রয়োজনীয় কম্পাইলার/ইন্টারপ্রেটার।
* Code Runner এক্সটেন্সন।
* এবং অবশ্যই ইন্টারনেট কানেকশন। *(শুধুমাত্র ইন্সটলেশনের জন্য)*

## VS Code ও  কম্পাইলার/ইন্টারপ্রেটার ইন্সটলেশন
গুগল করে VS Code আর প্রয়োজনীয় কম্পাইলার/ইন্টারপ্রেটার  ইন্সটল করে ফেলুন। অবশ্যই কম্পাইলার/ইন্টারপ্রেটারের পাথটি সিস্টেমে সেটআপ করে নিন। আর টার্মিনাল বা cmd ওপেন করে কম্পাইলার/ইন্টারপ্রেটারের ভার্শন চেক করতে ভুলবেন না। ভার্সন দেখালে জানবেন পাথটি সিস্টেমে ঠিকঠাক সেটআপ হয়েছে।

## Code Runner ইন্সটলেশন
Code Runner হল VS Code এর একটি এক্সটেনশন যা বেশ কটি ল্যাঙ্গুয়েজে লিখা কোড স্নিপেট বা ফাইল সরাসরি রান করার সুবিধা দেয়। ল্যাঙ্গুয়েজ গুলো হলঃ

> C, C++, Java, JavaScript, PHP, Python, Perl, Perl 6, Ruby, Go, Lua,
> Groovy, PowerShell, BAT/CMD, BASH/SH, F# Script, F# (.NET
> Core), C# Script, C# (.NET Core), VBScript, TypeScript, CoffeeScript,
> Scala, Swift, Julia, Crystal, OCaml Script, R, AppleScript, Elixir,
> Visual Basic .NET, Clojure, Haxe, Objective-C, Rust, Racket,
> AutoHotkey, AutoIt, Kotlin, Dart, Free Pascal, Haskell, Nim, D, Lisp,
> Kit, and custom command.

Code Runner সরাসরি ডাউনলোড করতে পারবেন [লিঙ্ক](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner) থেকে বা VS Code এর এক্সটেন্সান ট্যাবে গিয়ে সার্চ করে।

![ডিরেক্টলি কোড রান করার জন্য VS Code সেটআপ](/images/august20/code-runner.jpg)

এরপর ইন্সটল করে VS Code রিস্টার্ট করুন।

## এবার টেস্ট করার পালা
VS Code ওপেন করে একটি ফাইল খুলুন/তৈরি করুন। টেস্টিং এর জন্য কিছু কোড লিখুন। যেমন C++ এর জন্য -

```cpp
#includes<iostream>
using namespace std;

int main()
{
	cout << "I am running inside VS Code";
}
```
কোড রান করার জন্য চারটি উপায় আছেঃ
* মাউসের রাইট বাটন ক্লিক করার পর Run Code এ ক্লিক করে।
* শর্টকাট ব্যবহার করে -` Ctrl+Alt+N`
* F1 ক্লিক করে Run Code সিলেক্ট করে।
* টুল বারের ত্রিভুজে (রান বাটন) ক্লিক করে।

রান করলে VS Code এর output ট্যাবে প্রোগ্রামের আউটপুট দেখা যাবে। কিন্তু সমস্যার কথা হল VS Code এর output ট্যাব বাই ডিফল্ট read only । তাই যদি আপনি সেখানে ইনপুট দিতে যান তবে দিতে পারবেন না। অর্থাৎ কোন প্রোগ্রাম যদি ইউজার ইনপুট ব্যবহার করে, সেটি output ট্যাবে ঠিকমতো রান করবে না।

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

![ডিরেক্টলি কোড রান করার জন্য VS Code সেটআপ](/images/august20/vs-code-setup-for-all-type-programming.jpg)

তবে যদি আমরা প্রোগ্রাম টার্মিনাল ট্যাবে রান করতে পারি তবে এই সমস্যা সমাধান করতে পারব। ও হ্যাঁ, একটা টিপস দিয়ে যাই।  চলমান প্রোগ্রাম নিজে নিজে টারমিনেট হওয়ার আগেই টারমিনেট করতে চাইলে শর্টকাট ` Ctrl+Alt+M` ব্যবহার করুন অথবা output ট্যাবে মাউসের রাইট বাটন প্রেস করে Stop Running Code সিলেক্ট করুন।

## প্রোগ্রাম টার্মিনাল ট্যাবে রান করা
এ জন্য File > Preference > Setting > Extension এ যান। তারপর সেখান থেকে Run Code Configuration খুঁজে বের করে ক্লিক করুন। এবার স্ক্রল করলে Run in Terminal নামের একটা চেকবক্স পাবেন। সেটি চেক করে দিন।

অথবা VS Code এর `setting.json` ফাইলে গিয়ে নিচের কোডটুকু পেস্ট করে সেভ করুন।

```
"code-runner.runInTerminal": true
```

এখন থেকে কোড রান করলে সেটি Output ট্যাবের বদলে Terminal ট্যাবে রান হবে।
