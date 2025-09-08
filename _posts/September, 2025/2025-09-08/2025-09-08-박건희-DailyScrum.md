---
layout: scrum
title: "DailyScrum"
date: 2025-09-07
author: "박건희"
categories: [scrum]
---

## 📝 어제 한 일

- 외주) 프로젝트 구조 설계

### 상세 내용

1. DB 계정 생성<br>
```
CREATE DATABASE aaa;
CREATE USER 'aaa'@'localhost' IDENTIFIED BY 'aaa';
GRANT ALL PRIVILEGES ON aaa.* TO 'aaa'@'localhost';
FLUSH PRIVILEGES;
```

2. 프론트 vue 프로젝트 생성
```
npm create vue@^3.10.3
 - Add Vue Router
 - Add Pinia
```

3. 프록시 설정 및 ping 테스트
```
@RestController
@RequestMapping("/api")
public class PingController {
    @GetMapping("/ping")
    public Map<String, Object> ping() {
        return Map.of(
            "pong", true,
            "time", LocalDateTime.now().toString()
        );
    }
}
```
```
server: {
  port: 5173,
  proxy: {
    '/api': {
      target: 'http://localhost:8080',
      changeOrigin: true
    }
  }
}
```
```
<script setup>
import { ref, onMounted } from 'vue'

const pong = ref('loading...')

onMounted(async () => {
  try {
    const res = await fetch('/api/ping') // 프록시 → 백엔드
    const data = await res.json()
    pong.value = JSON.stringify(data)
  } catch (e) {
    pong.value = 'error: ' + e
  }
})
</script>

<template>
  <main style="padding: 24px">
    <h1>프론트 ↔ 백 연결 테스트</h1>
    <p>/api/ping → {{ pong }}</p>
  </main>
</template>
```

4. 권한 구분
```
 - 본사 → SUPERADMIN
 - 센터장 → MANAGER
 - 직원 → STAFF
```

5. JWT 개념 정리
```
1. 로그인 시

* 서버 응답
JSON: Access Token → Pinia에 저장
Set-Cookie: Refresh Token → 브라우저 쿠키(HttpOnly, Secure)

2. 로그인 상태에서 일반 요청

* 프론트 (Vue + Axios)
Pinia에서 Access Token 꺼냄
Authorization: Bearer <AccessToken> 헤더에 붙여서 API 호출

* 서버 (JwtAuthenticationFilter)
헤더 속 Access Token 검증
유효하면 그대로 통과 → 정상 응답

3. Access Token 만료된 경우

* 프론트 (Axios 인터셉터)
401 Unauthorized 응답 감지
/api/auth/refresh 요청 보냄 (이때는 헤더에 아무것도 안 붙임)

* 브라우저
자동으로 Refresh Token 쿠키를 서버에 전송

* 서버
쿠키에서 Refresh Token 꺼내 검증
새 Access Token 발급 → JSON 응답

* 프론트
받은 Access Token을 Pinia에 갱신
원래 하려던 요청 다시 시도
```

## 🎯 오늘 할 일

- 외주) 로그인/회원가입 구현하기

## 💭 한줄평

- 아자아자!

## 🔗 공유하고 싶은 것


---

### 🏷️ 태그
