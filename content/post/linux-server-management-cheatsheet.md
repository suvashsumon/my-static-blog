---
title: "Linux (Ubuntu) Server Management Cheatsheet"
date: 2021-08-14T20:51:01+06:00
draft: false
tags: []
categories: ["Linux Server", "Apache"]
---

I started this post for my personal use. However, if you find it useful, you can use it.This post will be edited continuously.

1. **Start/Stop/Restart** Apache Server:
```bash
sudo service apache2 restart/start/stop
```

2. To check apache **error log**: 
```bash
sudo grep -i invalid /var/log/apache2/error.log
```

3. **Start/Stop/Restart** Mysql Server:
```bash
sudo service mysql restart/start/stop
```

4. To check mysql **error log**:
```bash
sudo less /var/log/mysql/error.log
```

5. To check current status of apache/mysql:
```bash
sudo systemctl status mysql/apache2
```
