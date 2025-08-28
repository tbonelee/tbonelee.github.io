---
layout: post
title: "Born2beroot(발번역)"
date: "2021-05-21 23:08:21 +0900"
categories:
  - 42-Born2beroot
---
### Ch II.


#### Introduction(도입부)



 이번 프로젝트는 가상화를 소개하기 위한 프로젝트이다.(This
 project aims to introduce you to the wonderful world of
 virtualization.)  
  
너는 특정한 지침 하에서 첫번째
 머신을 ***VirtualBox***에 만들어야 한다. 프로젝트를 마치고 나면 엄격한 규칙을
 구현하면서 너만의 운영체제를 설정할 수 있을 것이다.(You will
 create your first machine in VirtualBox under specific
 instructions. Then, at the end of this project, you will be
 able to set up your own operating system while implementing
 strict rules.)
 




---


### Ch III.


#### General guidelines(일반 지침)


- **VirtualBox**의 사용은 필수이다.(The use of VirtualBox
 is mandatory)
- 리포지토리의 루트에 **signature.txt**만 제출하면 된다.
 그 안에 너가 만든 머신의 버츄얼 디스크 시그내처를 붙여넣기
 해야 한다. 더 많은 정보는 ch VI으로 ㄱㄱ.(You only have to
 turn in a signature.txt file at the root of your
 repository. You must paste in it the signature of your
 machine’s virtual disk. Go to Submission and
 peer\-evaluation for more information.)




---


### Ch IV.


#### Mandatory part(필수 파트)



 이 프로젝트는 다음의 특정 규칙들에 따라 너의 첫 번째 서버를
 설정하는 것으로 이루어져있다.(This project consists of
 having you set up your first server by following specific
 rules.)
 




| (!!) 서버를 설정하는 문제이므로 너는 최소한의 서비스만  설치할 것이다. 따라서 그래픽 인터페이스는 여기서  사용되지 않을 것이다. 그렇기 때문에 X.org나 다른  동등한 수준의 그래픽 서버를 설치하는 것은 금지된다.  이걸 어기면 너의 점수(?note)는 0이다.(Since it is a  matter of setting up a server, you will install the  minimum of services. For this reason, a graphical  interface is of no use here. It is therefore forbidden  to install X.org or any other equivalent graphics  server. Otherwise, your note will be 0\.) |
| --- |



 운영체제는 **Debian**의 최신 안정화 버전(테스트 버전 또는
 비안정화 버전은 ㄴㄴ)이나, **CentOS**의 최신 안정화
 버전을 골라야 한다. 시스템 관리를 처음 해보는 거라면
 **Debian**을 강력 추천합니다.(You must choose as an
 operating system either Debian latest stable (no
 testing/unstable), or CentOS latest stable. Debian is highly
 recommended if you are new to system administration.)
 




| ⓘ CentOS를 설정하는 것은 꽤나 복잡하다. 그러니까  KDump를 설정할 필요는 없다. 그렇지만 SELinux는 시작  순간에 실행되고 있어야 하고 그것의 환경 설정은  프로젝트에 필요에 맞춰져 있어야 한다. 마찬가지로  Debian의 AppArmor도 시작 순간에 실행되고 있어야  한다.(Setting up CentOS is quite compcdlex.  Therefore, you don’t have to set up KDump.  However, SELinux must be running at startup and its  configuration has to be adapted for the  project’s needs. AppArmor for Debian must be  running at startup too.) |
| --- |



**LVM**을 이용하여 최소한 두 개의 암호화된 파티션을
 만들어야 한다. 아래는 예상되는 파티션에 대한 예시다.
 





![](/public/img/42-Born2beroot/img/img.png)












| ⓘ 디펜스에서 너가 선택한 운영체제에 대한 몇 가지  질문을 받을 것이다. 예를 들어 운영체제에서  aptitude와 apt의 차이나 SELinux나 AppArmor가  무엇인지에 대해 알고 있어야 한다. 요약하자면 너가  쓰는 것에 대해서는 이해하고 있어야 한다.(During the  defense, you will be asked a few questions about the  operating system you chose. For instance, you should  know the differences between aptitude and apt, or  what SELinux or AppArmor is. In short, understand  what you use!) |
| --- |



 SSH서비스는 4242포트에서만 작동할 것이다. 보안적인 이유로
 SSH를 루트로 사용해서 접속하는 것은 가능하지 않아야만
 한다.(A SSH service will be running on port 4242 only. For
 security concerns, it must not be possible to connect using
 SSH as root.)
 




| ⓘ 디펜스 동안 새 계정을 설정해서 SSH의 사용을  테스트할 것이다. 따라서 어떻게 작동하는지에 대해  이해하고 있어야 한다. (The use of SSH will be tested  during the defense by setting up a new account. You  must therefore understand how it works.) |
| --- |



 운영체제에서 UFW 방화벽을 가지고 설정해놓아야 하고, 따라서
 오로지 4242포트만 열어놓아야 한다.
 




| ⓘ 너가 만든 버추얼 머신을 런치시킬 때 너가 설정한  방화벽이 작동하고 있는 상태여야 한다. CentOS에서는  기본 방화벽 대신에 UFW를 사용해야 한다. 이를  설치하려면 아마 DNF를 사용해야할 것이다. (Your  firewall must be active when you launch your virtual  machine. For CentOS, you have to use UFW instead of  the default firewall. To install it, you will  probably need DNF.) |
| --- |


- 너가 만든 버추얼 머신의 호스트명(hostname)은 너의 로그인
 아이디 뒤에 42를 붙인 것이여야 한다 (e.g., wil42\).(The
 hostname of your virtual machine must be your login ending
 with 42 (e.g., wil42\).)
- 너는 강력한 암호 정책을 구현해야 한다.(You have to
 implement a strong password policy.)
- 너는 엄격한 규칙을 따라서 **sudo**를 설치하고 설정해야
 한다.(You have to install and configure sudo following
 strict rules.)
- 루트 사용자에 더해서, 너의 로그인 아이디를 사용자명으로
 하는 사용자도 존재해야 한다.(In addition to the root user,
 a user with your login as username has to be present.)
- 이 사용자는 **user42**와 **sudo**그룹에 속해야
 한다.(This user has to belong to the user42 and sudo
 groups.)




| ⓘ 디펜스를 하면서 새 사용자를 만들고 그룹에  할당해야 할 것이다.(During the defense, you will  have to create a new user and assign it to a  group.) |
| --- |



 강력한 암호 정책을 설정하기 위해서 다음의 요구사항들을
 따라야 한다.(To set up a strong password policy, you have to
 comply with the following requirements)
 


- 매 30일마다 너의 패스워드는 만료되어야 한다.(Your password
 has to expire every 30 days.)
- 패스워드 변경 전 허용되는 최소 일자는 2일로 설정될
 것이다(The minimum number of days allowed before the
 modification of a password will be set to 2\.)
- 사용자는 패스워드 만료 7일전 경고 메세지룰 받아야 한다(The
 user has to receive a warning message 7 days before their
 password expires.)
- 너의 패스워드는 최소 10자는 되어야 한다. 대문자와 숫자를
 포함해야 한다. 동일한 문자가 세 개 이상 연속으로 있어서는
 안된다. (Your password must be at least 10 characters
 long. It must contain an uppercase letter and a number.
 Also, it must not contain more than 3 consecutive
 identical characters.)
- 패스워드는 유저 네임을 포함해서는 안된다 (The password
 must not include the name of the user.)
- 패스워드는 이전의 패스워드의 일부분이 아닌 문자를 최소한
 7글자는 포함해야 한다.(The password must have at least 7
 characters that are not part of the former password.)
- 물론 너의 루트 계정도 이것들을 지켜야 한다.(Of course,
 your root password has to comply with this policy.)




| (!!) 환경 설정 파일을 셋팅하고 나면, 루트 계정을  포함한 버추얼 머신 상 모든 계정의 패스워드를 변경해야  한다.(After setting up your configuration files, you  will have to change all the passwords of the accounts  present on the virtual machine, including the root  account.) |
| --- |



 sudo 그룹에 (보안상?)강력한 설정을 하기 위해서 다음을 따라야
 한다.(To set up a strong configuration for your
 **sudo** group, you have to comply with the following
 requirements):
 


- sudo에 대한 인증은 틀린 패스워드의 경우에 세 번의 시도로
 제한돠어야 한다(Authentication using sudo has to be
 limited to 3 attempts in the event of an incorrect
 password.)
- sudo사용시 잘못된 패스워드 사용으로 인해 에러가 발생하면
 너가 만든 메시지가 띄워져야 한다. (A custom message of
 your choice has to be displayed if an error due to a wrong
 password occurs when using sudo.)
- sudo를 사용한 모든 행동은 입력과 출력 모두 기록되어
 보관되어야 한다. 로그 파일은 /var/log/sudo/folder에
 저장되어야 한다.( Each action using sudo has to be
 archived, both inputs and outputs. The log file has to be
 saved in the /var/log/sudo/ folder.)
- 보안상의 이유로 **TTY**모드를
 활성화해야한다  (The TTY mode has to be enabled
 for security matters.)
- 마찬가지로 보안상의 이유로 sudo를 통해 이용할 수 있는
 경로도 제한되어야만
 한다.(ex.  */usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin*)  
 ((For security matters too, the paths that can be used by
 sudo must be restricted.(Example:
 /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin))



 마지막으로 monitoring.sh라는 간단한 스크립트를 만들어야
 한다. 이는 bash를 통해 짜여져야 한다. (Finally, you have to
 create a simple script called monitoring.sh. It must be
 developed in bash.)  
  
서버 가동시 스크립트가 매
 10분마다 몇 개의 정보를 모든 터미널에 띄워줄 것이다(wall을
 찾아봐라). 배너는 옵션. 에러가 띄워져서는 안 된다. (At
 server startup, the script will put a few information on all
 terminals every 10 minutes (take a look at wall).The banner
 is optional. No error must be visible.)
 



  
너가 만든 스크립트는 항상 다음 정보를 띄울 수 있어야
 한다.(Your script must always be able to display the
 following information) :
 


- 운영체제의 아키텍쳐와 커널 버전( The architecture of your
 operating system and its kernel version.)
- 물리 프로세서의 갯수 (The number of physical processors.)
- 가상 프로세서의 갯수(The number of virtual processors.)
- 서버의 가용 RAM과 그것의 활용률을 퍼센테이지로(The
 available RAM on your server and its utilization rate as a
 percentage.)
- 서버의 가용 메모리와 그것의 활용률을 퍼센테이지로 (The
 available memory on your server and its utilization rate
 as a percentage.)
- 프로세서의 활용률을 퍼센테이지로( The utilization rate of
 your processors as a percentage.)
- 마지막 리부팅의 날짜와 시간(The date and time of the last
 reboot.)
- LVM이 활성화되어있는지 여부(Whether LVM is active or not.)
- 활성화되어있는 연결의 수(The number of active
 connections.)
- 서버를 이용하는 유저의 수(The number of users using the
 server.)
- 서버의 IPv4 주소와 서버의 맥 어드레스(The IPv4 address of
 your server and its MAC (Media Access Control) address.)
- sudo 프로그램으로 수행된 커맨드의 수(The number of
 commands executed with the sudo program.)




| ⓘ 디펜스에서 스크립트가 어떻게 작동하는지 설명할  것을 요구받을 것이다. 또한 그것(스크립트?)을  수정하는 일 없이 중단(?)시킬 수 있어야 한다. cron을  찾아봐라. (During the defense, you will be asked to explain how  this script works. You will also have to interrupt it  without modifying it. Take a look at cron.) |
| --- |



 다음은 스크립트가 작동했을 때 보여줄 것으로 예상되는 것의
 예시이다.(This is an example of how the script is expected
 to work) :
 





![](/public/img/42-Born2beroot/img/img_1.png)











 다음은 과제의 몇몇 요구사항들을 확인하기 위해 사용할 수 있는
 두 개의 커맨드이다.(Below are two commands you can use to
 check some of the subject’s requirements) :
 


CentOS에 대해서는 다음과 같다 :





![](/public/img/42-Born2beroot/img/img_2.png)










Debian에 대해서는 다음과 같다 :





![](/public/img/42-Born2beroot/img/img_3.png)












---


### Ch V.


#### Bonus part


보너스 목록 :


- 파티션을 정확히 나눠서 아래와 비슷한 구조를 갖도록
 만들어라.(Set up partitions correctly so you get a
 structure similar to the one below)





![](/public/img/42-Born2beroot/img/img_4.png)










- 다음의 서비스를 이용하여 가동되는(?) 워드프레스 웹사이트를
 만들어라. (Set up a functional WordPress website with the
 following services: lighttpd, MariaDB, and PHP.)
- 너가 생각하기에 유용하다고 생각하는 서비스를 직접 선택해서
 구성해라. (NGINX / Apach2 제외!). 디펜스에서 너가 한
 선택에 대한 합당한 근거를 대야 한다.(Set up a service of
 your choice that you think is useful (NGINX / Apache2
 excluded!). During the defense, you will have to justify
 your choice.)




| ⓘ 보너스 파트를 마치기 위해 추가적인 서비스를  설정해야 할 수 있다. 이 경우에 너의 필요에 따라 더  많은 포트를 열 수 있다. 물론 UFW 규칙은 상황에  맞춰서 조정되어야 한다. (To complete the bonus part, you have the possibility  to set up extra services. In this case, you may open  more ports to suit your needs. Of course, the UFW  rules has to be adapted accordingly.) |
| --- |




| (!!) 보너스 파트는 필수 파트가 완벽할 때에만 평가하게  될것이다. 완벽하다는 말은 필수 파트가 통합적으로  완성되었고 기능 불량 없이 작동함을 의미한다. 만약 필수  요구사항을 다 통과하지 못했다면 보너스 파트는 아예  평가받지 못할 것이다.(The bonus part will only be  assessed if the mandatory part is PERFECT. Perfect  means the mandatory part has been integrally done and  works without malfunctioning. If you have not passed  ALL the mandatory requirements, your bonus part will  not be evaluated at all.) |
| --- |
