---
title: "🍵 이스케이프 문자(escape character)"
categories: java
tags: [ java ]
toc: true
---

### 정의

이스케이프 문자(escape character, 제어 문자)는 문자 제어 코드로, 화면에 출력하는 문자가 아니라 문자 출력을 제어하는 문자이다. `\`(역슬래쉬)와 한 개의 문자와 결합하여 작성한다.

### 종류

#### 1. Line Feed(LF):  `\n` 
줄바꿈 문자

```java
System.out.println("Hello,\nworld!");
```
```console
Hello,
world!
```

#### 2. Carrage Return(CR): `\r`
커서를 처음으로 돌리는 문자

```java
System.out.println("Hello,\rworld!");
```
```console
world!
```

#### 3. Backspace: `\b`
커서를 뒤로 한 칸 이동시키는 문자

```java
System.out.println("Hello,\b\b\bworld!");
```
```console
Helworld!
```

#### 4. Tab: `\t`
탭 공간을 추가시키는 문자

```java
System.out.println("Hello,\tworld!");
```
```console
Hello,  world!
```

#### 5. Form Feed: `\f`

```java
System.out.println("Hello,\fworld");
```
```console
Hello,
      world!
```

#### 6. Single Quote: `\'`
' 문자를 출력시키는 문자

```java
System.out.println('\'');
```
```console
'
```

#### 7. Double Quote: `\"`
" 문자를 출력시키는 문자

```java
System.out.println("Hello,\"w\"orld!");
```
```console
Hello,"w"orld!
```

#### 8. Backslash: `\\`
\ 문자를 출력시키는 문자
```java
System.out.println("c:\\Users\\user\\git");
```
```console
c:\Users\user\git
```

#### 주의

"" 안에서 '문자를 적거나 '' 안에서 " 문자를 적는 경우 이스케이프 문자 없이 그대로 적어주면 된다.

```java
System.out.println("Hello,'w'orld!");
System.out.println('"');
```
```console
Hello,'w'orld!
"
```

