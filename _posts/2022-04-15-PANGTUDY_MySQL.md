---
title: "[Pangtudy Web] MySQL 설치 및 DB 생성, 외부 접속 계정 설정"
excerpt: "ubuntu ec2 mysql install"
date: 2022-04-15
category:
    - pangtudy web
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

가장 먼저 아래 세팅이 필요할 것이다. 계정 설정, 권한 설정이 모두 완료되어도 access denied 가 발생할 수 있다. 

접속 에러가 날 때는 아래 명령어로 `mysqld.cnf` 설정 파일을 연다.

```cmd
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

이곳에서 `bind-address=0.0.0.0` 으로 바꾸거나 주석처리를 한다.

<span style="font-weight: bold; font-size: 1.2em">계정 생성 및 확인 쿼리</span>

mysql 에 관리자 권한으로 접속

```cmd
sudo mysql -u root -p
```

이후 계정 생성을 해준다.

```sql
# CREATE USER 'USER_ID'@'ACCESS_ADRESS' IDENTIFIED BY 'PASSWORD'
CREATE USER 'pangtudy'@'%' IDENTIFIED BY 'Qwerasdf!'
```

생성된 사용자 정보는 아래 쿼리로 확인해 볼 수 있다.

```sql
# 사용자 정보 확인
SELECT USER, HOST, AUTHENTICATION_STRING FROM MYSQL.USER;
```

생성된 계정의 비밀번호도 바꿀 수 있다. 이를 꼭 수행해주는 것이 중요하다. `application.properties` 값에 저장된 비밀번호로 바꿔주어야 access denied 에러를 피할 수 있다.

```sql
# 비밀번호 변경 (8.x 기준)
ALTER USER 'pangtudy'@'%' IDENTIFIED WITH MYSQL_NATIVE_PASSWORD BY '1234';

# 변경 사항 적용
FLUSH PRIVILEGES;
```

적용된 사항을 확인하기 위해 아래 쿼리로 가장 최근에 변경된 내용이 현재 시간과 일치하는지 확인한다.

```sql
# password 가 최근 언제 변경되었는지 확인
# authentication_string 은 암호화되어 저장되었음
select host, user, plugin, authentication_string, password_last_changed from mysql.user;
```

<span style="font-weight: bold; font-size: 1.2em">권한 부여</span>

계정 생성 후에는 해당 계정에 접근 권한과 명령 권한을 부여해야 한다.

생성한 계정에 권한을 부여하는 방법을 살펴보자. 아래 명령어로 mysql 데이터베이스에 접속해 사용자 목록을 확인해본다.

```sql
# mysql 스키마를 사용함을 명시 (mysql.[] 으로 쓰기 가능)
use mysql;

# mysql 5.x 에서는 password 가능
select host, user, password from user;

# mysql 8.x 에서는 authentication_string 대체
select host, user, authentication_string from user;
```

위에서 생성한 사용자에게 아래 쿼리를 통해 권한을 부여할 수 있다.

```sql
# DB.TABLE 권한 부여 (모든 권한) 부여
GRANT ALL PRIVILEGES ON *.* TO 'pangtudy'@'%' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'pangtudy'@'%';

# 특정 권한만 부여 : select, insert, update, delete
GRANT select, insert, update, delete ON 'PANGTUDY'.* TO 'pangtudy'@'%';
```

아래 명령어로 부여된 계정 권한을 확인할 수 있다.

```sql
# CREATE 권한 있는지 확인
SELECT USER, HOST, CREATE_PRIV FROM USER;

# 또는
SHOW GRANTS FOR `pangtudy`@'%';
```

<span style="font-weight: bold; font-size: 1.2em">계정 삭제</span>

만약 특정 게정을 삭제하고 싶다면 아래 쿼리를 사용하자.

```sql
# 사용자 정보 삭제
drop user 'pangtudy';
drop user 'pangtudy'@'%';
```

<span style="font-weight: bold; font-size: 1.2em">데이터베이스 생성</span>

먼저 현재 존재하는 데이터베이스 스키마를 확인하려면 아래 명령어를 입력해본다.

```sql
# 현재 생성된 데이터베이스
show databases;
```

그 후 개발에 활용할 데이터베이스를 생성한다.

```sql
# 'PANGTUDY' 라는 이름으로 데이터베이스 생성
create database 'PANGTUDY';

# 'PANGTUDY' 라는 이름으로 default character set 은 utf8 로 생성
create database 'PANGTUDY' default character set utf8;
```

다시 한 번 `show databases;` 명령어를 통해 생성된 DB를 확인해보자. 

<span style="font-weight: bold; font-size: 1.2em">데이터베이스 삭제</span>

데이터베이스를 삭제하는 방법은 간단하다.

```sql
# drop 으로 database 삭제
drop database 'PANGTUDY';
```

<span style="font-weight: bold; font-size: 1.2em">MySQL 한글 깨짐 문제</span>

만약 `INSERT` 명령에 `java.sql.SQLException: Incorrect string value: ‘\xEC\x84\x9C\xEC\x9A\xB8…’ for column ‘address’ at row 1` 오류가 발생하면 근본적으로 데이터베이스 character set 을 변경해야 한다.

MySQL 에서 명시적으로 charset 을 설정하지 않으면 기본 charset 이 적용된다. 이를 해결하기 위해 아래 과정으로 config 파일에 인코딩 속성을 적용한다.

먼저 `whereis mysql` 명령어로 MySQL 의 config 위치를 찾는다. 보통 `/etc/mysql` 에 있는데, `cd /etc/mysql` 로 해당 디렉토리로 이동한다. 그리고 `sudo vim my.cnf` 를 통해 편집기를 열어 아래 내용을 맨 밑에 입력하고 저장한다.

```cmd
[mysql]
default-character-set = utf8

[client]
default-character-set = utf8

[mysqld]
character-set-server = utf8
collation-server = utf8_general_ci
init_connect=’SET NAMES utf8’
```

끝으로 `sudo service mysql restart` 명령으로 서비스를 재시작한다. 이후 MySQL에 접속해 `STATUS` 또는 `SHOW VARIABLE LIKE 'char%';` 를 입력했을 때, 아래와 같이 나타나면 적용이 완료된 것이다.

```cmd
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
Server characterset: utf8
Db characterset: utf8
Client characterset: utf8
Conn. characterset: utf8
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
```

기존에 생성된 테이블은 아래와 같은 방법으로 charset 을 변경할 수 있다.

```sql
alter table 'PANGTUDY' convert to character set utf8;
```

간혹 `utf8mb4` 가 초기 값으로 잡혀있는 경우가 있다. 결론부터 얘기하자면 `utf8mb4`는 emoji 값까지 입력 가능하다.

<span style="font-weight: bold; font-size: 1.2em">데이터베이스 및 테이블 인코딩 정보 확인</span>

먼저 데이터베이스 인코딩 정보 확인 쿼리를 살펴보자. 

```sql
# 데이터베이스 인코딩 확인
SELECT default_character_set_name, DEFAULT_COLLATION_NAME FROM information_schema.SCHEMATA 
WHERE schema_name = "PANGTUDY";
```

다음은 테이블의 기본 타입과 인코딩 정보를 살펴보는 쿼리다.

```sql
SELECT *
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA='PANGTUDY';

# 테이블 이름만 조회
SELECT TABLE_NAME
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA='PANGTUDY';
```

해당 테이블에 어떤 컬럼들이 있는지를 확인하려면 아래 쿼리를 사용해 조회해보자.

```sql
SELECT *
  FROM INFORMATION_SCHEMA.COLUMNS
 WHERE TABLE_SCHEMA='PANGTUDY'  
   AND TABLE_NAME='테이블명';
```

