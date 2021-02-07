---
title: "🍵 Gradle 빌드 도구로 자바 프로젝트 만들기"
categories: java
tags: [ java, linux ]
toc: true
---

## 0. gradle을 사용하는 이유
프로그래밍 중에 생성되는 파일을 두는 폴더가 프로젝트 폴더이다. 다양한 종류의 파일을 한 폴더에 두면 관리하기가 불편하기 때문에 하위 폴더를 만들어 구분한다. 프로젝트의 하위 폴더를 만들 때는 국가, 회사, 개발자, 프로젝트에 상관 없이 일관된 방식으로 다루기 위해 **메이븐(Maven) 빌드 도구의 표준 디렉토리 구조**를 사용한다.

```
메이븐 프로젝트 폴더
└── src
    ├── main  <== 애플리케이션 관련 파일을 두는 폴더
    │   ├── java    <== 예) 패키지 폴더 및 자바 소스 파일(.java)
    │   └── resources  <== 예) 패키지 폴더 및 설정 파일(.xml, .properties 등)
    │   └── webapp  <== 웹 파일을 두는 폴더
    └── test  <== 단위 테스트 관련 파일을 두는 폴더
        ├── java    <== 패키지 폴더 및 단위 테스트 자바 소스 파일
        └── resources  <== 단위 테스트 관련 설정 파일(.xml, .properties 등)
```

Gradle이라는 도구를 사용하면 위와 같은 프로젝트 폴더를 쉽게 구성할 수 있다. Gradle은 프로젝트 폴더 구성에서 컴파일, 테스트, 배포 파일 생성 등 애플리케이션 생성에 필요한 전반적인 작업을 도와준다.


## 1. 자바 프로젝트 시작하기
`gradle init` 

```console
bitcamp-java-project git:(master) ✗ gradle init
Starting a Gradle Daemon (subsequent builds will be faster)

Select type of project to generate:  <== 프로젝트 유형 선택
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language: <== 프로그래밍에 사용할 언어 선택
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Swift
Enter selection (default: Java) [1..5] 3

Select build script DSL:
  1: Groovy <== 빌드 스크립트 파일을 작성할 때 사용할 언어 선택
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1

Select test framework:
  1: JUnit 4  <== 단위 테스트로 사용할 프레임워크 선택
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit 4) [1..4] 1

Project name (default: bitcamp-java-project): <== 프로젝트 이름 설정
Source package (default: bitcamp.java.project): com.eomcs.pms <== 자바 패키지 설정

> Task :init
Get more help with your project: https://docs.gradle.org/6.5.1/userguide/tutorial_java_projects.html

BUILD SUCCESSFUL in 53s
2 actionable tasks: 2 executed
```

Gradle로 생성한 프로젝트 설정 파일과 폴더를 확인하자.

```
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar <== (1)
│       └── gradle-wrapper.properties
├── gradlew <== (2)
├── gradlew.bat
├── settings.gradle <== (3)
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── eomcs
    │   │           └── pms
    │   │               └── App.java
    │   └── resources
    └── test
        ├── java
        │   └── com
        │       └── eomcs
        │           └── pms
        │               └── AppTest.java
        └── resources

```
- (2)`gradlew`: gradle로 빌드한 프로젝트를 누군가 서버에 올리고 동료가 클론했을 때, 동료는 굳이 따로 gradle을 설치하지 않아도 `gradlew` 파일이 (1) `gradle-wrapper.jar`를 풀어서 실행할 수 있게 한다.
- (3)`settings.gradle`: 프로젝트 이름은 `settings.gradle` 파일 안에 `rootProject.name = '프로젝트이름'`으로 적혀 있다.

## 2. 자바 애플리케이션 실행하기

`gradle run`

- gradle 도구가 클래스를 실행한다. 
- 실행하는 클래스를 바꿀 때마다 `build.gradle`에 적어줘야 한다.

```gradle
application {
    // Define the main class for the application.
    mainClassName = 'com.eomcs.pms.App'
}
```

- 그러면 귀찮지 않나?? => 실무에서 아무리 많은 파일을 만들어도 실행하는 클래스는 하나밖에 없음(main)!

## 3. 자바 프로젝트 빌드하기

- **일반 사용자**에게 **배포할 파일**을 만든다
- 우리가 만든 프로젝트를 제품으로 패킹하는 과정


`gradle build`

### 빌드 절차
- 소스 파일 컴파일
- 단위 테스트 수행
- .jar 아카이브 파일 생성
- 실행과 관련된 파일 생성
- .tar, .zip  배포 파일 생성

빌드하면 다음과 같이 `/build/distributions/`에 배포파일이 생긴다.

```
├── build
│   ├── distributions
│   │   ├── bitcamp-java-project.tar  <== Unix/Linux 사용자에게 배포하는 파일
│   │   └── bitcamp-java-project.zip  <== Windows 사용자에게 배포하는 파일
```

배포 파일의 압축을 플고 그 안의 스크립트 파일을 실행해보자.

```console
➜  distributions git:(master) ✗ tar -xvf *.tar  <== 압축파일 푸는 명령어 
➜  distributions git:(master) ✗ cd bitcamp-java-project 
➜  bitcamp-java-project git:(master) ✗ tree
.
├── bin
│   ├── bitcamp-java-project
│   └── bitcamp-java-project.bat
└── lib
    ├── bitcamp-java-project.jar
    ├── checker-qual-2.11.1.jar
    ├── error_prone_annotations-2.3.4.jar
    ├── failureaccess-1.0.1.jar
    ├── guava-29.0-jre.jar
    ├── j2objc-annotations-1.3.jar
    ├── jsr305-3.0.2.jar
    └── listenablefuture-9999.0-empty-to-avoid-conflict-with-guava.jar

2 directories, 10 files
➜  bitcamp-java-project git:(master) ✗ cd bin 
➜  bin git:(master) ✗ ./bitcamp-java-project  <== 스크립트 파일 실행
Hello world.
```
- 이때 Windows os의 경우 bat파일을 실행한다. 
- 특히 우리가 만든 app.class 가 lib안의 bitcamp-java-project.jar에 있다.
- 여기에 있는 app.class를 실행하는 명령어가 bin에 있는 거!!! => 이게 바로 **shell script**이다.
- 이 스크립트는 고객 컴퓨터가 어떤지 모르니까 모든 경우의 수를 다 따져봐서 실행할 수 있게 만들어졌다.
- 개발자라면 그냥 jvm의 java 명령어로 실행할 수 있겠지만 빌드는 일반 사용자를 위해 배포파일을 만드는 과정이다.
- `gradle clean`: 빌드한 걸 삭제하는 명령어