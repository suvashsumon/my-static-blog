---
title: "কমান্ড লাইনের মাদ্ধমে MySql ডাটাবেজ এক্সপোর্ট/ইম্পোর্ট করা"
date: 2021-07-17T11:39:02+06:00
draft: false
tags: []
categories: ["MySql", "Database", "Server"]
---

আমি এই লেখায় সার্ভারে অবস্থিত ডাটাবেজকে export/import করে দেখাবো। একই পদ্ধতি পিসির লোকাল apache/nginx সার্ভারেও অনুসরণ করা যাবে।

প্রথমেই ssh কী ব্যবহার করে সার্ভারে লগইন করুন।
```
ssh user_name@server_ip_address
```
# A. এক্সপোর্ট
### ধাপ-১ঃ নির্দিষ্ট পাথে প্রবেশ করুন
লগইন করার পর আপনি রুট ডিরেক্টরিতে থাকবেন। এক্সপোর্ট করা ফাইলটি যেখানে সেভ করতে চান সেই ফোল্ডারে যেতে নিচের কমান্ড রান করুন।
```
cd folder_path
```
আমি যেহেতু `/home` ফোল্ডারে এক্সপোর্ট করা ফাইলটি সেভ করব আমার কমান্ড হবে - 
```
cd /home
```
### ধাপ-২ঃ এক্সপোর্ট করা
ধরা যাক, আপনার `amar_db` নামে একটা ডাটাবেজ আছে এবং সেটিই এক্সপোর্ট করতে চান। তাহলে নিচের মত কমান্ড রান করুন। 

```
mysqldump -u username -p amar_db > exported_database.sql
```
এইখানে 
* `username` হল আপনার ইউজারনেম। এই ক্ষেত্রে রুট ইউজার হওয়া উচিত।
* `amar_db` হল ডাটাবেজের নাম।
* `exported_database.sql` হল আপনি যেই নামে এক্সপোর্ট করা ফাইলটি সেভ করতে চান। নামের শেষে অবশ্যই `.sql` থাকতে হবে।
কমান্ড চালানোর পর পাসওয়ার্ড চাইলে পাসওয়ার্ড দিন। এক্সপোর্ট করা ফাইলটি প্রেজেন্ট ওয়ার্কিং ডিরেক্টরিতে সেভ হয়ে যাবে। 
  
    
# B. ইম্পোর্ট
### ধাপ-১ঃ MySql লগইন
প্রথেমই একটা ডাটাবেজ তৈরি করতে হবে। সেজন্য MySql এ লগইন করা দরকার। নিচের কমান্ড রান করে লগইন করতে পারবেন।
```
mysql -u root -p
```
পাসওয়ার্ড দেওয়ার পর MySql শেল ওপেন হবে।

### ধাপ-২ঃ ডাটাবেজ তৈরি
ডাটাবেজ তৈরির জন্য MySql শেলে নিচের কমান্ড রান করুন।
```
CREATE DATABASE new_db;
```
এখানে `new_db` হল ডাটাবেজ নেম। আপনার পছন্দমত যেকোনো নামে ডাটাবেজ তৈরি করতে পারবেন।

### ধাপ-৩ঃ MySql লগ আউট
MySql শেলে আমাদের কাজ শেষ হয়েছে। কমান্ড দিয়ে লগ আউট করুন। 

### ধাপ-৪ঃ ফোল্ডারে প্রবেশ করা
প্রথমেই আপনাকে আপনার `sql` ফাইলটি যেই ফোল্ডারে আছে সেখানে প্রবেশ করতে হবে। সেজন্য নিচের মত কমান্ড রান করুন। 
```
cd folder_path
```

### ধাপ-৫ঃ ইম্পোর্ট করা
এইবার sql ফাইলটি new_database ডাটাবেজে ইম্পোর্ট করার জন্য নিচের কমান্ড চালান।
```
mysql -u username -p new_database < datafile.sql
```

* `username` হল ইউজার নেম।  
* `new_db` হল ডাটাবেজ নেম।  
* `datafile.sql` হল sql ফাইল যা ডাটাবেজে ইম্পোর্ট করতে হবে।  

এই ক্ষেত্রেও পাসওয়ার্ড চাইতে পারে। পাসওয়ার্ড দিয়ে ইন্টার চাপুন। ব্যাস! কাজ শেষ।