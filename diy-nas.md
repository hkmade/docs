* page_title: DIY-NAS system guide
* page_description: NAS system install on Ubuntu OS of PC.
* page_keywords: nas, vmware, diy, ubuntu


#NAS System on Ubuntu OS

> **NAS(Netowrk Attached System)**는 용어에서 보듯 네트웍으로 연결된 스토리지를 말한다.  
우리에게는 웹하드라는 서비스로 익히 알려져 있는 시스템이다.  
NAS 솔루션으로 Opensouce기반의 대표적인 제품은 Pydio가 있다. 다시 말하자면 공짜솔루션이라는 말이다.  
(하지만 그만큼 설정과 운영은 스스로에게 책임과 권한이 부여된다)   
기존 상용 NAS제품에도 번들형태로 포함되어 있으며 일반적으로 Linux 서버를 구성한후 여기에 Pydio를 설정한다.   
단 대용량의 파일을 upload/download 하기위해서는 OS의 버젼과 내부 설정이 필요하며 가상환경에서 Linux 머신을 운용하는 경우 
가상환경에 대한 설정과 이해가 필요하다.
이 문서는 전통적인 VM가상머신에서 NAS시스템을 구축하는 절차를 담고 있는 가이드 문서이다. 

##Virutal Machine
개인 PC(or Notebook)에서 리눅스머신을 운용하기위한 환경은 가상머신이 편리하다. 특히 OS의 설치 후 변경 사항에 대한 저장과 복원이 매우 편리하기 때문에 Vmware 나 VirtualBox등의 가상머신의 환경이 필요하다. 하지만 이러한 가상머신환경에서 NAS System을 운영하는 경우 가상머신을 운영하는 hostPC의 디렉토리와 가상머신이  연결되어야 하는데 여기서 문제가 발생할 확률이 높다.   
가상머신에서 공유디렉토리 방식으로 PC의 디렉토리와의 접근이 원할하면 다행이지만 그렇지 않은 경우 가상머신의 OS를 재설치해야 할 수도 있다. 

##개인용NAS 시스템을 설치하기 위한 준비
**Host PC or Notebook**  
가상머신을 설치하여 운영할  시스템이다.  가능하다면 가상머신이 설치되는 파티션은 SSD로 운영하는 것이 
가상머신의 백업,복원등의 속도적인 이점을 누릴수 있다.  메모리는 최소한 4G 이상 8G를 추천한다. 

**Host PC가 사용하는 OS**  
Host system에는 Windows or Linux를 OS로 사용한다. 사용자의 편의성에 따라 선택하면 될 것이다. 
개인적으로 Windows 8.1도 충분히 Host PC로서 편리하게 사용하고 있다.     

**VM에 설치할 OS Image**  
Ubuntu OS 64bit를 이용할 것이다. OS는 반드시 64bit를 준비하도록한다. 
추후 웹하드에서 대용량 파일을 upload/download하기 위해 필요하다. 

**Vmware workstation**  
Vmware의 vplayer도 가능하지만 실시간 snapshot 과 restore를 위해서는 workstation 버젼을 준비하도록 한다. 

**NAS에서 서비스할 데이터파일 저장소**
일반적으로 Host PC의 Local HDD나 USB등으로 연결하는 외장형 HDD or 스토리지가 필요하다. 

##VMware Install

Host PC는 Windows 8.1이 설치되어 있다. Windows나 Linux나 크게 상관은 없지만 Vmware의 GUI나 운영의 친숙함을 볼때 Windows가 편리하다. 설치는 크게 이슈사항이 없으며 Vmware를 설치한 후에 가상머신의 이미지 생성은 가능하다면 SSD를 이용하는 것이 백업과 복원 VM의 기동과 종료에서 속도의 이점을 매우 크게 누릴수 있다. 

**Host PC - VMware workstation - VM - Guest OS**
이러한 스택으로 DIY NAS 시스템을 구성한다. 

###Virtual Machine 생성
Vmware workstation 안내에 따라 설치후 실행하고 VM생성을 진행한다. 

```
	New Virutal Machine
	Wizard
	  Custom 선택
	  Hardware compatibility (기본값)
	  Installer disc image files : 미리 download한 Ubunutu Server iso 선택
	  Personalize Linux
	    Full name  : NAS
	    Username, Password, Confirm 
	  Virtual machine name : 이름을 지정
	  CPU
	  MEMORY는 반드시 3G 이상
	  Network connection은 우선 기본값인 NAT 옵션 유지
	  Disk관련 HW옵션은 모두 기본값
	  Disk 생성은 Create a new virtual disk
	  Maximum disk size 는 기본값 20G
	    Allocatie all disk space now 선택
	  Disk file : virtual machine의 이름 지정.
```

###Guest OS Ubuntu 설치
Guest OS에는 Ubuntu Server를 선택했다.
`http://www.ubuntu.com/download/server`  
(2014.06현재 14.04 64bit only가 정식버젼)

32bit. 또는 64bit 어떤 ubuntu OS를 설치할 것인가? VM에 메모리 할당을 3G이상 가능 하다면  64bitOS로  설치하자.  
특히 PHP기반의 웹하드 App(Pydio)등을 사용하여  2G이상의  파일을 업로드/다운로드다운로드하기 위해서는 64bit 설치가 필요하다.
언어설정은 기본값인 US와 ENGLISH로 설치한다.  
그외는 모두 기본값으로 설정

###네트웍 확인
VM 설치 이후 Host PC가 인터넷에 연결되어 있다면 NAT모드로 설정되어 있으므로 VM도 DHCP로 아이피가 할당되어 인터넷이 연결이 가능.

###설치 완료후  콘솔에서의 작업
아직 외부에서의 접근은 불가능하며 Vmware를 통해 콘솔로 접근한다.  
NAT로 네트웍 구성이되어 있으며 Host PC가 인터넷으로 연결되어 있다면 Guest OS또한 인터넷으로 연결된다.  

###최초 SnapShot
VMware의 Snapshot기능을 이용해 최초 ISO설치 이미지본을 백업한다.  


##기본 OS Setting
###Timezone
date명령으로 PDT(태평양표준시)로 설정되는 경우 서울시간대로 변경 필요
```
root@ubuntu:/etc# date
Sun Mar 10 04:21:37 PDT 2013
root@ubuntu:/etc#
```
아래의 디렉토리로 이동  
```
root@ubuntu:/usr/share/zoneinfo# cd Asia
root@ubuntu:/usr/share/zoneinfo/Asia#
root@ubuntu:/usr/share/zoneinfo/Asia# ls -la Seoul
 -rw-r--r-- 1 root root 380 Sep  6  2012 Seoul
root@ubuntu:/usr/share/zoneinfo/Asia#   
```
기존의 timezone화일 삭제 후 심볼릭 링크를 새로 설정.(sf옵션은 기존 timezone 화일을 삭제하고 새로운 화일생성)
```
root@ubuntu:/usr/share/zoneinfo/Asia# ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
root@ubuntu:/usr/share/zoneinfo/Asia#
```
시간확인
```
root@ubuntu:/etc/init.d# date
Sun Mar 10 20:37:11 KST 2013
```

###한글 세팅
EUC-KR과 UTF-8을 추가 설치한다.
기본적으로 root권한으로 수행.
> root@ubuntu:/#locale

`locale-gen` 명령을 통해 한글문자셋을 설치
```
root@ubuntu:/etc/default# locale-gen ko_KR.EUC-KR
Generating locales...
  ko_KR.EUC-KR... up-to-date
Generation complete.
root@ubuntu:/etc/default# locale-gen ko_KR.UTF-8
Generating locales...
  ko_KR.UTF-8... up-to-date
Generation complete.
root@ubuntu:~# env | grep LANG
LANG=en_US.UTF-8
root@ubuntu:~#
root@ubuntu:~# more /etc/default/locale
LANG="en_US.UTF-8"
root@ubuntu:~#vi /etc/default/locale
 LANG="en_UR.UTF-8"
 LANGUAGE="en_US:"
 // 위 en_US부분을 ko_KR로 변경할것.
```
리부팅 후 `env`명령어를 통해 문자셋 확인
```
root@ubuntu:~# env | grep LANG
LANG=ko_KR.UTF-8
root@ubuntu:~#
```

##UTIL 설치
###SSH
>sudo apt-get install ssh  

기본적으로 시스템설치는 sudo명령을 통해 root 권한으로 설치할것. 
최초 OS설치시 설정한 사용자아이디와 패스워드가 root계정 패스워드로 사용된다.
인터넷으로 연결된 상태에서 우분투서버를 통해 필요한 패키지를 설치한다 

####기본포트변경
`sudo vi /etc/ssh/sshd_config`  
항상 강조하지만  왠만한 시스템 작업은 sudo를 통한 어드민권한으로의 접근으로 진행하자. 
`Port 22` 부분에서 포트번호를 수정한다.   
`/etc/init.d/ssh restart` 

###FTP설치
제일 무난한 ftp 프로그램인 vsftpd  
`sudo apt-get install vsftpd`   
`cd /etc ; vi vsftpd.conf`
anonymus_enable=NO  
locale_enable=YES  
두개 항목이 위와 같이 설정되어 있는지 확인할것.
또한 write_enable 주석 해제 후 프로세스 재기동
```
root@ubuntu:~# vi /etc/vsftpd.conf
root@ubuntu:/etc# service vsftpd restart
vsftpd stop/waiting
vsftpd start/running, process 1300
root@ubuntu:/etc# 
```

###네트웍 설정 변경
이제 외부에서 접근이 가능하도록 NAT가 아닌 고유아이피로 설정한다.
IP자원을 할당 받고 고정방식 IP로 설정한다.
(CASE에 따라 사설아이피의 포트포워드가 필요할 수도 있다.)
```
root@ubuntu:/etc/network#vi interfaces
	#The primary network interface
	auto eth0
	iface eth0 inet dhcp
```
아래의 포맷대로 IP를 수정한다
```
root@ubuntu:/etc/network#vi interfaces
	#The primary network interface
	auto eth0
	iface eth0 inet static
	address 10.241.58.179
	netmask 255.255.255.0
	gateway 10.241.58.4
	dns-nameserver 168.126.63.1 8.8.8.8
```

저장완료 후 VM설정에서 해당 OS VM의 Setting - Hardware - Network Adapter에서 Brideged모드로 변경 후 VM를 restart한다.
리부팅 후 ping과 기본 네트웍 확인을 통해 네트웍 연결상태를 점검한다.

###SSH 접근
이제 불편한 VMware 콘솔의 접근이 아닌 SSH를 통한 원격접근으로 시스템 작업을 진행하자.
SSH1,2를 지원하는 텔넷 클라이언트로 접속을 진행한다.
SSH설치시 별도의 포트로 설정했다면 접속시 포트번호를 지정하고 프로토콜은 SSH2로 설정한다. 
또한 텔넷/SSH 클라이언트에서 Character encoding은 UTF-8로 지정한다. 

###Hostname 변경
```
/etc/hostname
hostname -F /etc/hostname
```

이제 작업을 위한 최소한의 OS설정은 마무리
이 지점에서 본격적인 APM 및 필요한 오픈소스 설치를 하기 전 Snapshot을 진행한다.

##APM 설치
 
###Apache2 설치
```
hkmade@ubuntu:~$ sudo -i  
[sudo] password for hkmade:  
root@ubuntu:~# apt-get install apache2  
Reading package lists... Done  
	.
	.
```
공유기 설정에서 포트포워딩 추가.  
만약 인터넷 서비스의 제한으로 80포트를 사용할수 없는 경우 다른 포트로 웹서비스가 필요.  
다른 포트를  웹서비스 포트로 사용한다면 위의 ports.conf에서 조정한다.  

http://xxx.xxx.xxx.xx를 통해 홈페이지 확인 할 것. 
설치후 기본 htdocs(홈페이지root위치)는 /var/www/html로 지정되어 있다. (Ubuntu 14.04의 경우)
기존대로 /var/www로 변경하고자 한다면
/etc/apache2/sites-enabled/000-default.conf 화일에서 DocumentRoot에서 변경. 

###Mysql
mysql root 패스워드는 설치 중간 창에서 설정한다. 
```
	root@ubuntu:~# apt-get install mysql-server
	Do you want to continue [Y/n]? y
	Get:1 http://us.archive.ubuntu.com/ubuntu/ quantal/main libaio1 i386 0.3.109-2ubuntu1 [6,648 B]
	.
	.
	password 설정. (mysql root passwd)
	mysql start/running, process 6227
	Setting up libhtml-template-perl (2.91-1) ...
```

	netstat -tab명령으로 실제 서비스를 제공하고 있는지 확인  
```
	root@ubuntu:~# netstat -tap | grep mysql
	tcp        0      0 localhost:mysql         *:*                     LISTEN      6227/mysqld
```
mysql 설정화일은 my.cnf이며 포트와 로그 화일등을 설정할 수 있다. 
```
	root@ubuntu:~# vi /etc/mysql/my.cnf
```
###PHP5
```
	root@ubuntu:~# apt-get install php5-common php5 libapache2-mod-php5
	Reading package lists... Done
	..
	Do you want to continue [Y/n]? y
	Get:1 http://us.archive.ubuntu.com/ubuntu/ quantal-updates/main apache2-mpm-prefork i386 2.2.22-6ubuntu2.1 [2,360 B]
	...
	* Starting web server apache2   
	apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1 for ServerName
                                                                                                                        [ OK ]
```	
apache에서 php모듈이 활성화되었는지 다시 확인.
```
	root@ubuntu:~# sudo a2enmod php5
	Module php5 already enabled
	root@ubuntu:~#
```

php기반 웹스토리지 프로그램을 이용할때 upload, download size의 제약이 있다. 
php의 설정에 좌우되므로 수정한 후 apache2 프로세스를 재기동 할것.
우분투에서는 아래의 경로에 php설정화일이 존재한다. 
기본적으로 32bit OS레벨에서는 2GB이상의 size는 불가능하다.  
변경해야할 factor는 아래와 같다.  
```
	memory_limit
	post_max_size
	upload_max_filesize
	root:/etc/php5/apache2/php.ini
```

###pear 설치
pear (PHP Extension and Application Repository) install
```
	root@ubuntu:/var/www# apt-get install php-pear
	....
	Unpacking php-pear (from .../php-pear_5.4.6-1ubuntu1.1_all.deb) ...
	Setting up php-pear (5.4.6-1ubuntu1.1) ...
	root@ubuntu:/var/www#
```

###PHP설치 확인
기본 apache 서버의 htdocs인 /var/www 디렉토리 아래에 test.php화일 생성 후 아래 내용 기입  
`<?php phpinfo(); ?>`  
http://221.122.xx.xx/test.php  실행 후 정상적인 출력이 되는지 확인.

###PHPmyadmin 설치
```
	root@ubuntu:/var/www# apt-get install phpmyadmin
	...
	 * Reloading web server config                                                  
	Processing triggers for libc-bin ...
	root@ubuntu:/var/www#
```
중간 apache 자동모듈설정부분에서는 스페이스바로 apache를 선택하고 OK
Configuration phpmyadmin부분에서는 mysql를 설치하고 root비번을 설정한 상태라면.. no로 선택
**http://xxx.xxx.xxx.xx/phpmyadmin**   으로 접근시 로그인 화면이 나오면 OK.

2013.03현재 phpMyAdmin은 3.4.11.1 버젼.  appearance부분에서 한글은 지원하지 않는다.
한글 문자셋의 충돌을 막기위해서 우분투서버는 utf-8, phpMyadmin에서 보여지는 
General Settings - MySQL connection collation은  utf8_general_ci로 설정되어 있음을 확인한다.

##NAS App설치
###Ajaxplorer
웹하드 인터페이스를 구현한 오픈소스 프로젝트
홈페이지. http://pyd.io
2013.03현재 stable version은 4.2.3
2014.03현재 5.2.2
apt-get install 을 이용해서 편리하게 설치(는 안됨.)

site에서 download후 해당 웹서버에 업로드
/var/www에서 해당 화일의 압축을 풀고 http://주소/ajaxp 로 접근하기 위해 디렉토리이름을 mv명령어로 변경.
```
	root@ubuntu:/home/hkmade# ls
	ajaxplorer-core-4.2.3.tar.gz  Documents  examples.desktop  Pictures  Templates
	Desktop                       Downloads  Music             Public    Videos
	root@ubuntu:/home/hkmade# tar xvf ajax*.tar 
	root@ubuntu:/var/www# mv ajaxplorer-core-4.2.3 ajaxp
	root@ubuntu:/var/www# ls -la
	total 20
	drwxr-xr-x  3 root root 4096 Mar  7 20:31 .
	drwxr-xr-x 15 root root 4096 Mar  7 06:13 ..
	drwxr-xr-x  6 root root 4096 Mar  7 20:29 ajaxp
	-rw-r--r--  1 root root  177 Mar  7 06:13 index.html
```

###AjaXplorer 설정
`http://xxx/ajaxp 로 웹브라우져 접근
AjaXplorer Diagnostic Tool화면이 나옴.  
디렉토리 권한 설정할 차례  

    root@ubuntu:/var/www# chown -R www-data:www-data ajaxp
    root@ubuntu:/var/www# ls -la
    total 20
    drwxr-xr-x  3 root     root     4096 Mar  7 20:31 .
    drwxr-xr-x 15 root     root     4096 Mar  7 06:13 ..
    drwxr-xr-x  6 www-data www-data 4096 Mar  7 20:29 ajaxp
    -rw-r--r--  1 root     root      177 Mar  7 06:13 index.html
    -rw-r--r--  1 root     root       20 Mar  7 17:35 test.php
    root@ubuntu:/var/www#
	
		
apache config 에서 `/var/www/ajaxp/data` 영역을 `Override ALL`로 설정.
`/etc/apache2/apache2.conf` 에서 추가

    <Directory "/var/www/pydio/data">
        AllowOverride All
    </Directory>

Ajajxp5부터는 wizard방식으로 기본 설정 진행.
최초 언어는 한국어로 설정. 
Start wizard
Admin Access 계정설정
Global options  Default langae -한국어
Configurations storage 지점은 빠른속도를 위해서는 No Database

설치 이후 upload 파일 사이즈를 확인 한다.
Global Configurations - Application Core - Uploaders Options
Limitation의 File Size를 체크한다. php에 설정한대로 4G의 byte수가 할당됨.

대용량 업로드를 위해서는 java 기반의 uploader plugin을 활성화 한다.
이를 위해서 jar화일의 download와 pydio의 plugin 디렉토리로의 upload가 필요
```
download jar file
http://pyd.io/plugins/uploader/jumploader - 설치가이드

http://jumploader.com 에서 jar 화일 download
jumploader_z.jar 화일 선택후 download
pyd.io의 jumploader 폴더에 해당 jar화일 이동

	root:/var/www/pydio/plugins# cd *jumploader
	root:/var/www/pydio/plugins/uploader.jumploader# ls
	class.JumploaderProcessor.php  i18n  jumploader_tpl.html  manifest.xml  plugin_doc.html
	root:/var/www/pydio/plugins/uploader.jumploader# cp /tmp/*.jar .
```
pydio.io의 Gloabal Configuration - Feature plugins - Uploader에서
기존의 활성화되어 있는 Flash upload, HTML upload 모두 Enable에서 해제.
Jumploader를 활성화 하고 applet install 할것.
저장 후 재로그인.
파일 업로드시 기존의 html방식이 아닌 applet방식으로 upload창이 뜨는지 확인 할것.
단 이방식은 applet기반이기 때문에 uploader의 PC에 JRE이상이 설치되어 있어야 한다. 

##Host PC와 VM OS간의 파일공유
> NAS 시스템의 데이터는 VM환경으로 구성한 Ubunut OS에 직접 업로드하는 것이 아니고 Host PC에 연결되어 있는 (내부HDD 또는 USB등으로 연결된 외부 HDD) 폴더와 파일을 공유하는 개념이다. 
>따라서 HostPC의 특정 디렉토리와 파일을 VM환경의 Ubuntu OS가 인식할수 있도록 설정을 해주어야 한다. 

### VMware tool 인스톨.
VM이 반드시 기동된 상태에서 Vmware workstation - VM - install VMware Tools
만약 설치시 아래의 메세지가 나온다면 

    VMware Tools installation cannot be started manually while the easy install is in progress.
    http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1017687
    
위 내용은 핵심은 바로 Floppy dirvie를 auto detect로 설정하는 부분이다. 
`VM - Setting - Floppy - Use physical dirive : Auto detect`로 설정할 것.

로그인 하면 /media 부분에  VMware tools 가 마운트되어 있다. 

    root@miles-base-ubt02:/media/VMware Tools# ls -la
    합계 60599
    dr-xr-xr-x 2 miles miles     2048  8월 27  2013 .
    drwxr-xr-x 5 root  root      4096  3월  6 16:52 ..
    -r--r--r-- 1 miles miles 60654199  8월 27  2013 VMwareTools-9.6.0-1294478.tar.gz
    -r-xr-xr-x 1 miles miles     1961  8월 27  2013 manifest.txt
    -r--r--r-- 1 miles miles     1847  8월 27  2013 run_upgrader.sh
    -r-xr-xr-x 1 miles miles   689456  8월 27  2013 vmware-tools-upgrader-32
    -r-xr-xr-x 1 miles miles   698376  8월 27  2013 vmware-tools-upgrader-64
    root@miles-base-ubt02:/media/VMware Tools#

read only media 폴더에서  /tmp폴더로 해당 gz화일을 복사한 후 압축해제한다.
 
`root@miles-base-ubt02:/tmp/vmware-tools-distrib# ./vmware-install.pl`

매우 길게 설치작업이 진행되며.  선택값은 모두 엔터. (기본값으로 충분)
이 VMware tool을 설치해야 하는 이유는 host PC의 파일시스템의 공유기능을 이용하기 위해서이다. 

설치완료후 hgfs가 활성화 된 것을 확인할 것.

    root@miles-base-ubt02:~# cd /mnt
    root@miles-base-ubt02:/mnt# ls
    hgfs
    root@miles-base-ubt02:/mnt#

이제 마운트 포인트를 만들고 /etc/fstab을 통해 부팅후 자동으로 마운트할수 있도록 설정 추가. 마친 후 시스템 리부팅
VM 메뉴에서 `Options - SHared Folders`에서 등록할것.

    root@ubuntu:/mnt/hgfs# mkdir /nas
    root@ubuntu:/mnt/hgfs# vi /etc/fstab
    	.host:/TMS	/nas	vmhgfs defaults,ttl=5,uid=1000,gid=1000 0 0  // 라인을 추가한다.


    root@ubuntu:/mnt/hgfs# reboot
    ....
    root@ubuntu:~# df -k
    	Filesystem      1K-blocks      Used  Available Use% Mounted on
    	/dev/sda1        19609276   3800064   14813116  21% /	
    	udev               505560         4     505556   1% /dev
    	tmpfs              205324       804     204520   1% /run
    	none                 5120         0       5120   0% /run/lock
    	none               513304       152     513152   1% /run/shm
    	none               102400        36     102364   1% /run/user
    	.host:/NAS     1953381372 828871844 1124509528  43% /nas
    	root@ubuntu:~#
	
	
##AFP설치 on VM
> OS X or iOS기기에서 NAS 시스템을 접근하기 위한 프로포콜이다. Apple Filing Protocol 

MAC OS에서 NAS의 디렉토리 폴더를 원격으로 마운트하여 사용 가능.
응용범위. 타임머신 백업, iPhoto라이브러리 공유. iTunes뮤직서버. 
2013.03현재 AFP를 지원하는 Netatalk 프로토콜은 3.0.2.

    root@ubuntu:/tmp# wget http://sourceforge.net/projects/netatalk/files/netatalk/3.0.2/netatalk-3.0.2.tar.gz
    root@ubuntu:/tmp# gunzip *.gz
    root@ubuntu:/tmp# tar xvf neta*.tar
    root@ubuntu:/tmp/netatalk-3.0.2#./configure -enable-debian
    ....
    root@ubuntu:/tmp/netatalk-3.0.2# make
    root@ubuntu:/tmp/netatalk-3.0.2# make install
    

    서비스기동 및 확인
    

    root@ubuntu:/tmp/netatalk-3.0.2# cd /etc/init.d
    root@ubuntu:/etc/init.d# netatalk restart
    root@ubuntu:/etc/init.d# /usr/local/sbin/afpd -V
    afpd 3.0.2 - Apple Filing Protocol (AFP) daemon of Netatalk
    root@storycube:/usr/local/sbin# ./netatalk restart
    netatalk is already running (pid = 27310), or the lock file is stale.
    root@storycube:/usr/local/sbin#
    해당 pid kill
    root@storycube:/usr/local/sbin# ./netatalk restart
    netatalk is already running (pid = 27310), or the lock file is stale.

AFP의 포트포워딩 설정
포트번호는 548

## JDK on Host PC
windows 8 
www.java.com 에서 desktop OS에 맞게 자동 다운로드
JAVA SE Runtime Environment 7 update 17   .2013.04.09

##Airvideo on Host PC
windows 8
컴퓨터의 영상을 무선랜(와이파이, 와이브로등등)을 지원하는 개인용 모바일 기기에서 볼 수 있게 해주는 프로그램. 
컴퓨터의 영상을 스마트 기기에 직접 복사해서 넣지 않고도 동영상을 볼수 있다. 
지원하는 단말기기는 iPhone, iPad, iPod touch

    http://www.inmethod.com/air-video/index.html

포트포워드 
45631포트를 포트포워드 규칙에 추가할것.
외부에서  http://공유기공인아이피:45631  체크.
웹브라우저에서 Air Video화면이 뜨면 성공.

##Qloud Server 설치
다운로드 사이트

    http://j.mp/qloud-server
    http://app.qiss.mobi  
    http://54.248.222.241/wordpress/?cat=10
