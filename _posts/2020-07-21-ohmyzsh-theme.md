---
title: "oh-my-zsh 플러그인을 활용한 터미널 테마 바꾸기✨"
categories: linux
tags: [linux, shell]
toc: true
---

밋밋한 터미널을 컬러풀한 테마로 바꿔보자. 

#### zsh과 oh-my-zsh 설치
우선 z shell과 Oh My Zsh를 설치해준다. [여기](https://www.addictivetips.com/ubuntu-linux-tips/switch-from-bash-to-zsh-on-linux/)를 따라해보자.

#### .zshrc 파일로 테마 설정하기
설치가 끝났으면, 사용자 홈 경로(~)에서 .zshrc 파일을 열어준다. `nano {파일명}` 명령어를 치면 해당 파일을 편집할 수 있다.

```zsh
nano .zshrc
```

```zsh
ZSH_THEME="robbyrussell" # 여기에 원하는 테마 이름을 넣어주자!
```
현재 테마는 'robbyrussell'로 지정되어 있다. 여기에 원하는 테마 이름을 넣으면 설정이 된다. 테마 이름은 [여기](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)서 확인하자.

#### 테마 종류
대표적인 테마는 다음과 같다.

**agnoster**

![agnoster](https://cloud.githubusercontent.com/assets/2618447/6316862/70f58fb6-ba03-11e4-82c9-c083bf9a6574.png)

**wezm**

![wezm](https://cloud.githubusercontent.com/assets/1441704/6315419/915f6ca6-ba01-11e4-95b3-2c98114b5e5c.png)


**sunrise**

![sunrise](https://cloud.githubusercontent.com/assets/2618447/6316766/51fbf062-ba00-11e4-9b66-2b0da5a0dbbc.png)

여기서는 wezm 테마를 선택하겠다. 
```zsh
ZSH_THEME="wezm"
```
테마 이름을 넣고 ^o, Enter로 저장하고 ^x 로 나온다.

#### 결과

터미널을 끄고 다시 실행하면 다음과 같이 테마가 적용된 것을 확인할 수 있다.
![ohmyzsh-theme-wezm](https://user-images.githubusercontent.com/50407047/88047023-e7519e80-cb8b-11ea-88e0-4d024b70d828.png)