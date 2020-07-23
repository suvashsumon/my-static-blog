---
title: "Cmder : একটি অসাধারণ টার্মিনাল ইমুলেটর"
date: 2020-07-23T11:08:13+06:00
draft: false
categories: ["Terminal Emulator"]
---

উইন্ডোজ অপারেটিং সিস্টেম ব্যবহার করেন কিন্তু টার্মিনালের খুব প্রয়োজন পড়ে? Powershell বা dufault cmd পছন্দ হয় না? Linux এর মত টার্মিনাল চান? তাহলে লেখাটি আপনার জন্য। Cmder হল টার্মিনালের/কমান্ড প্রম্পটের একটি ইমুলেশন যা উইন্ডোজ ও ইউনিক্স উভয় কমান্ড বুঝতে পারে। একই সাথে দেয় অসাধারণ সুন্দর ইউজার ইন্টারফেস। নিচের ছবিটি দেখুন –

![cmder](/images/july20/cmder.jpg)

তবে Cmder ইন্সটল করা কিছুটা জটিল।

## কিভাবে ইন্সটল করব?
https://cmder.net থেকে জিপ ফাইল ডাউনলোড করে নিন। মিনিমাম বা ফুল ভার্সনের যেকোনোটি ডাউনলোড করতে পারেন। অনেকেই ফুল ভার্সন নামাতে বলেন। আমি দুটোই ট্রাই করে দেখেছি। এখন মিনিমাম ভার্সন চালাচ্ছি কোন সমস্যা ছাড়াই। ডাউনলোড করা হলে আপনার C ড্রাইভে cmder নামে একটি ফোল্ডার তৈরি করুন। ফোল্ডারটির পাথ হবে এরকম –

>C:/cmder

আপনি চাইলে যেকোনো ড্রাইভে বা পাথে ফোল্ডারটি তৈরি করতে পারেন। তবে এই টিউটোরিয়ালের জন্য এভাবে করাটাই যুক্তিযুক্ত। পরবর্তীতে নিজের মত করে করতে পারবেন আশা করি।
ফোল্ডার তৈরি হয়ে গেলে জিপ ফাইলটি C:/cmder এ extract করুন। এবার C:/cmder ফোল্ডারটি দেখতে হবে নিচের মত –

![cmder folder view](/images/july20/cmder-folder-view.jpg)

Cmder.exe তে ডাবল ক্লিক করে নিচের মত উইন্ডো দেখতে পাবেন –

![cmder initial view](/images/july20/cmder-initial-view.jpg)

অভিনন্দন!! আপনি Cmder ইন্সটল করে ফেলেছেন। তবে আরও কিছুটা কাজ করলে cmder স্মার্টলি ব্যবহার করা সম্ভব।
খেয়াল করে দেখুন Cmder এর রুট ফোল্ডার C:/cmder

![cmder base folder view](/images/july20/cmder-base-folder-view.jpg)

যদি কোন ফোল্ডার এ যেতে হয় তবে কমান্ড দিয়ে যেতে হবে। যেমন D:/MyPicture/New ফোল্ডার এ যেতে লিখতে হবে cd D:/MyPicture/New . এই পদ্ধতিটি কিছুটা বিরক্তিকর। আমরা বরং কোন ফোল্ডার এর ভিতরে মাউসের রাইট ক্লিক করে Cmder ওপেন করার ব্যবস্থা করতে পারি। তার জন্য C:/cmder ফোল্ডারের ভেতরে cmder_context_enable.reg নামের ফাইল তৈরি করুন। ফাইলটি কোন টেক্সট এডিটরে ওপেন করে নিচের কোডটি কপি পেস্ট করুন এবং সেভ করুন।
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\cmder]
@="Open in Cmder"
"Icon"="C:\\cmder\\Cmder.exe,0"

[HKEY_CLASSES_ROOT\Directory\Background\shell\cmder\command]
@="\"C:\\cmder\\Cmder.exe\" \"%V\""

[HKEY_CLASSES_ROOT\Directory\shell\cmder]
@="Open in Cmder"
"Icon"="C:\\cmder\\Cmder.exe,0"

[HKEY_CLASSES_ROOT\Directory\shell\cmder\command]
@="\"C:\\cmder\\Cmder.exe\" \"%1\""
```
সেভ করা হয়ে গেলে cmder_context_enable.reg এ ডাবল ক্লিক করুন। কিছু ওয়ার্নিং দিতে পারে। সেগুলো accept বা ok করুন। এবার যেকোনো ফোল্ডারএ প্রবেশ করে মাউসের রাইট বাটন ক্লিক করুন। নিচের মত নিচের মত উইন্ডো দেখতে পাবেন –
open in cmder এ ক্লিক করে এই ফোল্ডার টিতে cmder ওপেন করতে পারবেন।

![cmder-open-in-cmder](/images/july20/cmder-open-in-cmder.jpg)

এবার লক্ষ্য করুন কারেন্ট ওয়ার্কিং ডিরেক্টরিতে ফোল্ডারটির নাম দেখাচ্ছে।

![cmder-open-in-cmder](/images/july20/cmder-open-in-folder.jpg)


## আনইন্সটল করবেন কিভাবে?

আনইন্সটল করার জন্য প্রথমে C:/cmder গিয়ে cmder_context_disable.reg নামের ফাইল তৈরি করুন। ফাইলটি কোন টেক্সট এডিটরে ওপেন করে নিচের কোডটি কপি পেস্ট করুন এবং সেভ করুন।
```
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\Directory\Background\shell\cmder]
[-HKEY_CLASSES_ROOT\Directory\shell\cmder]
```
এবার cmder_context_disable.reg এ ডাবল ক্লিক করুন। কিছু ওয়ার্নিং দিতে পারে। সেগুলো accept বা ok করুন। মাউসের রাইট বাটনে ক্লিক করে cmder ওপেন করার ফাংশনালিটি disable হয়ে যাবে। আপনার C ড্রাইভ থেকে cmder ফোল্ডারটি ডিলিট করে ফেলুন। ব্যস!! cmder আনইন্সটল করা সম্পন্ন ।

## বিঃ দ্রঃ

আপনার কাছে cmder_context_enable.reg এবং cmder_context_disable.reg ফাইল দুটি তৈরি করা কঠিন বা সমস্যা হলে এই লিঙ্ক থেকে ডাউনলোড করে নিন এবং C:/cmder ফোল্ডার এ পেস্ট করুন। ডাউনলোড লিঙ্ক : https://drive.google.com/drive/folders/1-KVFuICFaWQS7BMId8hq0hvSmRNLestu?usp=sharing