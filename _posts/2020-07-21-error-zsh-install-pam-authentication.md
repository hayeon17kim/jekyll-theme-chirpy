---
title: "🐛 zsh 설치 에러: PAM Authentication failures"
categories: error
tags: [ error, linux ]
---

#### 에러로그

bash를 zsh로 전환하면서 생긴 에러다.
[여기](https://www.addictivetips.com/ubuntu-linux-tips/switch-from-bash-to-zsh-on-linux/)대로 zsh를 다음의 명령어로 실행하려고 시도했으나..

```bash
~$ sudo -s
# chsh -s /bin/zsh root
# chsh -s /bin/zsh username
Password: 
chsh: PAM: Authentication failures
```
위와 같이 PAM 인증 실패가 뜬다.

#### 방법

쉘의 유효성을 체크하는 라이브러리 파일을 수정하여 에러를 해결할 수 있다.

1. `sudo nano /etc/pam.d/chsh`
2. `auth required pam_shells.so`를 주석처리 한다. (`#`사용)
3. `sudo chsh $USER -s $(which zsh)`
4. 로그아웃, 로그인 후 다시 시도한다.

```bash
~$ sudo -s
```
```bash
# chsh -s /bin/zsh root
# chsh -s /bin/zsh username
```
