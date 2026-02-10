# LLM + MCP Agent — 프로젝트 B 설계 문서

> Terminal Agent(C)의 선행 프로젝트 — 안전한 도메인에서 LLM·MCP·Tool 설계를 독립 프로젝트로 완수

---

## 문서 규칙

- **진실기준**: 이 레포 `docs/` (개발 중)
- **동기화**: The Record `2_Projects/LLM_MCP_Agent/` 에 기회될 때마다 갱신
- **완료 후**: Obsidian + 레포 둘 다 동일 내용 유지

---

## 1. 프로젝트 정체성

### 1.1 한 줄 요약

**안전한 도메인(파일 읽기, 검색, 문서 조회)에서 MCP 서버 + LLM Agent를 구현. 터미널 실행 없이 LLM·Tool·MCP에 익숙해진다.**

### 1.2 Terminal Agent와의 관계

- **이 프로젝트(B)** = LLM·MCP·Tool 설계에 익숙해지기 위한 **독립 완결 프로젝트**
- **금지**: 쉘 실행, `subprocess`로 임의 명령 실행
- **완수 후**: Terminal Agent(C)에서 Agent 레이어 설계 경험을 활용

### 1.3 안전한 도메인 원칙

| 허용 | 금지 |
|------|------|
| 파일 읽기 (read_file) | 터미널/쉘 명령 실행 |
| 디렉터리 목록 (list_dir) | subprocess, os.system |
| 텍스트 검색 (search) | eval, exec |
| 문서/노트 조회 | 임의 네트워크 요청 |
| LLM 추론 + Tool Calling | |

---

## 2. 목표 & 완수 기준

### 2.1 완수 기준 (Terminal Agent 시도 가능 조건)

| 항목 | 완수 산출물 |
|------|-------------|
| MCP 서버 | Tool을 MCP로 노출, Cursor 등에서 호출 가능 |
| LLM 연동 | Ollama 로컬 모델, Tool Calling |
| 안전한 Tool 3개 이상 | read_file, list_dir, search 등 |
| 프롬프트·Schema | JSON 기반 Tool Schema, pydantic 검증 |
| 문서화 | README, 아키텍처 설명 |

### 2.2 포트폴리오 포인트

- LLM을 "학습"이 아닌 "활용" — Tool Calling, Schema 설계
- MCP 프로토콜 이해 및 서버 구현
- 안전한 도메인 설계 — 터미널 없이 Agent 경험

---

## 3. MVP 기능 명세

### 3.1 도메인 선택 (택 1, 추천: A)

| 옵션 | 도메인 | Tool 예시 | 데이터 |
|------|--------|-----------|--------|
| **A** | 개인 지식 베이스 | read_file, search_notes, list_notes | The Record, 마크다운 |
| **B** | 코드베이스 읽기 | read_file, grep_search, list_dir | 프로젝트 소스 |
| **C** | MCP만 (LLM 나중에) | read_file, search_docs | config로 지정 |

**추천 A**: The Record 등 이미 있는 마크다운 노트 활용, "~에 대해 뭐라고 적혀있어?" 질의

### 3.2 Tool 명세 (안전만)

| Tool | 입력 | 출력 | 비고 |
|------|------|------|------|
| read_file | path (상대/절대) | 파일 내용 | 경로 화이트리스트 적용 |
| list_dir | path | 파일·폴더 목록 | 경로 제한 |
| search | query, path? | 매칭된 라인 목록 | grep 또는 간단 검색 |

**경로 제한**: config에 허용 디렉터리 목록, 그 밖 경로 거부

### 3.3 설정

| 항목 | 결정 |
|------|------|
| config | `config.yaml` 또는 `config.json` |
| 허용 경로 | The Record 경로, 프로젝트 루트 등 |
| LLM 모델 | Ollama 기본 모델 (config로 변경 가능) |

---

## 4. 프로젝트 위치

| 항목 | 결정 |
|------|------|
| 개발 레포 | 이 레포 (llm-mcp-agent/) |
| 진실기준 | 레포 `docs/DESIGN.md` |
| The Record | `2_Projects/LLM_MCP_Agent/` — 동기화 복사본 |

---

## 5. 기술 스택

| 항목 | 선택 |
|------|------|
| 언어 | Python 3.11+ |
| LLM | Ollama (로컬) |
| MCP | mcp 패키지 또는 stdio transport |
| 검증 | pydantic |
| CLI | typer |
| 설정 | pyyaml 또는 json |

---

## 6. 패키지 구조 (참고)

```
llm-mcp-agent/
├── docs/
│   ├── DESIGN.md
│   └── PHASE_PROMPTS.md
├── src/
│   ├── main.py           # CLI 진입점
│   ├── mcp_server.py     # MCP 서버
│   ├── tools/
│   │   ├── read_file.py
│   │   ├── list_dir.py
│   │   └── search.py
│   ├── agent.py          # LLM Agent (Phase 2~)
│   └── config.py         # 설정 로드
├── config.yaml
├── pyproject.toml
└── README.md
```

---

## 7. 학습·역할 분담

> 공부용 프로젝트이므로, "내가 직접 해서 배울 부분"과 "AI 도움/단순 반복"을 구분한다.

### 7.1 내가 해서 배울 것

| 항목 | 학습 포인트 | 참고 |
|------|-------------|------|
| **Python 프로젝트 구조** | pyproject.toml, 패키지 분리 | 프로젝트 레이아웃 |
| **Tool 로직** | read_file, list_dir, search 구현 | 파일 I/O, 경로 처리 |
| **config 로드** | YAML/JSON 읽기, 경로 화이트리스트 | 설정 관리 |
| **CLI (typer)** | 서브커맨드, 옵션 | 사용자 인터페이스 |
| **전체 흐름** | MCP ↔ Tool ↔ LLM 연결 | 아키텍처 감각 |

### 7.2 AI 도움이 필요한 부분

| 항목 | 개념 | AI 도움 시 |
|------|------|------------|
| **MCP 서버** | stdio transport, Tool 노출, 요청/응답 형식 | "Python MCP 서버 stdio 예제" |
| **Ollama API** | /api/chat, Tool Calling 형식 | "Ollama Tool Calling Python" |
| **Tool Schema** | JSON Schema, OpenAI 호환 형식 | "MCP Tool Schema 예제" |

### 7.3 단순 반복

| 항목 | 패턴 |
|------|------|
| Tool 클래스 | 입력 검증 → 경로 체크 → 실행 → 반환 |
| 에러 처리 | try/except, 사용자 메시지 통일 |

---

## 8. Phase별 워크플로우

### 워크플로우 공통 원칙

```
[1] 스텝 목표 이해
[2] 내가 직접 개발 (학습 포인트 집중)
[3] 동작 확인 (테스트)
[4] 막히면 AI 도움 요청
[5] 스텝 완료 후 코드 검토 (선택)
[6] 다음 스텝으로
```

---

### Phase 1: MCP 서버 + Tool 2개 (1주)

**목표**: MCP 서버 골격, read_file, list_dir — LLM 없이 Tool만 노출

#### Step 1.1 — 프로젝트 초기 설정

| 구분 | 내용 |
|------|------|
| **개발** | pyproject.toml, src/ 폴더, typer CLI 골격 |
| **테스트** | `python -m src.main --help` 동작 |
| **AI** | pyproject.toml, 의존성 막히면 요청 |
| **검토** | 생략 |

#### Step 1.2 — config + 경로 화이트리스트

| 구분 | 내용 |
|------|------|
| **개발** | config.yaml 로드, allowed_paths 목록 |
| **테스트** | 허용 경로만 접근 가능, 그 밖 경로 거부 |
| **AI** | 생략 가능 |
| **검토** | 생략 |

#### Step 1.3 — read_file, list_dir Tool

| 구분 | 내용 |
|------|------|
| **개발** | read_file(path), list_dir(path) 구현, 경로 검증 |
| **테스트** | 직접 호출 시 파일 내용·목록 반환 |
| **AI** | 생략 가능 |
| **검토** | 생략 |

#### Step 1.4 — MCP 서버 골격

| 구분 | 내용 |
|------|------|
| **개발** | MCP stdio 서버, Tool 2개 등록 |
| **테스트** | Cursor MCP 설정 후 read_file 호출 |
| **AI** | ✅ **"Python MCP 서버 stdio, Tool 노출 예제"** |
| **검토** | ✅ **MCP 서버 구조 검토** |

#### Step 1.5 — Phase 1 통합 테스트

| 구분 | 내용 |
|------|------|
| **테스트** | Cursor에서 read_file, list_dir 호출 검증 |
| **검토** | ✅ **Phase 1 전체 검토** |

**→ Phase 2 시작 시**: `docs/PHASE_PROMPTS.md`의 **"Phase 2 시작 프롬프트"** 복사·붙여넣기

---

### Phase 2: LLM 연동 + Tool Calling (2주)

**목표**: Ollama 연동, 질문 → Tool Calling → 답변 흐름

#### Step 2.1 — Ollama API 호출

| 구분 | 내용 |
|------|------|
| **개발** | Ollama /api/chat 호출, 기본 응답 수신 |
| **테스트** | "hello" 질문 → 응답 출력 |
| **AI** | ✅ **"Ollama Python API 호출 예제"** |
| **검토** | 생략 |

#### Step 2.2 — Tool Schema + Tool Calling

| 구분 | 내용 |
|------|------|
| **개발** | tools 파라미터에 read_file, list_dir 스키마 전달. tool_calls 응답 처리 |
| **테스트** | "X 파일 내용 알려줘" → read_file 호출 → 결과 LLM에 전달 → 최종 답변 |
| **AI** | ✅ **"Ollama Tool Calling, tool_use 응답 처리"** |
| **검토** | 생략 |

#### Step 2.3 — search Tool 추가

| 구분 | 내용 |
|------|------|
| **개발** | search(query, path?) — 텍스트 검색 Tool |
| **테스트** | "~에 대해 검색해줘" → search 호출 → 결과 요약 |
| **AI** | 생략 가능 |
| **검토** | 생략 |

#### Step 2.4 — Agent Loop (질문 → Tool → 답변)

| 구분 | 내용 |
|------|------|
| **개발** | 단일 턴: 질문 → LLM(Tool Calling) → Tool 실행 → LLM(최종 답변) |
| **테스트** | "The Record에서 xrun 관련 내용 알려줘" 등 |
| **AI** | 생략 가능 |
| **검토** | ✅ **Phase 2 전체 검토** |

**→ Phase 3 시작 시**: `docs/PHASE_PROMPTS.md`의 **"Phase 3 시작 프롬프트"** 복사·붙여넣기

---

### Phase 3: 완성 + 문서화 (1주)

**목표**: 프롬프트 튜닝, 문서화, Terminal Agent 시도 준비

#### Step 3.1 — 프롬프트·에러 처리

| 구분 | 내용 |
|------|------|
| **개발** | system prompt 정리, Tool 실패 시 fallback |
| **테스트** | 다양한 질의 시도 |
| **AI** | 생략 가능 |
| **검토** | 생략 |

#### Step 3.2 — README + 문서화

| 구분 | 내용 |
|------|------|
| **개발** | 사용법, 아키텍처, Cursor MCP 설정 방법 |
| **테스트** | — |
| **AI** | 생략 가능 |
| **검토** | 생략 |

#### Step 3.3 — MVP 완료 검토

| 구분 | 내용 |
|------|------|
| **테스트** | 전체 시나리오 (MCP 호출, LLM 질의) |
| **검토** | ✅ **프로젝트 B 전체 코드 검토** |

**→ 완료 후**: Terminal Agent(C) 시도 준비 완료

---

### 워크플로우 체크리스트 (각 스텝마다)

```
□ 스텝 목표 이해했는가?
□ 직접 코드 작성했는가?
□ 테스트 통과했는가?
□ 막혔을 때 AI에 구체적으로 질문했는가?
□ (검토 있는 스텝) 코드 검토 받았는가?
□ 다음 스텝 전에 커밋했는가?
```

---

## 9. 예상 기간

**2~3주** (도메인·Phase 복잡도에 따라 조정)

---

## 10. 참조

- Terminal Agent (완수 후 시도): The Record `2_Projects/TerminalAgent/DESIGN.md`
- xrun (프로젝트 A): `../xrun/docs/DESIGN.md`
