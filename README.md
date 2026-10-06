# Legal Procedure Viewer Assignment

React and Spring Boot integration assignment customized for a legal-procedure service.

## 과제 간단 설명

Spring Boot REST API와 React를 연동하여 법률 절차 정보를 조회하는 웹 애플리케이션을 구현한 과제입니다. 기존 메뉴 관리 예제의 기술 구조를 법률 도메인으로 바꾸어, 법률 절차 목록·검색·상세·체크리스트 화면을 구성했습니다.

이 과제에서 확인하는 핵심 흐름은 다음과 같습니다.

```text
React 화면
→ API 함수 분리
→ fetch로 JSON 요청
→ Spring Boot Controller
→ Service·Repository·DB
→ JSON 응답
→ React 화면 갱신
```

또한 로딩·오류·빈 결과 상태를 처리하고, `api-docs.json`, Postman 컬렉션, 디자인 토큰 파일을 함께 제공하여 API 명세·실행 검증·공통 디자인 기준을 확인할 수 있도록 했습니다. 법률 조회 API는 공개하고, AI API는 JWT 인증을 요구하도록 보안 흐름도 문서화했습니다.

## Project goal

This repository demonstrates the assignment's main technical flow:

```text
React screen
-> src/api API helper
-> fetch JSON request
-> Spring REST Controller
-> Service
-> Repository / database
-> JSON response
-> React state update
-> screen rendering
```

The sample shopping-mall domain was replaced with legal procedure data so the result connects to the legal AI project. The reusable learning target is still the same: a React client calls a Spring Boot REST API and renders the response with shared UI states.

## Structure

```text
frontend/legal-procedure-viewer
-> React + Vite frontend

backend/guardrail-ready-policy-checker
-> Spring Boot REST API
```

## Implemented frontend screens

- Legal procedure list
- Domain filter and keyword search
- Procedure detail and steps
- Criminal supplementary-investigation checklist
- Loading, error, and empty-result states

## API mapping

```text
GET /api/legal/procedures
GET /api/legal/procedures/{code}
GET /api/legal/procedures/{code}/steps
GET /api/legal/criminal/checklist/supplementary-investigation
```

The frontend request functions are separated in:

```text
frontend/legal-procedure-viewer/src/api/legalProcedureApi.js
```

The API contract used by the frontend is also recorded in:

```text
api-docs.json
```

Postman requests for the same contract are stored in:

```text
postman/legal-procedure-viewer.postman_collection.json
```

The collection checks login, public legal reads, procedure detail, checklist retrieval, an expected `401` without JWT, and an authenticated AI request. The final AI request requires a configured LLM provider key only when the external model call is enabled.

## Design tokens

The legal viewer uses a small domain-specific token set for colors, spacing, and control radii:

```text
frontend/legal-procedure-viewer/design/legal.tokens.json
-> npm run tokens
-> frontend/legal-procedure-viewer/src/tokens.css
-> shared React styles
```

The tokens are a collaboration convention for keeping screens visually consistent. They are not a backend security or API standard.

## Run locally

### Backend

```powershell
cd backend/guardrail-ready-policy-checker
.\gradlew.bat bootRun
```

The backend runs on `http://localhost:8080`.

The assignment backend uses an in-memory H2 database and seeds legal procedure data at startup, so a separate MySQL installation is not required for this standalone copy.

### Frontend

Open another terminal:

```powershell
cd frontend/legal-procedure-viewer
npm install
npm run dev
```

Open the Vite URL shown in the terminal.

## Security study point

The backend currently uses this access policy:

```text
POST /api/auth/login
-> public, used to receive a JWT

GET /api/legal/**
-> public read access for the assignment viewer

/api/ai/**
-> JWT authentication required
```

Detailed login, JWT filter, `Bearer` header, 401/403 handling, and Postman study steps are documented in:

```text
frontend/legal-procedure-viewer/README.md
```

## Verification

```powershell
cd frontend/legal-procedure-viewer
npm run build
```

The frontend production build passes. The Spring Boot backend tests also pass in the source project.
