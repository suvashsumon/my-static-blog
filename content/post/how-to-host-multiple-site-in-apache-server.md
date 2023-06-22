---
title: "একই অ্যাপাচি সার্ভারে (উবুন্টু) একাধিক সাইট হোস্ট করা"
date: 2021-07-05T19:47:43+06:00
draft: false
tags: []
categories: ["উবুন্টু", "উবুন্টু সার্ভার", "ভার্চুয়াল হোস্ট", "অ্যাপাচি সার্ভার"]
---
অ্যাপাচি সার্ভারে চাইলেই একই সাথে অনেকগুলো ওয়েবসাইট হোস্ট করা যায়। এতে খরচ যেমন কমে তেমনি একাধিক সার্ভার কনফিগারেশন করার ঝামেলায় যেতে হয় না। এই ফিচারটি ভার্চুয়াল হোস্ট নামে পরিচিত।

ভার্চুয়াল হোস্ট ব্যবহার করার জন্য আমাদের যা যা লাগবে - 
* উবুন্টু ভিপিএস
* অ্যাপাচি ওয়েব সার্ভার
* LAMP Stack ইন্সটল করা থাকতে হবে
* ssh ব্যবহার করে সার্ভারে লগইন থাকতে হবে

ধরা যাক, আমরা একই সাথে দুটি ওয়েবসাইট `example.com` এবং `example.org` হোস্ট করব। প্রথমেই আমাদের ডোমেইন প্রভাইডার সাইটে গিয়ে দুটি ডোমেইনেরই A-Record হিসেবে আমাদের ভিপিএস-টির আইপি এড্রেস সেট করে আসতে হবে। তারপর নিচের ধাপগুলো অনুসরণ করুন।

## ধাপ-১ঃ বেজ ফোল্ডার তৈরি
প্রথমেই দুটি সাইটের জন্য দুটি ফোল্ডার তৈরি করা যাক। ফোল্ডার দুটি অবশ্যই `/var/www` ফোল্ডারের ভেতর রাখতে হবে। নিচের মত করে দুটি ফোল্ডার তৈরি করতে পারেন।
```bash
sudo mkdir -p /var/www/example.com/public_html
```
```bash
sudo mkdir -p /var/www/example.org/public_html
```
## ধাপ-২ঃ ওনারশিপ পরিবর্তন
এখন ফোল্ডার দুটির ওনারশিপ শুধুমাত্র রুট ইউজারের। সেটা পরিবর্তন করে কারেন্ট ইউজারকে দিতে চাইলে নিচের কমান্ড রান করুন -
```bash
sudo chown -R $USER:$USER /var/www/example.com/public_html
```
```bash
sudo chown -R $USER:$USER /var/www/example.org/public_html
```

## ধাপ-৩ঃ পারমিশন চেঞ্জ করা
আপনার ওয়েবসাইট ঠিকমত সার্ভ করতে `/var/www` ফোল্ডারের পারমিশান চেঞ্জ করা দরকার। সেজন্য নিচের কমান্ড রান করুন - 
```bash
sudo chmod -R 755 /var/www
```

## ধাপ-৪ঃ স্যাম্পল ফাইল তৈরি
এই ধাপে আমরা ন্যানো এডিটর ব্যবহার করে সাইট দুটির জন্য `index.html` ফাইল বানাতে পারি। এইধাপটি বাধ্যতামূলক নয়। তবে সাইট ঠিকমত কাজ করছে কিনা যাচাই করার জন্য সহায়ক। `example.com` সাইটের জন্য নিচের কমান্ড রান করুন - 
```bash
sudo nano /var/www/example.com/public_html/index.html
```
এবার এডিটরে স্যাম্পল কোড লিখুন। যেমন - 
```html
<html>
<body>
This is our example.com website
</body>
</html>
```
একইভাবে অপর সাইটের জন্যও ফাইল বানাতে পারেন।
```bash
sudo nano /var/www/example.org/public_html/index.html
```
এবার এডিটরে স্যাম্পল কোড লিখুন। যেমন - 
```html
<html>
<body>
This is our example.org website
</body>
</html>
```


## ধাপ-৫ঃ কনফিগারেশন ফাইল তৈরি
অ্যাপাচি ইন্সটলের সময় আমাদের উবুন্টুতে ডিফল্ট কনফিগারেশন ফাইল তৈরি হয়ে যায়। যেটা `/etc/apache2/sites-available/000-default.conf` নামে থাকে। আমারা এবার আমাদের দুটি সাইটের জন্য `000-default.conf` কপি করে একই লোকেশনে দুটি আলাদা কনফিগারেশন ফাইল তৈরি করব। 
```bash
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf
```
```bash
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.org.conf
```
এইবার ফাইল দুটি ন্যানো এডিটরে ওপেন করে এডিট করতে হবে।  

`example.com` সাইটের জন্য নিচের কমান্ড রান করুন - 
```bash
nano /etc/apache2/sites-available/example.com.conf
```
ন্যানো এডিটর ওপেন হলে নিচের মত করে এডিট করুন - 
```bash
<VirtualHost *:80>
ServerAdmin webmaster@example.com
ServerName example.com
ServerAlias www.example.com
DocumentRoot /var/www/example.com/public_html
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
* এখানে `ServerAdmin` হল সার্ভারের এডমিন যা এডিট না করাই ভাল।
* `ServerName` হল ডোমাইন নেম।
* `ServerAlias` হিসেবে ডোমেইন নেমটি সহকারে দিতে হবে যাতে এবং উভয় ভাবেই সাইটটি অ্যাক্সেস করা যায়।
* `DocumentRoot` হিসেবে সাইটটির বেস ফোল্ডারের পাথ দিতে হবে। আমার ক্ষেত্রে সেটি হল `/var/www/example.com/public_html`

অনুরূপভাবে, `example.org` সাইটের জন্যও কনফিগারেশন ফাইল এডিট করুন।
```bash
nano /etc/apache2/sites-available/example.org.conf
```
```bash
<VirtualHost *:80>
ServerAdmin webmaster@example.org
ServerName example.org
ServerAlias www.example.org
DocumentRoot /var/www/example.org/public_html
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

## ধাপ-৬ঃ কনফিগারেশন ফাইল এনেবল করা
এই ধাপে আমরা কনফিগারেশন ফাইলগুলো এনেবল করব। তারজন্য প্রথমেই ডিফল্ট কনফিগারেশন ফাইল ডিজেবল করতে হবে। নিচের কমান্ড রান করুন - 
```bash
sudo a2dissite 000-default.conf
```bash

এবার নতুন তৈরি করা কনফিগারেশন ফাইল এনেবল করার পালা। নিচের কমান্ড দুটি রান করুন - 

```bash
sudo a2ensite example.com.conf
```
```bash
sudo a2ensite example.org.conf
```

## ধাপ-৭ঃ সার্ভার রিলোড করা
অ্যাপাচি সার্ভারটি রিলোড করার জন্য নিচ্রের কমান্ড রান করুন - 
```bash
sudo service apache2 restart
```

## ধাপ-৮ঃ টেস্ট করা
সাইটদুটির এড্রেস কোন ব্রাউজারে ব্রাউজ করে টেস্ট করুন সেগুলো ঠিকমত অ্যাক্সেস করা যায় কিনা। কোন এরর আসলে নিশ্চিত হয়ে নিন আপনার ডোমেইন প্রভাইডার ডোমেইন দুটি একটিভ করে দিয়েছে। তারপরও সমস্যা হলে উপরের ধাপ গুলো ঠিকমত অনুসরণ করেছেন কিনা যাচাই করুন।

