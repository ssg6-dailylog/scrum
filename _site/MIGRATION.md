# 🔄 HTML → Jekyll 마이그레이션 가이드

기존 HTML 기반 스크럼 시스템을 Jekyll + 마크다운 방식으로 변경하는 가이드입니다.

## 📋 변경 사항 요약

| 항목 | 기존 방식 | 새로운 방식 |
|------|-----------|-------------|
| 파일 형식 | `.html` | `.md` (마크다운) |
| 파일 위치 | `개인폴더/파일명.html` | `_posts/YYYY-MM-DD-제목.md` |
| 메타데이터 | HTML 내부 | Front Matter |
| 템플릿 | `template.html` | `template.md` |
| 빌드 방식 | GitHub Actions | GitHub Pages 자동 빌드 |

## 🚀 마이그레이션 단계

### 1단계: 새로운 파일 구조 이해
```
기존:
개인폴더/2024-01-15-제목.html

신규:
_posts/2024-01-15-제목.md
```

### 2단계: 기존 HTML 파일을 마크다운으로 변환

#### HTML 예시:
```html
<div class="scrum-header">
    <h1>🗓️ 2024년 01월 15일 (월)</h1>
    <div class="author">작성자: 신민혁</div>
</div>

<div class="section">
    <h2>📝 오늘 한 일</h2>
    <ul>
        <li class="completed">사용자 로그인 기능 구현</li>
        <li class="completed">JWT 토큰 시스템 구축</li>
    </ul>
</div>
```

#### 마크다운 변환:
```markdown
---
layout: scrum
title: "프로젝트 진행상황"
date: 2024-01-15
author: "신민혁"
categories: [scrum]
project: "쇼핑몰 프로젝트"
category: "백엔드"
---

## 📝 오늘 한 일

- [x] 사용자 로그인 기능 구현
- [x] JWT 토큰 시스템 구축
```

### 3단계: 자동 변환 스크립트 (선택사항)

#### 간단한 Python 스크립트로 변환 도우미:
```python
import os
import re
from datetime import datetime

def convert_html_to_md(html_file_path):
    """HTML 스크럼 파일을 마크다운으로 변환"""
    
    with open(html_file_path, 'r', encoding='utf-8') as f:
        content = f.read()
    
    # 기본 정보 추출
    title_match = re.search(r'<h1[^>]*>.*?(\d{4}년\s*\d{1,2}월\s*\d{1,2}일).*?</h1>', content)
    author_match = re.search(r'작성자[:：]\s*([^<\n]+)', content)
    
    if title_match:
        date_str = title_match.group(1)
        # 날짜 파싱 및 변환 로직 추가
    
    # Front Matter 생성
    front_matter = f"""---
layout: scrum
title: "프로젝트 진행상황"
date: {date_str}
author: "{author_match.group(1) if author_match else '작성자'}"
categories: [scrum]
project: "프로젝트명"
category: "분야"
---

"""
    
    # HTML을 마크다운으로 변환하는 로직 추가
    # (pandoc 사용 권장)
    
    return front_matter + converted_content

# 사용 예시
# convert_html_to_md('신민혁/2024-01-15-example.html')
```

### 4단계: 새로운 워크플로우 적용

1. **기존 파일 백업**
   ```bash
   cp -r 신민혁/ backup/신민혁/
   cp -r 고윤아/ backup/고윤아/
   # ... 다른 폴더들도 백업
   ```

2. **마크다운 파일 생성**
   - `template.md` 사용해서 새로운 스크럼 작성
   - `_posts/` 폴더에 저장

3. **GitHub Pages 설정**
   - Repository Settings > Pages
   - Source: Deploy from a branch
   - Branch: main (또는 master)
   - Folder: `/scrum`

## 🎯 변환 체크리스트

### ✅ 파일 구조
- [ ] `_config.yml` 생성됨
- [ ] `Gemfile` 생성됨
- [ ] `_layouts/` 폴더와 레이아웃 파일들 생성됨
- [ ] `_posts/` 폴더 생성됨
- [ ] `template.md` 생성됨

### ✅ 콘텐츠 마이그레이션
- [ ] 기존 HTML 파일들을 마크다운으로 변환
- [ ] Front Matter 메타데이터 추가
- [ ] 파일명을 Jekyll 규칙에 맞게 변경
- [ ] `_posts/` 폴더로 이동

### ✅ 설정 확인
- [ ] `_config.yml`에서 팀원 목록 업데이트
- [ ] GitHub Pages 빌드 성공 확인
- [ ] 웹사이트에서 스크럼 목록 정상 표시 확인
- [ ] 필터링 기능 동작 확인

## 🔧 주의사항

### 날짜 형식
```yaml
# 올바른 형식
date: 2024-01-15

# 잘못된 형식
date: "2024년 1월 15일"
date: 2024-1-15
```

### 카테고리 설정
```yaml
# 반드시 포함해야 함
categories: [scrum]

# 선택적
project: "프로젝트명"
category: "백엔드"
```

### 파일명 규칙
```
✅ 올바른 예시:
_posts/2024-01-15-백엔드개발.md
_posts/2024-12-31-연말정리.md

❌ 잘못된 예시:
_posts/백엔드개발.md
_posts/2024-1-15-개발.md
2024-01-15-개발.md (폴더 위치 틀림)
```

## 🐛 자주 발생하는 문제

### 1. Jekyll 빌드 실패
**증상**: GitHub Pages에서 빌드 실패 메일 수신

**해결책**:
- Front Matter 문법 확인
- 날짜 형식 확인
- 파일명 규칙 확인
- `_config.yml` 문법 확인

### 2. 스크럼이 목록에 안 나타남
**증상**: 파일은 있는데 게시판에 안 보임

**해결책**:
- `categories: [scrum]` 포함 확인
- `_posts/` 폴더 위치 확인
- 파일명 날짜 형식 확인

### 3. 한글 깨짐
**증상**: 한글이 제대로 표시되지 않음

**해결책**:
- 파일을 UTF-8 인코딩으로 저장
- `_config.yml`에 `encoding: utf-8` 추가

## 💡 팁과 권장사항

### 점진적 마이그레이션
모든 파일을 한번에 변환하지 말고, 새로운 스크럼부터 마크다운으로 작성하는 것을 권장합니다.

### 마크다운 에디터 활용
- **VS Code**: Jekyll 플러그인 설치
- **Typora**: WYSIWYG 마크다운 에디터
- **Mark Text**: 무료 마크다운 에디터

### 로컬 개발 환경
Jekyll을 로컬에 설치해서 변경사항을 미리 확인하는 것을 권장합니다.

```bash
# Jekyll 로컬 설치 및 실행
gem install jekyll bundler
cd scrum/
bundle install
bundle exec jekyll serve
```

## 📞 마이그레이션 지원

마이그레이션 과정에서 문제가 생기면:
1. 이 가이드를 다시 확인
2. Jekyll 공식 문서 참조
3. GitHub Pages 빌드 로그 확인
4. 이슈 생성해서 도움 요청

---

🎉 **성공적인 마이그레이션을 응원합니다!** 🚀 