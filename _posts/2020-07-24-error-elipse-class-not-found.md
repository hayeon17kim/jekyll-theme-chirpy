---
title: "🐛 eclipse Java파일 실행 에러: class not found"
categories: error
tags: [ error ]
toc: true
---

### 에러로그

eclipse와 Visual Studio Code로 gradle을 통해 시작한 프로젝트를 동시에 볼 때, vsc 플러그인과 충돌해서 이클립스 상에서 파일이 실행이 되지 않을 수가 있다. 이 경우 콘솔 창에서는 클래스를 찾을 수 없다는 에러가 뜬다. 클래스 파일이 생성되어야 하는 bin 폴더 해당 패키지 디렉토리를 확인해보면 실제로 class 파일 자체가 생성이 안 되어 있다. 이는 설정 파일이 잘못된 문제로, 이러면 설정 파일을 깔끔하게 지워주고 다시 생성하면 된다.

### 방법

1. `gradle clean`: 빌드 폴더 삭제
2. `gradle cleanEclipse`: 모든 이클립스 관련 설정 삭제
3. 다시 `gradle eclipse`: 모든 이클립스 설정 파일을 생성하는 task 설정
4. 이클립스에서 해당 프로젝트로 가서 refresh하기
5. 실행하고 bin에 클래스 파일이 생성되었는지 확인하기
