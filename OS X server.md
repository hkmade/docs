> OS X 맥미니를 서버 환경으로 세팅하자.

### Telnet 활성화
sudo -i 로 root 로그인
root계정용 .profile 생성
```
export PATH=/Users/miles/depot_tools:"$PATH"

export LANG=ko_KR.UTF-8
 
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced
export PS1="[\D{%Y-%m-%d %H:%M:%S}]-[\u@\h:\$PWD]\n\$ "
```

