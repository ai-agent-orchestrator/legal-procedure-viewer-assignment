# 법률 절차 조회 서비스 과제

Spring Boot REST API와 React를 연동하여 법률 절차 정보를 조회하는 웹 애플리케이션입니다. 기존 메뉴 관리 예제의 기술 구조를 법률 도메인에 맞게 변경했습니다.

## 주요 기능

- 법률 절차 목록 조회
- 분야별 필터와 검색
- 법률 절차 상세 및 단계 조회
- 형사 보완수사 체크리스트 조회
- 로딩·오류·검색 결과 없음 상태 처리
- JWT 로그인과 API 접근 권한 문서화

## 핵심 연동 흐름

```text
React 화면
→ API 함수 분리
→ fetch JSON 요청
→ Spring Boot Controller
→ Service
→ Repository·DB
→ JSON 응답
→ React 화면 갱신
```

## 폴더 구성

```text
frontend/legal-procedure-viewer
→ React 프론트엔드

backend/guardrail-ready-policy-checker
→ Spring Boot 백엔드
```

## API 목록

```text
GET /api/legal/procedures
GET /api/legal/procedures/{code}
GET /api/legal/procedures/{code}/steps
GET /api/legal/criminal/checklist/supplementary-investigation
POST /api/auth/login
POST /api/ai/chat
```

React의 API 요청 함수는 다음 파일에 분리했습니다.

```text
frontend/legal-procedure-viewer/src/api/legalProcedureApi.js
```

API 명세와 Postman 검증 자료도 함께 제공합니다.

```text
api-docs.json
postman/legal-procedure-viewer.postman_collection.json
```

## 디자인 토큰

색상·간격·모서리 규칙을 디자인 토큰으로 관리했습니다.

```text
frontend/legal-procedure-viewer/design/legal.tokens.json
→ npm run tokens
→ frontend/legal-procedure-viewer/src/tokens.css
```

디자인 토큰은 여러 화면에 동일한 스타일을 적용하기 위한 협업 기준입니다.

## 실행 방법

### 백엔드 실행

```powershell
cd backend/guardrail-ready-policy-checker
.\gradlew.bat bootRun
```

백엔드는 `http://localhost:8080`에서 실행됩니다. 인메모리 H2 데이터베이스를 사용하므로 별도의 MySQL 설치가 필요하지 않습니다.

### 프론트엔드 실행

새 터미널에서 실행합니다.

```powershell
cd frontend/legal-procedure-viewer
npm install
npm run tokens
npm run dev
```

터미널에 표시되는 Vite 주소를 브라우저에서 엽니다.

## 보안 정책

```text
POST /api/auth/login
→ JWT 발급을 위해 공개

GET /api/legal/**
→ 법률 조회 화면을 위해 공개

/api/ai/**
→ JWT 인증 필요
```

JWT 로그인·Bearer 토큰·401/403 오류·Postman 실습 방법은 다음 문서에 정리했습니다.

```text
frontend/legal-procedure-viewer/README.md
```

## 검증

```powershell
cd frontend/legal-procedure-viewer
npm run build
```

프론트엔드 빌드와 백엔드 테스트를 완료했습니다.
