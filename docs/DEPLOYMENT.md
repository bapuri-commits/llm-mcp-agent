# llm-mcp-agent — VPS 배포 가이드

> ⚠️ **초안 문서** — 구현 진행 시 실제 환경에 맞게 수정될 수 있음.

> 구현과 동시에 배포를 고려하는 DevOps-first 프로젝트.
> DevOps 학습 로드맵 Stage 10에서 사용.

---

## 아키텍처 개요 (설계 기준)

```
[CLI / API 클라이언트]  →  [MCP 서버 (Python)]  →  [Ollama (로컬 LLM)]
                                 ↓
                          [Tool 실행 (파일 읽기, 검색)]
```

---

## 사전 요구사항 (예정)

- Python 3.11+
- Ollama (로컬 LLM 서버)
- VPS RAM: Ollama 모델에 따라 4~8 GB

---

## 현재 상태

⚠️ **설계 문서만 존재.** 구현은 아직 시작되지 않음.

이 문서는 구현이 진행되면서 채워질 예정:

- [ ] Ollama 설치 및 모델 설정
- [ ] MCP 서버 Dockerfile 작성
- [ ] systemd 또는 Docker 서비스 등록
- [ ] CI/CD 파이프라인 구축

---

## Ollama VPS 설치 (Stage 10 준비)

```bash
# TODO: 구현 시점에 실행
# curl -fsSL https://ollama.com/install.sh | sh
# ollama pull llama3.2:7b
# ollama serve
```

RAM 가이드:
- 3B 모델: ~2 GB
- 7B 모델: ~5 GB
- 13B 모델: ~10 GB (VPS에서는 다른 서비스와 충돌 가능)

---

## DevOps-First 원칙

이 프로젝트는 처음부터 다음을 고려하며 개발:
1. **Dockerfile**을 코드와 함께 작성
2. **환경변수**로 설정 분리
3. **GitHub Actions**로 테스트 + 배포 자동화
4. **로그 구조화** (JSON 로그 등)
