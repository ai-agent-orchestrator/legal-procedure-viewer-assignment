# Legal Procedure Viewer Assignment

React and Spring Boot integration assignment customized for a legal-procedure service.

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
