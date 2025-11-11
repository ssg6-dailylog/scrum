---
layout: scrum
title: "EC2에서 git push 하기"
date: 2025-11-12
author: "박건희"
categories: [scrum]
---

## 📝 어제 한 일

- 외주) 핫픽스-J열을 카테고리로 새로 입력받기

### 상세 내용

## EC2에서 Git Push 완전 세팅 가이드 (2025)

### 1️⃣ SSH 키 생성 및 등록

**💡 목적**
EC2가 GitHub에 “신뢰된 사용자”로 인증되도록 SSH 키를 등록한다.
(2021년 이후 패스워드로는 push 불가)

**🔧 SSH 키 생성**

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

* `~/.ssh` 폴더에 2개 파일 생성됨

  * 비밀키: `id_ed25519`
  * 공개키: `id_ed25519.pub`

**🔑 공개키 등록**

1. GitHub → 프로필 아이콘 클릭 → **Settings → SSH and GPG keys**
2. **New SSH key** 클릭
3. `cat ~/.ssh/id_ed25519.pub` 결과 전체 복사
4. Title 입력 후 **Add SSH key**

---

### 2️⃣ 연결 테스트 및 SSH 설정

**✅ 연결 테스트**

```bash
ssh -T git@github.com
```

정상 응답:

```
Hi psns0122! You've successfully authenticated, but GitHub does not provide shell access.
```

**⚠️ 실패 시 SSH Config 수정**

```bash
nano ~/.ssh/config
```

아래 내용 추가:

```
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
```

저장 후 다시 테스트:

```bash
ssh -T git@github.com
```

---

### 3️⃣ 원격 저장소(레포) 연결 방식 수정

**❌ 잘못된 예시 (기존 HTTPS 방식)**

```
origin  https://github.com/blue-CRM/blue_crm_v2.git (fetch)
origin  https://github.com/blue-CRM/blue_crm_v2.git (push)
```

→ GitHub은 비밀번호 인증 차단 → push 실패

**✅ 수정 (SSH 방식)**

```bash
git remote set-url origin git@github.com:blue-CRM/blue_crm_v2.git
```

확인:

```bash
git remote -v
```

결과:

```
origin  git@github.com:blue-CRM/blue_crm_v2.git (fetch)
origin  git@github.com:blue-CRM/blue_crm_v2.git (push)
```

---

### 4️⃣ 권한 문제 해결 (Write 권한 부여)

**📍 원인**
조직 레포(`blue-CRM/blue_crm_v2`)의 Base role 이 `Read`로 되어 있으면
SSH 인증이 되어도 push 거부됨.

**✅ 해결**

1. GitHub → 레포 → **Settings → Collaborators and teams**
2. “Base role” → `Write` 로 변경
   (또는 본인을 **Add people → psns0122 → Write** 로 추가)

---

### 5️⃣ 최종 Push

```bash
git add .
git commit -m "chore: test push"
git push origin release
```

정상 출력 예시:

```
Enumerating objects: 5, done.
To github.com:blue-CRM/blue_crm_v2.git
   0760ad2..94f9ac0  release -> release
```


## 🎯 오늘 할 일

- 외주) 권한 확장 (ㄹㅇ 이제 미룰 수 없다 다음주 미팅 잡힘 ㄷㄷ)

## 💭 한줄평

## 🔗 공유하고 싶은 것

---

### 🏷️ 태그
