---
layout: scrum
title: "백엔드 - 로그인/회원가입"
date: 2025-09-09
author: "박건희"
categories: [scrum]
---

## 📝 어제 한 일

- 외주) 로그인/회원가입 구현

### 상세 내용

1. 로그인 구현<br>

가. 로그인시 동작
- 세션쿠키(브라우저가 닫히면 사라지는 쿠키)로 리프레시 토큰 발급
- 엑세스 토큰 피아나로 발급
- 토큰의 유지시간과 쿠키의 유지시간은 다르다는 점을 이용하여 브라우저가 닫히면 즉시 로그아웃 되고, 브라우저가 닫히지 않았어도 토큰의 유지시간이 만료되면 로그아웃 처리 됨

나. 엑세스 토큰 갱신
- /api/token/* 으로 서버가 호출되는 경우 브라우저는 쿠키를 포함해서 리퀘스트를 날림
- 쿠키에 담겨있는 리프레시 토큰으로 엑세스 토큰 갱신

다. 리프레시 토큰의 수동 갱신
- CRM 프로젝트 특성상 보안에 중점을 두고, 리프레시 토큰을 수동 갱신하도록 함 (은행처럼 로그인 연장 시스템)
- 사용자가 "로그인연장" 버튼을 클릭하면 리프레시 토큰과 엑세스 토큰을 갱신

라. 라우터 가드 설정
- 권한에 따라 접근가능한 url 제한
- 권한이 없는 링크로 타이핑하여 접근하려고 하면 home으로 강제 이동

마. 로딩화면 설정
- 비동기 라우팅의 문제점으로, 권한에 대한 판단이 수정되기 전에 기존 권한에 대한 정보를 표시하는 문제를 비어있는 로딩화면 삽입으로 해결

바. 결론
- CRM 프로젝트임을 감안하여 최대한 보안을 신경써서 JWT, 라우터를 설계함
- 만약 동시 로그인(모바일/pc)을 막고 싶다면 레디스를 사용해서 최신 리프레시 토큰이 무엇인지 관리해야함 

2. 로그아웃 구현

3. 회원가입 구현
- java 메일센더 사용해서 이메일 인증하도록 구현
- 인증번호를 Redis를 통해 보관 (세션을 사용하지 않는 이유 : 로드밸런서로 다서버 환경 구축을 염두에 두기 위해 - Redis는 별도의 중앙 저장소 역할을 하므로, 서버 A/B 어디서 요청이 들어와도 동일한 키(auth:code:email)를 조회할 수 있음)
- 윈도우에서는 레디스 사용이 어려운 문제점 발생 -> 윈도우에도 우분투 깔아서 개발진행
```
- 1. WSL 설치 및 설정
wsl --install                            # WSL 설치 (Ubuntu 기본 설치)
wsl --update                             # WSL 업데이트 (필요한 경우)
wsl --set-default-version 2              # WSL 2로 기본 설정

- 1-1. 설치가 이미 되어있는 경우
wsl --list --verbose
wsl -d Ubuntu

- 2. Ubuntu 실행 및 업데이트
sudo apt update && sudo apt upgrade -y   # 패키지 업데이트 및 업그레이드

- 3. Redis 설치
sudo apt install redis -y                # Redis 설치

- 4. Redis 실행
sudo service redis-server start          # Redis 서버 시작

- 5. Redis 동작 확인
redis-cli ping                           # 결과: PONG
```
```
- 배포시 application.properties 설정
spring.data.redis.host=내부 IP나 호스트네임 (혹은 AWS ElastiCache라면 엔드포인트 주소 사용)
spring.data.redis.port=6379
spring.data.redis.timeout=6000

- 배포시 AWS ElastiCache 사용하지 않을 경우 우분투 설정
- 1. 패키지 업데이트
sudo apt update && sudo apt upgrade -y

- 2. Redis 설치
sudo apt install redis -y

- 3. Redis 실행
sudo systemctl enable redis-server
sudo systemctl start redis-server

- 4. 보안 설정 (bind 0.0.0.0 허용 + 비밀번호 설정)
sudo nano /etc/redis/redis.conf
-> bind 0.0.0.0
-> requirepass yourStrongPassword

- 5. 방화벽/보안그룹에서 포트 6379 열기 (내부망만 허용 권장)
```

4. 인증코드 관련 처리
- 새로고침시 과거 인증코드는 무효처리 완료
- 시간이 오래 걸리는 메일 발송은 비동기처리 하여 UX 향상 (인증코드 유효시간 정보 먼저 프론트로 발송)

## 🎯 오늘 할 일

- 외주) 프론트 화면 만들기

## 💭 한줄평

- 아자아자!

## 🔗 공유하고 싶은 것


---

### 🏷️ 태그
