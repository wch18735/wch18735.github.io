---
title: "[Pangtudy-web] WSL2 에 MySQL 설치"
excerpt: "mysql install in wsl2"
date: 2022-04-17
category:
    - pangtudy-web
tag:
    - web development
    - spring boot
    - jenkins
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# MySQL 설치 (feat. WSL2)

기존 OS에 MySQL 이 이미 설치되어 서비스가 돌아가고 있다면 3306 포트가 잡혀 에러가 날 것이다.

WSL2 로 기존에 설치한 Ubuntu-20.04 에 들어가준다. 그리고 mysql-server 를 설치한다.

<span style="font-weight: bold; font-size: 1.2em">ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)</span>

여기서 mysql 데몬을 실행하면 아래와 같은 에러가 발생할 수 있다.

```cmd
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)
```

아래 로그에서 에러 로그를 확인해보면 이미 3306 포트를 점유 중이라는 얘기가 있었다.

```cmd
cat /var/log/mysql/error.log
```

이럴 때에는 설정 파일 경로를 찾아 포트를 바꿔주면 된다.

```cmd
$ cd /etc/mysql/mysql.conf.d
$ sudo chmod 777 mysqld.cnf
$ vim mysqld.cnf

# ... PORT 바꿔 줌

$ sudo chmod 644 mysqld.cnf
```

이후 `sudo mysql -u root -p` 로 접속이 가능한지 확인해본다.

<span style="font-weight: bold; font-size: 1.2em">[Errcode: 2] Can't create/write to file '/var/run/mysqld/mysqld.pid'</span>

이 떄, 아래와 같은 에러가 발생할 수 있다.

```cmd
[Errcode: 2] Can't create/write to file '/var/run/mysqld/mysqld.pid'
```

해당 디렉토리 생성 후 소유권을 줌으로써 해결할 수 있다.

```cmd
$ cd /var/run
$ mkdir mysqld
$ chown mysql mysqld
$ chgrp mysql mysqld
```

<span style="font-weight: bold; font-size: 1.2em">error: su: warning: cannot change directory to /nonexistent: No such file or directory</span>

이렇게 했는데도.. 불구하고 아래와 같은 에러가 발생할 수 있다.

```cmd
error: su: warning: cannot change directory to /nonexistent: No such file or directory
```

이건 mysql 이 home directory 를 찾지 못해서 발생하는 이슈라고 한다. StackOverflow 에서 찾은 방법은 아래와 같다.

```cmd
sudo systemctl stop mysql.service
sudo usermod -d /var/lib/mysql/ mysql
sudo systemctl start mysql.service
```

또는

```cmd
sudo service mysql stop
sudo usermod -d /var/lib/mysql mysql
sudo service mysql start
```

나는 두 번째 방법으로 해결했다.
