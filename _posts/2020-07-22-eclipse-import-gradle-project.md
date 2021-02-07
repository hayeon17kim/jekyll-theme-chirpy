---
title: "🍵 이클립스에 gradle로 만든 외부 프로젝트 import"
categories: java
tags: [ java ]
toc: true
---

### 이클립스에 gradle로 만든 외부 프로젝트 불러오기

기존에 존재하는 프로젝트를 불러오기 위해서는

file(화면 상단) > import > general > existing project into workspace로 들어가야 한다.

#### 문제 상황

그러나 이클립스가 아닌 다른 도구(이 경우에는 gradle)로 자바 프로젝트를 만들었을 경우 'No projects are found to import'라는 경고를 띄우며 이클립스가 인식을 하지 못한다.

![no-project-found](https://user-images.githubusercontent.com/50407047/88126527-a8aff880-cc0c-11ea-8ef1-677e2aecb7eb.png)

#### 필요 조건

이 이유는 프로젝트 폴더에 elipse 용 프로젝트 정보 파일이 없기 때문이다. 필요한 리스트는 다음과 같다.
- .project
  - 프로젝트 정보를 담은 파일
  - 이클립스는 이 파일의 정보를 가지고 프로젝트 및 메뉴를 설정한다.
- .settings/
  - Eclipse IDE의 플러그인 설정 파일이 들어 있는 폴더
- .classpth 파일
  - 자바 프로젝트인 경우 존재하는 파일이다.
  - 프로젝트에서 사용할 자바 라이브러리 파일에 경로 정보를 갖고 있다.


#### 방법

다행히도(?) 친절한 gradle이 이 리스트를 자동으로 만들어준다.

우리가 할 일은

eclipse 정보 파일이 없을 경우 gradle 도구를 이용하여 프로젝트 폴더에 이클립스가 사용할 설정 파일을 만든다.
- 1) `build.gradle` 파일의 plugin {} 에서 'eclipse' 플러그인을 추가한다.

```gradle
plugins {
    id 'java'
    id 'application'
    id 'eclipse'  // 이클립스 플러그인 추가
}
```

- 2) 터미널에서 `gradle eclipse` 실행한다.

```
(master)⚡ % gradle eclipse              ~/bitcamp-workspace/bitcamp-java-basic

BUILD SUCCESSFUL in 634ms
3 actionable tasks: 3 executed
```

그러면 gradle이 eclipse 설정 파일을 생성해줄 것이다.

**단!!!**

생성된 `.classpath`, `.project` 파일들은 git에 올라가면 안 되니, 원격 저장소에 올리기 전에 '.gitignore' 파일에 추가해준다.

#### 결과

이클립스로 들어가서 다시 한 번 확인해보자.

![eclipse-import-success](https://user-images.githubusercontent.com/50407047/88126525-a8176200-cc0c-11ea-8204-0b89a716ab78.png)

이제 이클립스가 이 프로젝트가 이클립스 프로젝트라는 것을 알아본다!

이클립스가 아니라 인텔리로 프로젝트를 불러올 때도 필요 파일을 생성해줘야 한다는 골자는 같다.