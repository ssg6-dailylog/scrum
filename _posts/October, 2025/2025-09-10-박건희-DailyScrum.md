---
layout: scrum
title: "외주 마감 (mark crm)"
date: 2025-10-10
author: "박건희"
categories: [scrum]
---

## 📝 어제 한 일

- 외주) 최종 배포 완료

### 상세 내용

1. 로그인 및 토큰 처리 문제 해결
- 문제 상황 : 로그인 후 최초 1회 accessToken refresh 시 에러 발생
- 증상 : 리프레시 자체는 성공하지만, 그 직후 상태가 고임
- 원인 : 로그인시 form submit과 onclick 이벤트가 중복으로 발생했었음. 그로 인해 두번째 요청에서 스토어의 state가 비정상적으로 덮어씌워짐
- 추가 문제 : 서버 강제 종료 시 리프레시 요청 무한 루프 발생
- 해결 : 인터셉터에서 refresh 요청 자체를 건드리지 않도록 axios.js 수정

2. 베이직 테이블 컴포넌트 생성
- 필터, 정렬, 페이지네이션, 버튼처리를 담당할 수 있는 공용 테이블 컴포넌트 생성
- 테이블에서 사용될 컴포저블과 전체 페이지에서 사용될 글로벌 컴포저블도 접목하여 코드의 재사용성을 키움
- 글로벌&테이블 컴포저블 구조
    1. Header.vue에서 날짜, 카테고리, 키워드 입력 -> 글로벌 컴포저블의 status 변경
    2. 테이블 컴포저블이 글로벌 컴포저블의 값 변경 감시 (watch)
    3. 백엔드로 테이블에서 발생되는 이벤트 정보 전달 (페이지네이션, 필터, 버튼 클릭 등) 

3. 구글 스프레드시트 API
- 구글 계정의 2차 인증 필요
- 읽고자 하는 스프레드시트의 ID와 시트, 읽을 범위 필요 (DB로 관리)
- 어디까지 데이터를 읽어들였는지는 커서로 관리 (데이터를 읽을 때는 완전행인지 판단하여 입력받음, 완전행이 아니라면 해당 위치에서 커서는 더이상 전진하지 않음 - 구글 폼의 필수/선택 응답에 대한 백엔드 적용)
- 계정에 구글 API 활성화 시키고 JSON 키 발급 필요

4. 구글 메일 발송 API
- 구글 계정의 2차 인증 필요
- 계정에서 생성되는 API 용 비밀번호 키 필요

5. flatpickr UIUX 문제
- HTML에서 기본 제공되는 날짜 위젯이 아니라 디자인 요소가 들어간 위젯을 사용하려다보니 다루는 것이 복잡했음. 그냥 사용하는 것은 상관없으나, 의뢰인의 요구에 맞춰 디자인을 변경하는 것에 어려움을 겪음
- 브라우저의 요소 보기를 통해 flatpickr이 정확히 어느 태크 아래서 작동하는지, 어떤 style이 적용되는지 확인해서 원하는 디자인 적용하는데 성공

6. 승인 및 권한 규칙
- 일괄 승인은 super 계정만 가능
- super 계정은 자기 정보 수정 불가
- 관리자 계정의 본사 계정 수정 불가
- 소속이 본사인 경우 수정 불가 → 구분 변경 후만 수정 가능
- 소속 본사 → 구분 관리자로 수정 시만 변경 가능
- 상태 변경 시:
    1. 관리자 → 센터장/스탭: 소속 NULL
    2. 센터장/스탭 → 관리자: 소속 본사(1)
- 가시 권한 변경:
    1. 관리자 → 센터장/담당자: visible = Y
    2. 센터장/담당자 → 관리자: visible = N

7. 탈퇴 회원 처리
- 탈퇴 회원은 로그인 불가
- 탈퇴 회원과 동일한 아이디로 재가입 불가 → 재가입 원할 시, 직원관리 페이지에서 해당 아이디의 상태를 승인으로 변경
- 탈퇴 도중 API 호출 시 → access token 만료 후로는 추가로 리프레시 금지되고 강제 로그아웃 됨

8. 최초, 유효, 중복 DB 구분 정책
- 원본 DB에 입력하고자 하는 전화번호가 존재하지 않는다면 -> 최초로서 원본 DB에 입력
- 원본 DB에 전화번호가 존재하며, 최근 30일 이내에 이력이 남아있다면 -> 중복 테이블에 추가
- 원본 DB에 전화번호가 존재하지만 최근 30일 이내에는 이력이 존재하지 않는다면 -> 유효로 원본 DB에 입력

9. 테이블에 로딩 스피너 추가
- 데이터가 너무 많으면 로딩에 1~2초씩도 걸림 (100만개 데이터 기준)
- 이럴 때 사용자의 무분별한 새로고침, 버튼 클릭으로 서버에 과부하 걸리는 상황을 피하고자 프론트에서 클릭 -> 백엔드 응답까지 로딩 스피너를 화면에 출현시켜 사용자의 반복적인 이벤트 발생을 막음

10. 이메일 인증과 유휴시간 적용
- 모든 이메일 인증을 하는 페이지 (로그인, 회원가입, 비밀번호 초기화, 내 정보 수정) 에서는 인증 완료 후 사용자의 이벤트 (마우스 이동, 클릭 등)이 없을 경우 페이지를 리프레시 하도록 유휴 시간을 적용함
- 이를 통해 이메일 인증이 완료된 상태로 장시간 방치되어 중요한 개인정보가 수정되는 일이 없도록 방지

11. 배포 매뉴얼화 (백엔드, 프론트엔드, RDS, Elasticache)

0. .pem 키 권한 설정 변경

- 현재 권한 확인
icacls "C:\Users\qkrwm\Desktop\blue\release\bluemark-key.pem"

- 권한 삭제
icacls "C:\Users\qkrwm\Desktop\blue\release\bluemark-key.pem" /inheritance:r
icacls "C:\Users\qkrwm\Desktop\blue\release\bluemark-key.pem" /remove:g Everyone
icacls "C:\Users\qkrwm\Desktop\blue\release\bluemark-key.pem" /remove:g Users

- 내 계정만 권한 주기
icacls "C:\Users\qkrwm\Desktop\blue\release\bluemark-key.pem" /grant:r "qkrwm:R"

1. 기본 패키지 설치
sudo dnf -y update
sudo dnf -y install nginx git

- Node.js 20 설치 (Amazon Linux 2023 권장)
sudo dnf -y install nodejs npm

- 버전 확인
node -v
npm -v

2. 프로젝트 클론
cd ~
git clone https://github.com/gun-seon/blue_crm.git frontend
cd ~/frontend/frontend

- 원하는 브랜치 체크아웃
git fetch origin
git checkout release/main-v2

- 브랜치 확인
git branch

3. 빌드
ls -al   # package.json 있는지 확인
npm install
npm run build

cd ~/frontend/frontend
ls -a # dist 생겼는지 확인

4. Nginx 설정
sudo mkdir -p /var/www/mark-frontend
sudo cp -r dist/* /var/www/mark-frontend/

- 재배포시
sudo rm -rf /var/www/mark-frontend/*
sudo cp -r dist/* /var/www/mark-frontend/

- Nginx config 파일 생성
sudo nano /etc/nginx/conf.d/mark-frontend.conf

- 파일 내용
server {
    listen 80;
    server_name markcrm25.com www.markcrm25.com;

    root /var/www/mark-frontend;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://172.31.79.184:8080;   # 백엔드 프라이빗 IP
        proxy_http_version 1.1;
        proxy_set_header Host              $host;
        proxy_set_header X-Real-IP         $remote_addr;
        proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

- 저장 후 테스트 & 재시작
sudo nginx -t
sudo systemctl restart nginx

5. 검증
- 프론트 index.html
curl -I http://43.203.79.198/

- 백엔드 API 프록시
curl -I http://43.203.79.198/api/actuator/health

-----------------------------------------------------------------

프론트 서버 (HTTPS 설정)하기

1. Certbot 설치
sudo dnf -y install certbot python3-certbot-nginx

1-1. 도메인 인증 필요

2. 인증서 발급
sudo certbot --nginx -d markcrm25.com -d www.markcrm25.com

3. 응답해야할 것들
1) 본인 이메일 - 긴급 연락 등 받을 이메일
2) 약관 - 동의 필수
3) 뉴스레터 구독 - 동의하지 말것

-----------------------------------------------------------------

1. 백엔드는 프라이빗 서버라서 프론트 들어갔다가 백엔드로 재접속
C:\Users\qkrwm\.ssh 경로에 config 파일 생성

아래 내용 붙여넣기
Host front
    HostName 43.203.79.198
    User ec2-user
    IdentityFile C:/Users/qkrwm/Desktop/blue/file_2/mark-key.pem

Host backend
    HostName 172.31.65.245
    User ec2-user
    IdentityFile C:/Users/qkrwm/Desktop/blue/file_2/mark-key.pem
    ProxyJump front
	
저장 후 PowerShell에서 그냥: ssh backend

- 주의사항
0. rds / redis 인바운드 아웃바운드랑 백엔드 연결 필요
1. 백엔드 인바운드 ssh - 프론트 sg 연결 필요
2. 프론트 아웃바운드 ssh - 백엔드 sg 연결 필요
3. 백엔드에 네트워크 게이트웨이 연결 필요 - vpc 편집에서 만들어주기
4. 레디스의 인바운드에 백엔드 연결 필요
* 배포시 RDS 설정이 필요하다면 RDS의 인바운드와 프론트 서버의 아웃바운드를 연결해야함

-> curl -I https://google.com 응답 확인

1. 기본 패키지 설치
sudo dnf -y update
sudo dnf -y install git

2. 자바 설치
sudo dnf -y install java-17-amazon-corretto-devel

- 버전 확인
java -version
javac -version

3. 소스 가져오기 & 빌드
cd ~
git clone https://github.com/gun-seon/blue_crm.git backend

- 원하는 브랜치 체크아웃
git fetch origin
git checkout release/main-v2

* 어플리케이션 프로퍼티스
cd ~/backend/blue/src/main/resources
nano application.properties
* bluemark-473516-6eabe53c0183.json 파일 생성 필요
([ec2-user@ip-172-31-65-245 blue]$ ls
build  build.gradle  gradle  gradlew  gradlew.bat  keys  settings.gradle  src
[ec2-user@ip-172-31-65-245 blue]$ 경로로!)
mkdir -p ~/backend/keys # cd ~/backend/keys 로 확인
nano ~/backend/key/bluemark-473516-6eabe53c0183.json

cd ~/backend/blue   # ← build.gradle 있는 위치

git config core.filemode false
chmod +x ./gradlew
./gradlew build -x test

4. 실행할 JAR 확인
ls build/libs/

5. systemd 서비스 등록 (자동 실행)
sudo nano /etc/systemd/system/blue.service

- 아래 내용 입력
[Unit]
Description=Blue CRM Backend
After=network.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user/backend/blue
ExecStart=/usr/bin/java -jar /home/ec2-user/backend/blue/build/libs/blue-0.0.1-SNAPSHOT.jar
SuccessExitStatus=143
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target

6. 등록 및 실행
sudo systemctl daemon-reload
sudo systemctl enable blue
sudo systemctl start blue
sudo systemctl status blue

- 로그 계속 보기
journalctl -u blue -f -n 100

7. 헬스 체크
curl -I http://localhost:8080/actuator/health

8. 서버 시간 변경
timedatectl            # 현재 상태 확인
timedatectl list-timezones | grep -i seoul
sudo timedatectl set-timezone Asia/Seoul
timedatectl            # 적용 확인

9. 서비스 재시작
sudo systemctl daemon-reload
sudo systemctl restart blue
sudo systemctl status blue -n 20

- 로그 계속 보기
journalctl -u blue -f -n 100

10. 레디스 관련 설치
sudo dnf -y install valkey

valkey-cli --tls \
  -h master.bluemark-redis-01.1cpuip.apn2.cache.amazonaws.com \
  -p 6582 GET "auth:code:m2@naver.com"

---------------------------------------------

1. DB 파라메타에 서울 시간대 변경 필요


## 🎯 오늘 할 일

- 배포 버전에 대해 버전 업그레이드 진행 필요 (한두개 기능 및 페이지 추가 예정)
- 우선은 대시보드의 데이터 요약을 저장할 갯수 테이블을 생성하여 적용 하는 것부터 끝내자!
- 동문회 연말회식 장소 찾아보기
- 영화 보기 (프랑켄슈타인)

## 💭 한줄평

- 보고 싶어용 다들

## 🔗 공유하고 싶은 것


---

### 🏷️ 태그
