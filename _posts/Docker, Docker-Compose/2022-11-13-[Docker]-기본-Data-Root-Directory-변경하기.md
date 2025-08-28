---
layout: post
title: "[Docker] 기본 Data Root Directory 변경하기"
date: "2022-11-13 18:08:42 +0900"
categories:
  - Docker, Docker-Compose
---
(내용에 오류가 있는 경우 댓글로 정정 부탁드립니다)  
  
도커
 데몬은 persistent하게 관리해야 할 데이터를 data\-root
 디렉토리에 저장한다. (ex. 컨테이너, 이미지, named
 volumes)  
  
해당 디렉토리는 따로 설정해주지 않는
 경우 `/var/lib/docker` 로 설정된다.  
  
일반적인
 경우에는 기본 설정 값을 사용해도 되지만 모종의 이유로 이를
 직접 설정해줘야 할 경우가 생길 수 있다. (ex. 여러 개의 도커
 데몬을 구동, 기본 data\-root의 스토리지의 용량 부족 문제,
 etc.)  
  
이런 경우에 `data-root` 설정
 값을 변경하고 기존 데이터를 가져오는 작업을 진행해보았다.  
  
(Ubuntu
 기준)
 




---


## 현재 설정된 Data Root Directory 확인



`docker info`커맨드에서
 `Docker Root Dir` 부분에서 확인할 수 있다.
 


간략하게 다음 커맨드로 확인해보자



```False
docker info | grep "Docker Root"
```




![](/public/img/Docker, Docker-Compose/img/img.png)












---


## How to



 전체적인 과정은 기존 data\-root 디렉토리를 새로 저장하고 싶은
 위치로 복사하는 과정과 data\-root 디렉토리 설정을 새로
 적용하여 도커 데몬을 실행하는 과정으로 나눌 수 있다.
 


1. 도커 서비스 중지  
보통은 도커 데몬을 직접 실행시키지
 않고 리눅스 시스템 서비스를 통해 실행시키므로 다음
 명령어를 통해 도커 데몬을 중지시킨다.
 



```False
sudo service docker stop
```
2. data\-root config 수정  
도커 데몬 기본 설정 파일은
 `/etc/docker/daemon.json`에 작성하면 된다.  
기존
 설정 파일이 존재한다면 `data-root` 프로퍼티를
 추가하고, 아니라면 파일을 생성한 후 다음과 같이
 작성한다.
 



```False
{  
"data-root": "새 저장 경로"
}
```
3. (기존 데이터를 가져오고 싶은 경우에만) 데이터 복사하기
 



```False
sudo cp -r /var/lib/docker ${새 저장 경로}
```


 cf) 기존 데이터를 가져오지 않는 경우에도 해당 경로를
 생성해줘야 한다. 경로를 생성해주지 않으면 도커 데몬 실행
 시 오류가 발생.
4. 도커 서비스 실행



```False
sudo service docker start
```
5. 제대로 설정되었는지 확인  
`docker info`
 커맨드를 통해 확인해보자.
