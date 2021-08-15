---
title: "Creating New Database User and Grant Permissions"
date: 2021-08-14T23:40:38+06:00
draft: true
tags: []
categories: []
---
1. First log in to mysql:
```bash
mysql -u root -p
```
2. Create new user
```bash
CREATE USER 'ameenruhul'@'localhost' IDENTIFIED BY 'ac?Z6&7XvT#hyE6w';
```
3. Grant Privilages
```bash
GRANT * ON database_name.table_name TO 'username'@'localhost';
```
```
GRANT ALL PRIVILEGES ON blog.* TO 'ameenruhul'@'localhost';
```

4. Now last step
```bash
FLUSH PRIVILEGES;
```

