# 📋 데일리 스크럼 게시판 (Jekyll 버전)

Jekyll과 마크다운을 활용한 정적 사이트 생성 기반의 데일리 스크럼 게시판입니다.

## 🚀 특징

- ✅ **Jekyll 기반**: GitHub Pages 자동 빌드 지원
- 📝 **마크다운 작성**: HTML보다 훨씬 쉽고 편리한 스크럼 작성
- 🎯 **팀원별 필터링**: 각 팀원별로 스크럼을 필터링해서 볼 수 있음
- 📱 **반응형 디자인**: 모바일에서도 편리하게 사용 가능
- 🏷️ **메타데이터 관리**: Front Matter로 작성자, 프로젝트, 카테고리 자동 관리
- 🚀 **완전 자동화**: 파일만 추가하면 GitHub Pages가 자동으로 빌드

## 📂 폴더 구조

```
scrum/
├── _config.yml          # Jekyll 설정
├── Gemfile             # Ruby 의존성 관리
├── index.html          # 메인 게시판 (Jekyll 템플릿)
├── template.md         # 마크다운 스크럼 템플릿
├── README.md           # 이 파일
├── MIGRATION.md        # HTML→Jekyll 마이그레이션 가이드
├── _layouts/           # Jekyll 레이아웃
│   ├── default.html    # 기본 레이아웃
│   └── scrum.html      # 스크럼 전용 레이아웃
└── _posts/             # 스크럼 마크다운 파일들
    ├── 2024-01-15-예시-프로젝트-진행상황.md
    ├── 2024-01-16-신민혁-백엔드개발.md
    ├── 2024-01-16-고윤아-디자인작업.md
    └── 2024-01-17-김병곤-취준활동.md
```

## 📝 새 스크럼 작성하기

### 1. 템플릿 복사
`template.md` 파일을 복사해서 시작하세요.

### 2. 파일명 규칙 (중요!)
Jekyll에서는 `_posts` 폴더에 특정 형식으로 파일을 저장해야 합니다:

```
_posts/YYYY-MM-DD-작성자명-제목.md
```

**예시:**
- `_posts/2024-01-15-신민혁-백엔드개발.md`
- `_posts/2024-01-16-고윤아-UI디자인.md`
- `_posts/2024-01-17-김병곤-취준활동.md`

### 3. Front Matter 설정
파일 맨 위에 다음과 같은 메타데이터를 설정하세요:

```yaml
---
layout: scrum
title: "프로젝트 진행상황"
date: 2024-01-15
author: "신민혁"
categories: [scrum]
project: "쇼핑몰 프로젝트"
category: "백엔드"
---
```

### 4. 마크다운 작성
HTML 대신 마크다운으로 편리하게 작성:

```markdown
## 📝 오늘 한 일

- [x] 로그인 API 구현
- [x] JWT 토큰 시스템 구축
- [ ] 테스트 코드 작성

### 상세 내용

오늘은 인증 시스템을 구현했습니다...

```python
def authenticate_user(email, password):
    # 인증 로직
    return token
```

## 🎯 내일 할 일

- [ ] 상품 CRUD API 구현
- [ ] 데이터베이스 최적화
```

### 5. GitHub 업로드
1. `_posts/` 폴더에 마크다운 파일 저장
2. GitHub 리포지토리에 커밋 & 푸시
3. GitHub Pages가 자동으로 Jekyll 빌드 실행
4. 몇 분 후 게시판에 자동 반영!

## 🎯 사용법

### 로컬 개발 환경 설정

```bash
# Ruby와 Jekyll 설치 (macOS)
brew install ruby
gem install jekyll bundler

# 프로젝트 의존성 설치
cd scrum/
bundle install

# 로컬 서버 실행
bundle exec jekyll serve

# http://localhost:4000 접속
```

### GitHub Pages 환경
1. 리포지토리를 GitHub Pages로 배포
2. `https://username.github.io/repo-name/scrum/` 접속
3. 마크다운 파일이 자동으로 HTML로 변환되어 표시

## 🛠️ 기술 스택

- **Jekyll**: 정적 사이트 생성기
- **Liquid**: Jekyll 템플릿 엔진
- **Kramdown**: 마크다운 프로세서
- **HTML5 + CSS3**: 반응형 UI
- **JavaScript**: 클라이언트사이드 필터링
- **GitHub Pages**: 자동 빌드 및 호스팅

## 📋 스크럼 템플릿 구조

각 스크럼은 다음 섹션들로 구성됩니다:

1. **📝 오늘 한 일** (체크리스트 + 상세 내용)
2. **🎯 내일 할 일** (계획)
3. **🚧 블로커/이슈** (어려운 점)
4. **💡 배운 점** (새로 습득한 지식)
5. **📊 진행률** (프로젝트 진행 상황)

## 🔧 커스터마이징

### 팀원 추가/변경
`_config.yml`의 `team_members` 섹션을 수정하세요:

```yaml
team_members:
  - name: "신민혁"
    folder: "신민혁"
  - name: "새팀원"
    folder: "새팀원"
```

### 레이아웃 변경
- `_layouts/default.html`: 전체 페이지 레이아웃
- `_layouts/scrum.html`: 스크럼 포스트 레이아웃
- `index.html`: 메인 게시판 페이지

## 🎨 Jekyll vs 기존 HTML 방식 비교

| 기능 | 기존 HTML | Jekyll 마크다운 |
|------|-----------|----------------|
| 작성 방식 | HTML 태그 | 마크다운 문법 |
| 파일 관리 | 개인별 폴더 | `_posts/` 통합 |
| 메타데이터 | HTML 내부 | Front Matter |
| 자동화 | GitHub Actions | GitHub Pages |
| 빌드 시간 | 즉시 | 1-2분 |
| 편의성 | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

## 🐛 문제 해결

### 스크럼이 안 보일 때
1. 파일명이 `YYYY-MM-DD-작성자명-제목.md` 형식인지 확인
2. `_posts/` 폴더 루트에 있는지 확인 (하위 폴더 X)
3. Front Matter가 올바른지 확인
4. `categories: [scrum]`이 포함되어 있는지 확인
5. `author` 필드가 본인 이름과 일치하는지 확인
6. `date` 필드와 파일명의 날짜가 일치하는지 확인
7. GitHub Pages 빌드 상태 확인

### GitHub Pages 빌드 실패
- Repository Settings > Pages에서 빌드 로그 확인
- Jekyll 문법 오류가 있는지 확인
- `_config.yml` 설정 확인

### 로컬에서 Jekyll 오류
```bash
# 의존성 업데이트
bundle update

# 캐시 삭제 후 재빌드
bundle exec jekyll clean
bundle exec jekyll serve
```

## 📞 지원

문제가 있거나 개선 사항이 있으면 이슈를 생성해주세요!

## 🎯 다음 업데이트 계획

- [ ] 댓글 시스템 (utterances)
- [ ] 검색 기능
- [ ] RSS 피드
- [ ] 태그 시스템
- [ ] 통계 대시보드

---

⭐ **Happy Jekyll Scrumming!** 📈 