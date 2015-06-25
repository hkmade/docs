> OS X 맥에어 를 서버 환경으로 세팅하자. 

### Telnet 활성화
http://labs.hoffmanlabs.com/node/1690
매우 간단한 명렁으로 서비스 활성화 가능
Telnet 서버 활성화
sudo launchctl load -w /System/Library/LaunchDaemons/telnet.plist

Telnet 서버 비활성화
sudo launchctl unload -w /System/Library/LaunchDaemons/telnet.plist``` 


sudo -i 로 root 로그인
root계정용 .profile 생성
```
export PATH=/Users/miles/depot_tools:"$PATH"

export LANG=ko_KR.UTF-8
 
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced
export PS1="[\D{%Y-%m-%d %H:%M:%S}]-[\u@\h:\$PWD]\n\$ "
```

Homebrew설치


