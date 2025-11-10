---
layout: scrum
title: "HTML autocomplete 속성"
date: 2025-11-10
author: "박건희"
categories: [scrum]
---

## 📝 어제 한 일

- 독서) 군주론 읽는 중
- 외주) 1차 개발 레포랑 2차 개발 레포 구분 + 리드미 작성 (https://github.com/orgs/blue-CRM/repositories)
- 외주) autocomplete 속성 적용

### 상세 내용

1. 독서
	- 사람들의 본성은 자신이 받는 혜택과 마찬가지로 자신이 베푸는 혜택에 의해서도 서로 얽매이게 된다.
2. 외주

## 🧩 HTML `autocomplete` 속성 정리

### 📝 1. 개요

* `autocomplete`은 브라우저가 **입력값을 자동완성하거나 비밀번호 저장/입력 제안**을 할 때 참고하는 **표준 힌트 속성**이다.
* 단순히 자동입력 기능을 켜거나 끄는 게 아니라, **입력 필드의 의미(역할)** 를 명확히 알려주는 메타데이터다.
* HTML 명세(WHATWG / W3C)에서 정해진 값만 브라우저가 인식한다.

---

### ⚙️ 2. 주요 특징

| 항목                              | 설명                                                                          |
| ------------------------------- | --------------------------------------------------------------------------- |
| 브라우저 자동완성 제어용 속성                | `autocomplete`                                                              |
| 값은 **표준 키워드**로 한정               | 임의 문자열 사용 불가                                                                |
| 브라우저는 이 값을 기반으로 자동완성 데이터 분류     | 예: 로그인 계정 세트, 주소, 결제정보 등                                                    |
| 잘못된 값 사용 시 → 자동완성 비활성화 또는 경고 표시 | DevTools에서 `[DOM] Input elements should have autocomplete attributes` 경고 발생 |

---

### 🔑 3. 대표적 표준값

#### 🟦 로그인 / 인증 관련

| 용도             | 권장 값               | 설명                       |
| -------------- | ------------------ | ------------------------ |
| 로그인 아이디 또는 이메일 | `username`         | 로그인 시 계정 식별용             |
| 로그인 비밀번호       | `current-password` | 저장된 계정 비밀번호 자동완성         |
| 회원가입 이메일       | `email`            | 새 계정 등록용 이메일             |
| 새 비밀번호 입력      | `new-password`     | 회원가입/비밀번호 변경 시 사용        |
| 비밀번호 확인        | `new-password`     | 동일하게 지정 (브라우저가 한 세트로 인식) |

#### 🟩 개인 정보

| 용도   | 권장 값                                          |
| ---- | --------------------------------------------- |
| 이름   | `name`, `given-name`, `family-name`           |
| 전화번호 | `tel`, `tel-national`, `tel-country-code`     |
| 생년월일 | `bday`, `bday-day`, `bday-month`, `bday-year` |

#### 🟨 주소 및 결제 정보

| 용도   | 권장 값                                                         |
| ---- | ------------------------------------------------------------ |
| 주소   | `street-address`, `address-level1`, `postal-code`, `country` |
| 카드정보 | `cc-name`, `cc-number`, `cc-exp`, `cc-csc`                   |

---

### 🧠 4. 실제 사용 예시

#### 🔹 로그인 폼

```html
<input type="email" autocomplete="username" placeholder="이메일" />
<input type="password" autocomplete="current-password" placeholder="비밀번호" />
```

#### 🔹 회원가입 폼

```html
<input type="email" autocomplete="email" placeholder="이메일" />
<input type="password" autocomplete="new-password" placeholder="비밀번호" />
<input type="password" autocomplete="new-password" placeholder="비밀번호 확인" />
<input type="tel" autocomplete="tel-national" placeholder="010-1234-5678" />
```

---

### 🔒 5. 보안적 특징

* `new-password`는 **자동입력되지 않음** (보안상 제한)
* 로그인 필드(`username`, `current-password`)만 저장된 계정일 경우 자동입력 허용
* 브라우저는 `username`과 `current-password`를 **하나의 로그인 세트로 인식**하여 “비밀번호 저장” 제안을 함

---

### 🧩 6. 정리 요약

| 폼 종류 | 필드      | `autocomplete` 값   |
| ---- | ------- | ------------------ |
| 로그인  | 이메일     | `username`         |
| 로그인  | 비밀번호    | `current-password` |
| 회원가입 | 이메일     | `email`            |
| 회원가입 | 비밀번호    | `new-password`     |
| 회원가입 | 비밀번호 확인 | `new-password`     |
| 회원가입 | 전화번호    | `tel-national`     |

---

### 🚀 7. 요약

* `autocomplete`는 단순 편의기능이 아니라 **웹 표준 기반의 UX·보안 기능**
* 브라우저 경고(`[DOM] Input elements should have autocomplete attributes`)는 무시해선 안 됨
* 올바른 값을 지정하면 다음과 같은 효과가 있음:

  * 🔒 보안성 향상
  * ⚡ 자동완성 품질 개선
  * ♿ 접근성(Accessibility) 점수 향상

## 🎯 오늘 할 일

- 외주) 권한 확장
- 위키드 팝업 놀러가기

## 💭 한줄평

- 항상 로그인 자동완성해주는 사이트 궁금했었는데 이제 나도 할 수 있다!

## 🔗 공유하고 싶은 것

---

### 🏷️ 태그
