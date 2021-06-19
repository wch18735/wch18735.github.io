---
title: "[Python] Python file download from url"
excerpt: "urllib을 이용한 파일 다운로드"
date: 2021-06-20
layout: single
classes: wide
category:
    - python
tag:
    - urllib
    - file download
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Python을 이용한 파일 다운로드

강의가 비대면으로 바뀐 뒤 좋은 점은 온라인 강의를 여러 번 들을 수 있다는 점입니다. 그러나 학기가 지나고나면 강의들은 들을 수 없게 되는데, 이를 위해 강의를 다운로드 해놓는 분들이 계실 것 같습니다. 14주차에 각 2개의 강의가 합쳐지면... 많이 귀찮은 일일텐데요. 저는 이를 코드로 해결해봤습니다.

```python
import requests

if __name__=="__main__":
    with open("./lecture.txt", "r", encoding="utf-8") as f:
        lines = [line.strip() for line in f.readlines()]

        file_order = 1
        week = ""
        for line in lines:
            # len(https) == 5
            if len(line) < 5:
                week = line
                file_order = 1
                continue

            # process print
            print(f"{week}({file_order}).mp4 download start!")

            # get request from url
            r = requests.get(line)
            with open("C:/Users/wch18/Downloads/" + f"{week}({file_order}).mp4", "wb") as video:
                video.write(r.content)

            # print processed
            print(f"{week}({file_order}).mp4 finish!")

            # plus file order
            file_order += 1
```

위의 코드는 URL 정보가 적힌 txt 파일로부터 하나씩 컨텐츠를 선택하여 주차별로 이름을 설정해 원하는 경로에 다운받는 짧은 코드입니다.

```python
response = request.get("URL")

with open("file path", "wb") as file_pointer:
    file_pointer.write(response.content)
```

위의 코드가 핵심입니다. **URL**이 주어지면 `request.get()`을 통해 이를 GET 해옵니다. 그리고 해당 응답의 **content**를 원하는 경로 `file path`에 쓰게 되는 것입니다. 주의할 점으로는 `file path`의 끝에는 항상 `.mp4`와 같은 파일 형식 지정자가 붙어야 한다는 것입니다.

![download result](/_img/2021-06-20/download_result.jpg){: .align-center}

결과는 위와 같은 형식으로 나타나게 됩니다! 나중에는 GUI 형태로 URL을 입력받아 다운로드를 진행하는 프로그램을 짜보는 것도 재미있을 것 같습니다.