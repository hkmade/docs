> 기본문서는 Websquare Engine 개발 환경문서를 토대로 작성.

###개요
OS X 맥북의 개발환경을 설정하고 구축한다. 

###구축
####설치 준비물
* JDK 1.6이상
* Apache tomcat 7.0
* Eclipse 4.3 Kepler Java EE 32BIT --> 32비트 버전이 중요.
* Websqure 5 Studio 
* 2014.11. Websquaure Studio는 Eclipse 3.6 ~ 4.3버젼만을 지원한다.

#### JDK
OS X는 기본적으로 설치가 되어 있다. 
#### Apache Tomcat
적당한 위치에 압축해제. 디렉토리로 압축이 해제된다. TOMCAT의 홈경로가 됨.

#### Eclipse
원하는 위치에 압축해제. 터미널에서 아래와 같은 경로로 들어가 ini화일 수정
`$EClipse_HOME/eclipse.app/Contents/MacOS/eclipse.ini`
-xms -xmx 값을 적절히 조정. (512m - 1024m)
Eclipse 실행 후 지정해야하는 workspcae경로는  $EClipse_HOME 하위에 workspace사용. (최초 디렉토리가 없다면 생성할 것)

#### Eclipse Plugin 설치
##### SVN /GIT 설치
Eclipse Pulldown Menu - Help - Eclipse Marketplace..  선택  
모든 목록을 받아와서 화면이 뿌릴때까지 잠시 기다릴것(고장아님)  
검색창에서 SVN으로 입력후 Subversive-SVN Team Provider 3.0.0 설치 (2015.06버젼)  
설치후 이클립스 리스타트  
동일한 위치에서 EGit-Git Team Provider 3.71.설치 (2015.06버전)  

#### Websquare Feature 설치
Eclipse Menu - Help - Install New Software.. 선택  
준비한 WebSquare feature 파일을 아카이브 모드로 설치.  
이때 feature 화일의 zip화일은 풀지 말고 zip화일을 선택한다.  
Add Repository 창에서 Name : WebSquare Studio. Location은 feature zip화일을 선택한다.  
Group items by category 옵션을 해제해야 3개의 fautures를 체크할 수 있다.  
설치단계에서 License 동의에 체크  
이제 features 설치를 진행한다. 온라인 repositoy에서 다운로드후 설치과정을 진행.  
중간에 Security Warning은 websquare제품의 jar화일에서 발생. 무시하고 OK 버튼 누른후 진행  
설치 완료후 리스타트. WebSquare License를 입력  

### EClipse 환경 설정
#### Workspace 
Menu - Eclipse -> 환경설정  
General -> Workspace  
"Refresh using natvie hooks or polling" 항목 체크  
Text file encoding  
Other : UTF-8 로 설정  

#### Editor 
General -> Editors -> Text Editors
"Show line numbers" 체크

#### TOMCAT Server 
환경설정
General -> Server -> Runtime Environments
Add
압축해제한 톰켓 설치 경로 선택
설치완료

Eclipes의 메인 창의 하단 Servers Tab 선택
"No servers are available. CLick this link to create a new server.." 클릭
이전에 구성을 완료한 "Apache Tomcat v 7.0을 선택하고 "finish"

#### SVN 등록
MENU-Window-Open Perspective-Other...
SVN Repository 선택
리스트업하는 모든 connector를 선택.

## Websquare Engine 개발 환경 ##



JDK
