# LLM + MCP Agent Phase별 시작 프롬프트

> 대화창을 Phase마다 바꿔서 진행할 때 사용. 새 대화 시작 시 아래 해당 Phase 프롬프트를 전체 복사해 붙여넣기.

---

## 문서 규칙

- **진실기준**: 이 레포 `docs/` (개발 중)
- **동기화**: The Record `2_Projects/LLM_MCP_Agent/` 에 기회될 때마다 갱신

---

## Phase 1 시작 프롬프트 (지금 사용)

```
LLM + MCP Agent 프로젝트 (프로젝트 B) Phase 1을 진행하려고 해. 아래 컨텍스트를 바탕으로 도와줘.

## 프로젝트 개요
- 안전한 도메인(파일 읽기, 검색, 문서 조회)에서 MCP 서버 + LLM Agent 구현
- 터미널/쉘 실행 금지. Terminal Agent(C) 선행 프로젝트
- 공부용이라 내가 직접 개발해서 배우는 부분과 AI 도움이 필요한 부분이 구분되어 있음

## 현재 Phase: 1 (MCP 서버 + Tool 2개)
**목표**: MCP 서버 골격, read_file, list_dir — LLM 없이 Tool만 노출

## 스텝 (1.1 → 1.5)
1.1 프로젝트 초기 설정 (pyproject.toml, src/, typer CLI)
1.2 config + 경로 화이트리스트
1.3 read_file, list_dir Tool 구현
1.4 MCP 서버 골격 — AI 도움
1.5 Phase 1 통합 테스트 + 검토

## 워크플로우
- 나: 스텝 목표 이해 → 직접 개발 → 테스트
- 막히면: "~를 시도했는데 ~에서 막혔어" 구체적으로 질문
- 검토: 1.4 완료 시 MCP 서버, 1.5 완료 시 Phase 1 전체

## 경로·문서 (레포 = 진실기준)
- 개발 레포: 이 llm-mcp-agent 레포
- 설계 문서: docs/DESIGN.md
- Phase 프롬프트: docs/PHASE_PROMPTS.md

## 학습·역할 분담 (DESIGN 7절)
- 내가 할 것: Python 구조, Tool 로직, config, CLI
- AI 도움: MCP 서버 stdio, Tool 노출
- 단순 반복: Tool 클래스 패턴

지금 Step 1.1부터 시작할게. (또는 진행 중인 스텝을 알려줘)
```

---

## Phase 1 마무리 시 → Phase 2 시작 프롬프트

Phase 1 완료 후, **새 대화**를 열고 아래를 붙여넣기:

```
LLM + MCP Agent 프로젝트 Phase 2를 진행하려고 해. Phase 1(MCP 서버, read_file, list_dir)은 완료했어.

## 프로젝트 개요
- 안전한 도메인에서 MCP + LLM Agent. 터미널 실행 금지
- Phase 1 완료: MCP 서버, read_file, list_dir Tool

## 현재 Phase: 2 (LLM 연동 + Tool Calling)
**목표**: Ollama 연동, 질문 → Tool Calling → 답변 흐름

## 스텝 (2.1 → 2.4)
2.1 Ollama API 호출 — AI 도움
2.2 Tool Schema + Tool Calling — AI 도움
2.3 search Tool 추가
2.4 Agent Loop (질문 → Tool → 답변) + Phase 2 전체 검토

## 경로·문서
- 개발 레포: 이 llm-mcp-agent 레포
- 설계 문서: docs/DESIGN.md

지금 Step 2.1부터 시작할게.
```

---

## Phase 2 마무리 시 → Phase 3 시작 프롬프트

Phase 2 완료 후, **새 대화**를 열고 아래를 붙여넣기:

```
LLM + MCP Agent 프로젝트 Phase 3를 진행하려고 해. Phase 1(MCP, Tool), Phase 2(LLM, Tool Calling) 완료했어.

## 프로젝트 개요
- 안전한 도메인 MCP + LLM Agent. Phase 1·2 완료

## 현재 Phase: 3 (완성 + 문서화)
**목표**: 프롬프트 튜닝, README, Terminal Agent 시도 준비

## 스텝 (3.1 → 3.3)
3.1 프롬프트·에러 처리
3.2 README + 문서화
3.3 MVP 완료 검토

## 경로·문서
- 개발 레포: 이 llm-mcp-agent 레포
- 설계 문서: docs/DESIGN.md

지금 Step 3.1부터 시작할게.
```

---

## Phase 3 마무리 시 → 완료 후

Phase 3 완료 후:

```
LLM + MCP Agent (프로젝트 B) MVP 개발이 완료됐어. MCP 서버, read_file/list_dir/search Tool, Ollama Tool Calling, Agent Loop까지 구현했어.

다음 중 도움이 필요해:
1. README 보완, 포트폴리오용 정리
2. Terminal Agent(C) 시도 전 최종 점검

개발 레포: 이 llm-mcp-agent 레포
설계 문서: docs/DESIGN.md
```
