---
title: "Install XAMPP, Composer on Ubuntu 20.04 LTS or Newer"
date: 2021-07-28T22:28:51+06:00
draft: false
tags: []
categories: ["XAMPP Server", "PHP", "Composer", "Laravel"]
---

XAMPP is a complete package of Apache, MariaDB(MySql), PHP and Perl. XAMPP is completely free software and widely used on Windows platforms for web development and database management. Its easy to install XAMPP on a Windows operating system but complicated to install on a Linux machine. In this article, I am trying to show how to install XAMPP on Ubuntu 20.0.4LTS+.

  
  
## Download XAMPP
Download the latest version of XAMPP from their official website https://www.apachefriends.org. Your file may be stored in the Downloads folder.

  
## Installation and Run
1. Navigate to the containing folder using `cd` command.
2. Change permission of this file.
    ```
    sudo chmod 755 xampp-linux-*-installer.run
    ```
    Here `xampp-linux-*-installer.run` is your downloaded file name.
3. Install this file using -
   ```
   sudo ./xampp-linux-*-installer.run
   ```  

Now you can run XAMPP on your Linux pc.

To start XAMPP run the command
```
sudo /opt/lampp/lampp start
```
To stop
```
sudo /opt/lampp/lampp stop
```
To restart
```
sudo /opt/lampp/lampp restart
```

  
## Its Time to Set the Environment Variable
To install composer, laravel, or others PHP technologies you need to set PHP path as an environment variable. To do dis follow the following steps.
1. Open `/etc/environment` using an editor. For example using nano - 
   ```
   sudo nano /etc/environment
   ```
2. At the end of file add following code -
   ```
   :/opt/lampp/bin/php
   ```
   Then save and exit.

3. Now we need to build a link. Run this command - 
   ```
   sudo ln -s /opt/lampp/bin/php /usr/local/bin/php
   ```

  
## Installing Composer
To install composer globally you need to download composer using `curl`. If `curl` is not installed on your pc, at first install it. Then run the command - 
```
curl -sS https://getcomposer.org/installer | php
```

Go to download folder and move composer to `/usr/local/bin/composer` folder using following command - 
```
sudo mv composer.phar /usr/local/bin/composer
```

To install **Laravel** follow the official documentation. Its need to run only one or two commands.

