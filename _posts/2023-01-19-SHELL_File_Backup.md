---
title: "[Shell] 파일 백업을 위한 Shellscript"
excerpt: "existence check for file backup"
date: 2023-01-19
layout: single
classes: wide
category:
    - shellscript
tag:
    - shell script
    - file backup
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
comments: true
---

# 파일 백업을 위한 shellscript

파일이 있으면 해당 파일을 백업 폴더로 옮기고, 아닌 경우 단순 저장하는 쉘 스크립트를 짰다. 가령 Ansible에서 파일 백업을 위해 원격 서버 sh backup.sh을 실행하기 위해 아래와 같은 스크립트를 작성해 놓을 수 있다.

```sh
VERSION=1.0.0
DATE=`date +%Y%m%d%H%M%S`

TARGET_PATH=~/system-name/target
BACKUP_PATH=~/system-name/backup
WAR_FILE=PRODUCTION-"$VERSION"-RELEASE.war
WAR_BACKUP_FILE=PRODUCTION-"$VERSION"-"$DATE".war

if [ -f "$TARGET_PATH"/"$WAR_FILE" ]; then
        echo "File exist"
        ls -l
        mv "$TARGET_PATH"/"$WAR_FILE" "$BACKUP_PATH"/"$WAR_BACKUP_FILE"
        echo "$TARGET_PATH"/"$WAR_FILE" moved to "$BACKUP_PATH"/"$WAR_BACKUP_FILE"
else
        echo "File doesn't exist"
fi
```

테스트를 위해 아래와 같이 테스트 스크립트를 작성해 볼 수 있다.

```sh
DATE=`date +%Y%m%d%H%M%S`
TEST_FILE=~/system-name/target/tmp.txt
if [ -f "$TEST_FILE" ]; then
        ls -l
        mv tmp.txt ~/system-name/backup/tmp-"$DATE".txt
else
        echo "Test File doesn't exist"
fi
```

그리고 내부에 `tmp.txt`를 만든 다음 실행해보면, 아래와 같은 결과를 볼 수 있다.

```sh
[account@server target]$ ls ~/system-name/backup/
tmp-20230119194123.txt
```