# Command: /review-pr

## 목적

GitHub PR을 리뷰하고 리뷰 코멘트를 GitHub에 등록한다.
`gh` CLI를 사용하여 PR diff를 읽고, 아키텍처 규칙 기반으로 리뷰한 뒤,
결과를 GitHub PR Review로 직접 등록한다.

---

## 사용 방법

```
/review-pr <PR 번호>
```

예시

```
/review-pr 13
```

리포를 지정할 수도 있다.

```
/review-pr 3 --repo alpha-desk-frontend
```

---

## 동작 규칙

1. `gh pr view <번호>` 로 PR 메타 정보(제목, 작성자, 브랜치, body)를 조회한다
2. `gh pr diff <번호>` 로 전체 diff를 가져온다
3. 아래 체크리스트 기준으로 코드를 리뷰한다
4. 리뷰 결과를 GitHub에 등록한다

---

## 리뷰 체크리스트

### 아키텍처 (CLAUDE.md 기반)

- Domain Layer가 순수 Python인가 (FastAPI, SQLAlchemy, Redis import 없는가)
- ORM Model과 Domain Entity가 분리되어 있는가
- Request/Response DTO가 Domain Entity와 분리되어 있는가
- Router에 비즈니스 로직이 없는가
- 의존성 방향이 올바른가 (Adapter → Application → Domain)

### 코드 품질

- 네이밍이 명확한가
- 중복 코드가 없는가
- 에러 처리가 적절한가
- 보안 취약점이 없는가

### 프로젝트 규칙

- 브랜치 전략을 따르는가 (`feature/{role}-{기능명}`)
- 커밋 메시지가 적절한가
- 백엔드-프론트엔드 간 API 필드명이 일치하는가

---

## 리뷰 등록 방법

리뷰 결과를 `gh pr review <번호>` 명령으로 GitHub에 등록한다.

심각한 문제가 있으면:
```
gh pr review <번호> --request-changes --body "<리뷰 내용>"
```

문제가 없으면:
```
gh pr review <번호> --approve --body "<리뷰 내용>"
```

경미한 지적만 있으면:
```
gh pr review <번호> --comment --body "<리뷰 내용>"
```

---

## 리뷰 본문 형식

리뷰 본문은 다음 형식을 따른다.

```
## 아키텍처 체크
- [x] Domain Layer 순수 Python
- [x] ORM-Entity 분리
- ...

## 잘된 점
- ...

## 지적 사항
### 1. [중요도: 높/중/낮] 제목
파일: `경로:라인`
설명

### 2. ...

## 결론
Approve / Request Changes / Comment 사유
```

---

## 주의사항

- 리뷰 등록 전 반드시 사용자에게 리뷰 내용을 보여주고 확인을 받는다
- `--repo` 옵션이 주어지면 해당 리포에서 PR을 조회한다
- 프론트엔드 PR인 경우 아키텍처 체크리스트를 프론트엔드 기준으로 조정한다

$ARGUMENTS
