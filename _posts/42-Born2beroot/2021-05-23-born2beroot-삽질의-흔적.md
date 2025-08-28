---
layout: post
title: "born2beroot 삽질의 흔적"
date: "2021-05-23 18:41:42 +0900"
categories:
  - 42-Born2beroot
---
#### \* 저도 처음 해보는 작업이라 아래 적은 글에는 틀린 내용이
 있을 수도 있습니다.  또 '왜 그렇게 설정을 하는지'에
 대해서는 생략한 부분이 많습니다. 첫번째는 제가 잘 모르는
 부분도 많기 때문이고, 두번째는 제가 나중에 다시 설정할 때
 빠르게 필요한 부분만 읽기 위해서입니다. 생략한 내용은
 보통은 같이 첨부한 링크에 자세한 내용이 있습니다(간혹 없는
 경우도,,). 과제 평가 받으실 때에는 어느 정도 필요한
 수준만큼은 설명이 필요하므로 이 점 참고해서 아래 쓰여있는
 것보다는 더 자세히 공부하시는 것을 추천드립니다.


#### <https://github.com/wshloic/born2beroot_correction/blob/master/correction_born2beroot.pdf>
 (평가 받기 전 점검 필수)



[wshloic/born2beroot\_correction
 



 Contribute to wshloic/born2beroot\_correction
 development by creating an account on GitHub.
 


github.com](https://github.com/wshloic/born2beroot_correction)



---


#### 가상 머신 설치(**yesim**님이 많이 도와주셨습니다)


- 우선 command \+ space를 눌러서 spotlight를 부르고 msc를
 검색해서 managed software center를 연다.
- 거기서 virtualBox를 검색해서 설치하면 관리자 권한 없이
 버추얼 박스 설치 가능.
- 그 후 버추얼 박스를 실행한 다음 새로 만들기를 클릭.
- 이름 적당히 짓고, 머신 폴더의 경로는 파일이 주기적으로
 사라지는 '그 경로'로 설정한다.
- '그 곳'의 맥은 일반적인 경로는 용량 제한이 있으므로 용량
 제한이 없는 그 곳을 경로로 설정해야 한다.
- 선택한 os에 따라 설정을 하고 난 후, '다음'을 클릭.
- 메모리는 주어진 1024mb를 받아들이고 또 '다음'을 클릭.
- '새 가상 하드디스크 만들기' , 'VDI' , '동적 할당' 선택
- 용량도 8기가 선택.
- 데비안과 센트os 중 데비안을 깔기로 마음먹었기 때문에
 구글에서 'debian stable release download'를 검색해서
 iso파일을 받은 후 머신의 광학 디스크에 마운트.
- 그 후 머신을 시작하면 다음과 같은 화면이 뜬다.


 





![](/public/img/42-Born2beroot/img/img-4.png)




 맨 첫 두 줄 중 아무 거나 선택해도 진행은 동일한 것 같다.
 (설치 도중 gui 깔 것인지 물어보는 상황에서 체크 해제할
 수 있음)\&amp;nbsp; 저는 두번째 줄을 선택.
 





![](/public/img/42-Born2beroot/img/img_1-3.png)











 맨 첫 두 줄 중 아무 거나 선택해도 진행은 동일한 것 같다.
 (설치 도중 gui 깔 것인지 물어보는 상황에서 체크 해제할 수
 있음) 저는 두번째 줄을 선택.
 





![](/public/img/42-Born2beroot/img/img_2-3.png)









![](/public/img/42-Born2beroot/img/img_3-3.png)









![](/public/img/42-Born2beroot/img/img_4-3.png)



저의 국적은 남쪽입니다.





![](/public/img/42-Born2beroot/img/img_5.png)









![](/public/img/42-Born2beroot/img/img_6.png)




 여기까지 선택하고 나면 뭔가 설정이 자동으로 진행된다.
 





![](/public/img/42-Born2beroot/img/img_7.png)




 호스트네임은 문제에서 요구하는 대로 채워주시면 된다.
 





![](/public/img/42-Born2beroot/img/img_8.png)



여기는 빈 칸으로





![](/public/img/42-Born2beroot/img/img_9.png)



루트 암호 입력





![](/public/img/42-Born2beroot/img/img_10.png)



다시 입력





![](/public/img/42-Born2beroot/img/img_11.png)




 여기서 문제에서 요구되는 user명을 fullname에 입력하면,,
 





![](/public/img/42-Born2beroot/img/img_12.png)




 다음 단계의 username에도 입력이 되어 있다
 





![](/public/img/42-Born2beroot/img/img_13.png)



마찬가지로 비밀번호 두 번 입력





![](/public/img/42-Born2beroot/img/img_14.png)




 문제에서 요구하는 대로 encrypted LVM 선택
 





![](/public/img/42-Born2beroot/img/img_15.png)



선택





![](/public/img/42-Born2beroot/img/img_16.png)




 문제에서 요구하는 LV를 위해 seperate 선택
 





![](/public/img/42-Born2beroot/img/img_17.png)



yes를 선택





![](/public/img/42-Born2beroot/img/img_18.png)




 LVM encryption에 대한 암호 두 번 입력
 





![](/public/img/42-Born2beroot/img/img_19.png)



8\.1로 진행





![](/public/img/42-Born2beroot/img/img_20.png)



진행





![](/public/img/42-Born2beroot/img/img_21.png)



예쓰





![](/public/img/42-Born2beroot/img/img_22.png)



고 쓰루





![](/public/img/42-Born2beroot/img/img_23.png)



적당한 것 선택





![](/public/img/42-Born2beroot/img/img_24.png)



이것도 적당한 것으로\~





![](/public/img/42-Born2beroot/img/img_25.png)



비워두기





![](/public/img/42-Born2beroot/img/img_26.png)



노우





![](/public/img/42-Born2beroot/img/img_27.png)



마지막 두 개만





![](/public/img/42-Born2beroot/img/img_28.png)



No. 다른 곳에 설치할 것





![](/public/img/42-Born2beroot/img/img_29.png)



직접 쓰자





![](/public/img/42-Born2beroot/img/img_30.png)




 문제 요구사항에 맞추기 위해 (보너스는 좀 다름)
 





![](/public/img/42-Born2beroot/img/img_31.png)



설치 완료








---


#### GRUB 부트로더



 (<https://m.blog.naver.com/dudwo567890/130158001734>
 참고)  
부트로더 : 리눅스가 부팅되기까지 전 과정 진행하는
 프로그램. 현재 대표적인 것이 GRUB
 




---


#### Vim 설치



 sudo apt\-get install vim
 




---


#### CentOS vs. Debian



[https://www.educba.com/centos\-vs\-debian/](https://www.educba.com/centos-vs-debian/)  
[https://www.openlogic.com/blog/centos\-vs\-debian](https://www.openlogic.com/blog/centos-vs-debian)
  
[https://coding\-factory.tistory.com/318](https://coding-factory.tistory.com/318)
  
[https://m.blog.naver.com/PostView.naver?isHttpsRedirect\=true\&blogId\=hymne\&logNo\=220976678541](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=hymne&logNo=220976678541)
 (ext4, xfs 비교)
 


- 패키지 관리 : 센트오에스는 RPM 포맷과 YUM/DNF 패키지
 매니저 사용. 데비안은 DEB 포맷과 dpkg/APT 매니저 사용
- 파일 시스템 : 센트오에스 디폴트는 XFS. 데비안 디폴트는
 EXT4
- 업그레이드 : from 센트오에스 7\.8 to from 센트오에스 7\.9
 \-\> OK. from 센트오에스 6 to 센트오에스 7 \-\> not OK.
 But, 데비안은 둘 다 OK
- 지원 : 센트오에스는 레드햇 공개한 RHEL을 가져와서 레드햇의
 브랜드와 로고만 제거하고 배포한 배포본. 무료 사용
 가능하지만 문제 발생시 레드햇이 아닌 커뮤니티 통해
 지원되므로 패치가 다소 느린 감이 있음. 데비안은 오래
 된만큼 커뮤니티에 강점.




---


#### APT vs. aptitude



[https://www.tecmint.com/difference\-between\-apt\-and\-aptitude/](https://www.tecmint.com/difference-between-apt-and-aptitude/)  
[https://unix.stackexchange.com/questions/767/what\-is\-the\-real\-difference\-between\-apt\-get\-and\-aptitude\-how\-about\-wajig](https://unix.stackexchange.com/questions/767/what-is-the-real-difference-between-apt-get-and-aptitude-how-about-wajig)  
[https://ksbgenius.github.io/linux/2021/01/13/apt\-apt\-get\-difference.html](https://ksbgenius.github.io/linux/2021/01/13/apt-apt-get-difference.html)
  
<http://linuxmint.kr/Etc/2062>  
APT는 low\-level 패키지 매니저. aptitude는 high\-level
 패키지 매니저  
apt는 다른 하이레벨 패키지 매니저에 의해
 사용될 수 있음
 





![](/public/img/42-Born2beroot/img/img_32.png)




 aptitude는 터미널 메뉴 인터페이스를 제공한다.
 







 그 외에 CLI만 고려했을 때의 마이너한 차이점은 다음과 같다.
 


- aptitude는 사용하지 않는 패키지를 자동적으로 제거해준다.
 반면 apt는 추가적 옵션이 필요하다. (ex. 'autoremove',
 '\-auto\-remove')
- apt에서 각각 upgrade와 dist\-upgrade로 쓰이던 커맨드는
 safe\-upgrade와 full\-upgrade로 바뀌었다.(보다 명확히 하기
 위해?)
- aptitude는 apt\-get 외에 apt\-chche, apt\-mark와 같은 툴도
 같이 사용한다.
- aptitude는 검색에서 조금 다른 쿼리 신택스를 사용한다
 (apt\-cache와 비교했을 때)
- aptitude는 why와 why\-not 커맨드를 통해 특정 패키지를
 설치할 때 어떤 것이 요구되고, 어떤 것과 충돌하는지 확인할
 수 있다.
- aptitude는 설치, 제거, 업데이트 과정에서 충돌이 있는 경우
 다른 대안을 제시해줌. apt는 그냥 안 된다고만 함.




---


#### AppArmor \&\& SELinux(깃랩 주소로 있는 곳 가서 공식
 docu읽는 것이 제일 이해가 잘 되는듯..)



[https://www.redhat.com/en/topics/linux/what\-is\-selinux](https://www.redhat.com/en/topics/linux/what-is-selinux)  
<https://linuxhint.com/debian_apparmor_tutorial/>  
<https://wiki.debian.org/AppArmor/HowToUse>  
<https://2infinity.tistory.com/59>  
<https://linuxreviews.org/AppArmor>
 (앱아머 설명)  
[https://gitlab.com/apparmor/apparmor/\-/wikis/home](https://gitlab.com/apparmor/apparmor/-/wikis/home)  
[https://debian\-handbook.info/browse/stable/sect.apparmor.html](https://debian-handbook.info/browse/stable/sect.apparmor.html)



- 둘 다 security framework
- SELinux는 IBM/RedHat 계열에서 선호되고 같은 계열의
 CentOS와 같은 운영체제에서 사용된다. 나머지는 AppArmor사용
- SELinux는 policy file과 right file system을 통해 작동.
 AppArmor는 policy file만으로 작동. SELinux가 조금 더
 복잡하고 표준화하여 설정하기 어려운 단점.
- 데비안에서는 기본적으로 깔려 있으나 깔려 있지 않다면 "apt
 install apparmor apparmor\-utils"을 통해 설치 가능.
- "aa\-enabled" 명령어 통해 활성화 여부 확인 가능
- 앱아머는 정책 파일을 통해 어떤 어플리케이션이 어떤
 파일/경로에 접근 가능한지 허용해준다.
- enforce모드와 complain모드 두 가지 존재
- enforce 모드 : 허가되지 않은 파일에 접근하는 것을 거부하는
 모드
- complain 모드 : 실질적으로 보안을 제공하는 것은 아님. 대신
 어플리케이션이 해야 할 행동이 아닌 다른 행동을 하는 경우에
 앱아머는 로그를 남겨준다(중지하지는 않음).
- "sudo aa\-status"통해 현재 상태 확인 가능(enforced,
 complain, unconfined)
- "ps auxZ \| grep \-v '^unconfined'" 통해 현재 앱아머에 의해
 제한된 실행 파일 확인 가능




---


#### UFW 방화벽 설정하기



[https://www.tecmint.com/setup\-ufw\-firewall\-on\-ubuntu\-and\-debian/](https://www.tecmint.com/setup-ufw-firewall-on-ubuntu-and-debian/)  
<https://webdir.tistory.com/206>  
<https://wiki.ubuntu.com/UncomplicatedFirewall>
 (ufw 장점 : 쉽다! Uncomplicated FireWall의 약자)
 


1. "sudo apt install ufw" \-\> ufw 설치
2. "sudo ufw status verbose"로 ufw 상태 확인 (디폴트는
 inactive)
3. "sudo ufw enable"로 부팅 시 ufw 활성화되게 설정
4. "sudo ufw default deny"로 기본 incoming deny로 설정
5. "sudo ufw allow 4242"로 ssh연결 허용(4242라는 커스텀 포트
 사용하는 경우)
6. 정책 삭제 원하는 경우 "sudo ufw status numbered"로 지우고
 싶은 규칙 확인후 "sudo ufw delete 규칙번호"입력




---


#### SSH 서버 설정하기



[https://phoenixnap.com/kb/how\-to\-enable\-ssh\-on\-debian](https://phoenixnap.com/kb/how-to-enable-ssh-on-debian)  
[https://linuxhint.com/enable\-ssh\-server\-debian/](https://linuxhint.com/enable-ssh-server-debian/)  
[https://m.blog.naver.com/PostView.naver?blogId\=jodi999\&logNo\=221334854192\&proxyReferer\=https:%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.naver?blogId=jodi999&logNo=221334854192&proxyReferer=https:%2F%2Fwww.google.com%2F)  
[https://www.cyberciti.biz/faq/howto\-change\-ssh\-port\-on\-linux\-or\-unix\-server/](https://www.cyberciti.biz/faq/howto-change-ssh-port-on-linux-or-unix-server/)  
<https://mrgamza.tistory.com/506>
 (ssh서버 접속하기)  
[https://linuxize.com/post/how\-to\-change\-ssh\-port\-in\-linux/](https://linuxize.com/post/how-to-change-ssh-port-in-linux/)
 (ssh포트 변경 이슈)  
[https://medium.com/@jamessoun93/ssh%EB%9E%80\-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94\-87b58c521d6f](https://medium.com/@jamessoun93/ssh%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94-87b58c521d6f)
 (ssh의 장점 : 암호화)
 


1. "apt search openssh\-server"를 입력하여 openssh가 깔려
 있는지 확인. 깔려 있지 않다면 "apt install
 openssh\-server"를 통해 설치.
2. 설치 후 "systemctl status ssh"를 통해 openssh 실행 여부와
 사용 포트를 확인할 수 있다.
3. "sudo ufw allow 4242" (ufw참조)로 4242 포트 허용
4. "sudo vim /etc/ssh/sshd\_config"를 통해 ssh 설정을 변경.
 Port 22라 되어있는 줄을 Port 4242로 바꾸고 앞에 \#이 붙어서
 주석 처리되어있으면 \#을 삭제해준다.
5. "sudo systemctl restart ssh"로 재시작하여 설정 적용




---


#### sudo 커맨드 설치 \&\& 사용자 그룹 설정



 ([https://unix.stackexchange.com/questions/354928/bash\-sudo\-command\-not\-found](https://unix.stackexchange.com/questions/354928/bash-sudo-command-not-found)를 참고함)  
데비안을 처음 설치하면 sudo 명령어가
 설치되어 있지 않다.  
다음 과정을 통해 sudo를 사용하게 할
 수 있다.
 


1. "dpkg \-l sudo" 통해 sudo 설치 여부 확인 가능
2. "su" 또는 "su \-"를 통해 root권한으로 일시적으로 넘어갈 수
 있다. ("su"의 "\-"옵션에 대해서는 다음 블로그 참조 :
 <https://storycompiler.tistory.com/44>)
3. "apt\-get install sudo \-y"를 통해 sudo를 설치. (\-y 옵션을
 통해 설치 시 뜨는 프롬프트들에 자동적으로 yes로 대답할 수
 있다)
4. "groupadd user42"로 user42라는 그룹을 추가
5. "usermod \-aG sudo 사용자명"을 통해 특정 사용자를 sudo라는
 그룹에 추가시킬 수 있다. "usermod \-aG sudo,user42
 chanlee"라는 명령어를 통해 문제에서 요구한대로
 "user42"라는 그룹에도 속할 수 있게 해주었다(그룹이 여러
 개인 경우 공백 없이 콤마로 구분). cf) usermod에 \-G옵션을
 붙이면 명시한 그룹에 사용자가 속하게 되고, 현재 사용자가
 속해 있는 그룹이 명시되지가 않았다면 해당 그룹에 속하지
 않게 된다. (ex. chanlee라는 사용자가 user42와 home이라는
 그룹에 속한 상태에서 "usermod \-G sudo,user42 chanlee"를
 실행하는 경우 sudo와 user42에만 속하게 된다.) 이때
 \-a옵션(append)을 붙이게 되면 커맨드에 적어준 그룹에
 '추가적으로' 들어가게 되는 효과만 발생한다.(useradd를
 사용할 수도 있음 :
 <https://m.blog.naver.com/wideeyed/221512008307>)
 

![](/public/img/42-Born2beroot/img/img_33.png)
6. 그 후 "usermod \-g user42 chanlee"를 통해 사용자 chanlee의
 primary group이 user42가 되게 했다. (cf. 해당 사용자의
 홈디렉토리 내 파일 권한은 자동으로 변경되고 그 외의
 파일들은 직접 수정이 필요하다고 man에 나옴)![](/public/img/42-Born2beroot/remote/blog.kakaocdn.net/dn/lhLU9/btq5toXs47V/YrY3cHwBkt9nXKWlJv5m40/img-2.png)
7. "sudo deluser 사용자명 그룹명"을 통해 그룹에서 사용자를
 제거할 수 있다.
8. "sudo userdel \-r 사용자명" 통해 사용자 제거 가능.




---


#### 호스트네임 및 파티셔닝



[https://linuxize.com/post/how\-to\-change\-hostname\-in\-linux/](https://linuxize.com/post/how-to-change-hostname-in-linux/)  
[https://youngmind.tistory.com/entry/CentOS\-%EA%B0%95%EC%A2%8C\-PART\-1\-8\-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80\-%EA%B4%80%EB%A6%AC\-%E3%85%A3%E3%85%8D](https://youngmind.tistory.com/entry/CentOS-%EA%B0%95%EC%A2%8C-PART-1-8-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EA%B4%80%EB%A6%AC-%E3%85%A3%E3%85%8D)
 (lvm 설명)
 


- 호스트명 체크 커맨드 : "hostnamectl"
- 호스트명 변경 커맨드 : "sudo hostnamectl set\-hostname
 바꾸려는호스트명"



 LVM 개략적 느낌 : 물리적 매체가 있고 이를 PV(Physical
 Volume)로 나눈다. 이들을 적당히 모아서 VG(Volume Group)로
 구성. 그 후 각 VG를 LV(Logical Volume)으로 나누어서 사용.
 




---


#### SSH 서버 설정하기



[https://phoenixnap.com/kb/how\-to\-enable\-ssh\-on\-debian](https://phoenixnap.com/kb/how-to-enable-ssh-on-debian)  
[https://linuxhint.com/enable\-ssh\-server\-debian/](https://linuxhint.com/enable-ssh-server-debian/)  
[https://m.blog.naver.com/PostView.naver?blogId\=jodi999\&logNo\=221334854192\&proxyReferer\=https:%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.naver?blogId=jodi999&logNo=221334854192&proxyReferer=https:%2F%2Fwww.google.com%2F)  
[https://www.cyberciti.biz/faq/howto\-change\-ssh\-port\-on\-linux\-or\-unix\-server/](https://www.cyberciti.biz/faq/howto-change-ssh-port-on-linux-or-unix-server/)  
<https://mrgamza.tistory.com/506>
 (ssh서버 접속하기)  
[https://linuxize.com/post/how\-to\-change\-ssh\-port\-in\-linux/](https://linuxize.com/post/how-to-change-ssh-port-in-linux/)
 (ssh포트 변경 이슈)  
[https://medium.com/@jamessoun93/ssh%EB%9E%80\-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94\-87b58c521d6f](https://medium.com/@jamessoun93/ssh%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94-87b58c521d6f)
 (ssh의 장점 : 암호화)
 


1. "apt search openssh\-server"를 입력하여 openssh가 깔려
 있는지 확인. 깔려 있지 않다면 "apt install
 openssh\-server"를 통해 설치.
2. 설치 후 "systemctl status ssh"를 통해 openssh 실행 여부와
 사용 포트를 확인할 수 있다.
3. "sudo ufw allow 4242" (ufw참조)로 4242 포트 허용
4. "sudo vim /etc/ssh/sshd\_config"를 통해 ssh 설정을 변경.
 Port 22라 되어있는 줄을 Port 4242로 바꾸고 앞에 \#이 붙어서
 주석 처리되어있으면 \#을 삭제해준다.
5. "sudo systemctl restart ssh"로 재시작하여 설정 적용




---


#### VM에서 포트포워딩 설정하기



<https://gist.github.com/wacko/5577187>




[SSH between Mac OS X host and Virtual Box guest
 



 SSH between Mac OS X host and Virtual Box guest.
 GitHub Gist: instantly share code, notes, and
 snippets.
 



 gist.github.com](https://gist.github.com/wacko/5577187)

1. 일단 위의 깃헙 링크대로 설정
2. 호스트 컴퓨터(맥)에서 ifconfig 치면 나오는 vboxnet0의
 inet주소 기억
3. 가상머신에서 ifconfig 통해 enp0s3 비슷한 이름이 붙은 것의
 inet주소 기억(sudo apt\-get net\-tools로 넷툴즈 설치해야
 ifconfig 사용 가능)
4. 해당 가상머신 설정 \-\> 네트워크 \-\> NAT \-\> advanced
 \-\> port forwarding \-\> rule 이름 적당히 짓고 위에서
 기억한 ip 주소들을 기입 \&\& 포트는 호스트/게스트
 전부 4242로 설정
5. 호스트 컴퓨터 쉘에서 "ssh \-p 4242
 사용자명@가상머신ip주소"치면 접속 가능 (처음엔 됐었는데
 나중에 안 되는 경우 여기 참조
 https://m.blog.naver.com/PostView.naver?isHttpsRedirect\=true\&blogId\=midovan1\&logNo\=220250544307



 ssh 접속 root 계정 제한 방법
 


1. "vi /etc/ssh/sshd\_config"로 ssh 설정 파일 열기
2. PermitRootLogin옵션 주석 해제하고 no로 값 변경
3. 파일 저장하고 "service ssh restart"



 특정 계정에 su 획득 권한 부여  
\-
 <https://wiki.debian.org/WHEEL/PAM>



1. vi /etc/pam.d/su로 들어가서 auth required pam\_wheel.so주석
 해제
2. addgroup \-\-system wheel을 통해 루트 권한 가진 그룹
 'wheel'생성
3. adduser 사용자명 wheel을 통해 특정 사용자를 wheel그룹에
 넣음으로써 su 권한 부여




---


#### Password 정책 변경



[https://www.2daygeek.com/how\-to\-set\-password\-complexity\-policy\-on\-linux/](https://www.2daygeek.com/how-to-set-password-complexity-policy-on-linux/)  
[https://www.linuxtechi.com/enforce\-password\-policies\-linux\-ubuntu\-centos/](https://www.linuxtechi.com/enforce-password-policies-linux-ubuntu-centos/)  
[https://webhostinghero.org/enforce\-password\-change\-policies\-in\-linux\-server\-security\-checklist/](https://webhostinghero.org/enforce-password-change-policies-in-linux-server-security-checklist/)



1. "sudo vi /etc/login.defs"친 후, "PASS\_MAX\_DAYS 30",
 "PASS\_MIN\_DAYS 2", "PASS\_WARN\_AGE 7", "PASS\_MIN\_LEN
 10"으로 변경.
2. "sudo apt install libpam\-pwquality"를 통해 패키지 설치
3. 설치 후 "sudo vi /etc/pam.d/common\-password" 입력
4. ![](/public/img/42-Born2beroot/img/img_34.png)




 위와 같이 내용을 수정한다.(maxreapeat은
 오타\-\>maxrepeat)
5. "passwd \-e 사용자명" 통해 root계정과 현존하는 사용자
 계정의 암호 변경을 강제한다. 다음 번 로그인시에 암호를
 변경하라고 뜨게 된다.


- retry\=3 : 암호 입력 3회까지
- minlen\=10 : 암호 최소 길이는 10
- difok\=7 : 기존 패스워드와 달라야하는 문자 수는 7
- ucredit\=\-1 : 대문자 한 개 이상
- lcredit\=\-1 : 소문자 한 개 이상
- dcredit\=\-1 : 숫자 한 개 이상
- reject\_username : username이 그대로 또는 뒤집혀서 새
 패스워드에 들어있는지 검사하고, 들어있으면 거부
- enforce\_for\_root : root 사용자가 패스워드를 바꾸려고 하는
 경우에도 위의 조건들 적용





![](/public/img/42-Born2beroot/img/img_35.png)



패스워드 정책의 효용








---


#### sudoer 파일 설정



[https://info\-lab.tistory.com/163](https://info-lab.tistory.com/163)  
[https://www.sudo.ws/man/1\.8\.14/sudoers.man.html](https://www.sudo.ws/man/1.8.14/sudoers.man.html)  
<https://www.tuwlab.com/ece/24044>  
'man sudoers'참조
 


- "sudo mkdir /var/log/sudo/"를 통해 로그 파일을 저장할 경로
 생성.
- "sudo visudo"를 통해 '/etc/sudoers'파일을 수정 가능.
- 파일에서 'Defaults secure\_path\="경로"'에서 경로부분을
 "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"로
 수정해준다.(sudo를 통해 실행되는 명령어의 경로를 명시된
 경로로 제한해준다. sudo만의 $PATH 환경변수를
 설정해주는듯하다.)
- 마지막 줄에 다음을 입력.



```False
Defaults    authfail_message="Authentication attempt failed."
Defaults    badpass_message="Wrong password!"
Defaults    log_input
Defaults    log_output
Defaults    requiretty
Defaults    iolog_dir="/var/log/sudo/"
```


 authfail\_message\="메세지" : 권한 획득 실패시 띄울 커스텀
 메세지  
badpass\_message\="메세지" : 암호 실패시 띄울
 메세지  
log\_input : sudo를 통해 입력된 input은 로그에
 기록된다.  
log\_output : sudo를 통해 입력된 output은
 로그에 기록된다.  
requiretty : tty에 연결되지 않은 채로
 sudo를 실행하는 것을 금지? ex. 쉘 스크립트 상에서 sudo
 커맨드 수행 금지.  
iolog\_dir\="경로" : 로그를 저장할
 경로.
 




---


#### monitoring.sh 작성



[https://myshell.co.uk/blog/2012/07/how\-to\-determine\-the\-number\-of\-physical\-cpus\-on\-linux/](https://myshell.co.uk/blog/2012/07/how-to-determine-the-number-of-physical-cpus-on-linux/)  
[https://superuser.com/questions/1003057/what\-is\-the\-exact\-different\-between\-linux\-kernel\-release\-and\-version](https://superuser.com/questions/1003057/what-is-the-exact-different-between-linux-kernel-release-and-version)
 커널 릴리즈와 버전의 차이  
[https://www.cyberciti.biz/faq/check\-how\-many\-cpus\-are\-there\-in\-linux\-system/](https://www.cyberciti.biz/faq/check-how-many-cpus-are-there-in-linux-system/)
 물리적 프로세서 갯수  
[https://webhostinggeeks.com/howto/how\-to\-display\-the\-number\-of\-processors\-vcpu\-on\-linux\-vps/](https://webhostinggeeks.com/howto/how-to-display-the-number-of-processors-vcpu-on-linux-vps/)
 가상 프로세서 갯수  
[https://stackoverflow.com/questions/10585978/how\-to\-get\-the\-percentage\-of\-memory\-free\-with\-a\-linux\-command](https://stackoverflow.com/questions/10585978/how-to-get-the-percentage-of-memory-free-with-a-linux-command)
 메모리 사용량 퍼센테이지로 나타내기 with AWK  
[https://stackoverflow.com/questions/9229333/how\-to\-get\-overall\-cpu\-usage\-e\-g\-57\-on\-linux](https://stackoverflow.com/questions/9229333/how-to-get-overall-cpu-usage-e-g-57-on-linux)
 cpu 사용량 퍼센테이지  
[https://phoenixnap.com/kb/check\-cpu\-usage\-load\-linux](https://phoenixnap.com/kb/check-cpu-usage-load-linux)
 cpu 사용량 퍼센테이지 두번째 방법  
[https://www.golinuxcloud.com/list\-check\-active\-ssh\-connections\-linux/](https://www.golinuxcloud.com/list-check-active-ssh-connections-linux/)
 number of active connections  
[https://www.letmecompile.com/scheduler\-cron\-tutorial/](https://www.letmecompile.com/scheduler-cron-tutorial/)
 cron  
<https://zetawiki.com/wiki/%ED%81%AC%EB%A1%A0%ED%83%AD_%EC%9E%91%EC%97%85_5%EB%B6%84%EB%A7%88%EB%8B%A4_%EC%88%98%ED%96%89>
 cron 매 n분마다 실행
 


- 기존에 있던 내용은 치트 시트 같아서 비공개 처리했습니다..
 printf, sed, awk, 조건문 등을 잘 활용하시면 됩니다
