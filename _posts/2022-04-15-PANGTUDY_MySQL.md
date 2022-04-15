---
title: "[Pangtudy-web] Ubuntu 에 MySQL 설치"
excerpt: "ubuntu ec2 mysql install"
date: 2022-04-15
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

<span style="font-weight: bold; font-size: 1.2em">MySQL 설치 (8.x)</span>

```cmd
sudo apt-get update
sudo apt-get install mysql-server
```

<span style="font-weight: bold; font-size: 1.2em">포트 및 외부접속 활성화</span>

```cmd
sudo ufw allow mysql
sudo ufw allow out 3306/tcp
sudo ufw allow in 3306/tcp
sudo systemctl start mysql
sudo systemctl enable mysql
```

접속 에러가 날 때는

```cmd
sudo vim /etc/mysql/mysql.conf.d/mysqld/cnf
```

에서 `bind-address=0.0.0.0` 으로 바꾸거나 주석처리를 한다.

<span style="font-weight: bold; font-size: 1.2em">계정 생성 및 확인 쿼리</span>

mysql 에 관리자 권한으로 접속

```cmd
sudo mysql -u root -p
```

이후 계정 생성.

```SQL
# CREATE USER 'USER_ID'@'ACCESS_ADRESS' IDENTIFIED BY 'PASSWORD'
CREATE USER 'pangtudy'@'%' IDENTIFIED BY 'Qwerasdf!'
```

```sql
# 사용자 정보 확인
SELECT USER, HOST, AUTHENTICATION_STRING FROM MYSQL.USER;
```

참고로 기존에 생성된 계정의 비밀번호도 바꿀 수 있다.

```sql
# 비밀번호 변경
ALTER USER 'pangtudy'@'%' IDENTIFIED WITH MYSQL_NATIVE_PASSWORD BY '1234'
```

<span style="font-weight: bold; font-size: 1.2em">권한 부여</span>


끝으로 생성한 계정에 권한을 부여할 수 있다. 아래 명령어로 mysql 데이터베이스에 접속.

```cmd
use mysql
```

아래 명령어로 부여된 계정 권한을 확인할 수 있다.

```sql
# CREATE 권한 있는지 확인
SELECT USER, HOST, CREATE_PRIV FROM USER;

# DB.TABLE 권한 부여 (모든 권한) 부여
GRANT ALL PREVILEGES ON *.* 'pangtudy'@'%' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;
```



