# 적대적 리뷰: cross-agent-image-harness 계획 문서

- 대상: `.omo/plans/cross-agent-image-harness.md` (836줄 / ~240KB / ~89K 토큰)
- 리뷰 일자: 2026-06-21
- 리뷰 범위: (1) LLM 친화적 온톨로지 여부, (2) 논리적 정합성, (3) 2026 최신 기술·논문·OSS 대비 정합성 — 독립 웹 검증 포함
- 상태: 계획 문서 하드닝 목적. `/speckit-*` 진입 전 단계. 본 리뷰는 구현을 승인하지 않음.

---

## 0. 검증 방법론 & 보정(calibration)

이 문서는 N차 개정본이다. `.omo/evidence/`의 기존 리뷰 3건(ontology / plan-quality / consistency)이 지적한 HIGH급 모순은 **대부분 이미 수정됨**을 직접 grep으로 확인했다. 따라서 본 리뷰는 낡은 지적을 반복하지 않고, *남아있는 + 새로 발견한* 문제에만 집중한다.

| 기존 리뷰 지적 | 현재 상태 |
|---|---|
| SQLite-WAL vs "file-first" source-of-truth 모순 | ✅ 해소 (6, 20, 408, 450, 500, 815행 SQLite로 통일; 549행 부록에 supersede 명시) |
| 머신 TL;DR "Large/medium risk" | ✅ "XL/High"로 수정 (20행) |
| "AG-UI-inspired" vs normative 표현 충돌 | ✅ "inspired" 0건; R3-PROTO로 단일 정의 |
| "tests-after for CLI/adapters" | ✅ 0건; 484행 "before or alongside"로 수정 |
| 온톨로지 placeholder ID `(all)`/`(root)`/`(intake)` | ✅ 제거; R3.3 references가 `M-*` ID 사용 |
| `core/proto` namespace 누수 | ✅ 제거 |
| "R3 alone" 자족성 과장 | ✅ 131행 "minimal safe load set = R2+R3"로 약화 |

**전체 평결: 계획 문서로서 매우 높은 완성도(상위 1%). 단, 3가지 구조적 결함과 1건의 실제 사실 오류가 남아 있으며, 이들은 모두 "휘발성·중복" 문제로 수렴한다.**

---

## 질문 1 — LLM 친화적 온톨로지인가?

**판정: R3 온톨로지 자체는 설계가 탁월. 그러나 "LLM 친화"의 핵심인 *물리적 로드 가능성*에서 자기모순.**

### 강점 (실제로 잘 됨)
- 네임스페이스(`core.*`/`pack.*`/`proto.*`/`sec.*`) + 타입드 ID(`E-`/`M-`/`S-`/`R-`/`INV-`/`CAP-`/`V-`)로 ID 주소화.
- GraphRAG식 community(R3.1a) + SKOS prefLabel/altLabel(R3.6) + RDF triple 매핑(R3.2) — 실제 표준에 정확히 정합.
- INV-*를 "Do not…" 부정문이 아닌 양의 공리(positive axiom)로 재표현 — LLM 추론에 우월.
- llms.txt식 진입점(R3.8) + 최소 로드 순서(R3-STD).

### 🔴 L1 — llms.txt를 인용하면서 정작 monolith로 배포
R3-STD(139행)는 llms.txt의 *"context windows cannot hold whole corpora"*를 근거로 든다. 그런데 문서 자체가 ~89K 토큰 단일 파일이고, "skippable optional tier"인 산문(Scope/Verification/Candidate/Success)이 전체의 약 60%를 차지한다. llms.txt의 본질은 *링크로 분리된 작은 파일들*인데, 이 문서는 형식(load-order 안내)만 빌리고 실질(개별 로드 가능한 파일 분리)은 구현하지 않았다.
→ R2/R3를 `R2-facts.md`, `R3-ontology.md`로 물리 분리하고 본문은 ID 참조만 남길 것.

### 🔴 L2 — 유일하게 타입화 안 된 핵심: 생명주기 상태기계(R3.4)
엔티티·관계·매니페스트·불변식은 전부 타입드 테이블인데 상태기계만 산문이다. `A -> B -> (submitted|running|pending) -> …` 나열 + guard 한 문장만 존재. `(from_state, event, guard, to_state)` 엣지 테이블이 없어 코드 변환 LLM이 합법 전이를 추론해야 한다. 온톨로지에서 가장 약한 고리. (기존 ontology 리뷰 MEDIUM#1이 가리켰으나 미해결.)

### 🔴 L3 — R2 > R3 우선순위 규칙이 *동기화 갭*을 만든다
"R2(facts) > R3(ontology) > prose"(131행)는 깔끔해 보이나, R2와 R3는 깨끗이 분리되지 않는다. INV-7(SQLite-WAL)=R2.4 결정, INV-18(sandbox)=R2.11 결정 — 한 개념이 양쪽에 산다. source-refresh로 R2 사실이 바뀌면(예: MCP 2025-11-25 → 2026-07-28) **R3의 어떤 INV/cluster를 재검증할지 강제하는 메커니즘이 없다.** 우선순위 규칙은 "충돌 시 R2 승"만 말할 뿐 역추적 링크가 없어, R3가 stale해도 여전히 "canonical ontology"로 남는다.
→ R2 사실 행에 `F-N` ID 부여 + 이를 인용하는 R3 항목이 `F-N`을 명시 역참조.

---

## 질문 2 — 내용이 논리적인가?

**판정: 국소 논리는 매우 견고. 그러나 *중복이 모순을 생산하는 구조*가 여전히 가동 중이며, 그 증거가 이미 두 개 존재한다.**

### 🔴 C1 — "중복 = 모순 생성기"가 실증되었다
R3는 "왜 R3가 존재하는가"에서 산문이 같은 개념을 4~6회 재정의해 LLM-hostile하다고 스스로 진단한다(135행). 그런데 해결책으로 산문을 제거한 게 아니라 R2+R3를 그 위에 얹었다 — 리팩터링이 아니라 *증식*. Q-id 재기술 빈도: **Q37=31회, Q29=18회, Q26·Q28·Q32=11~12회.** 각 결정 변경은 N개 위치에 전파돼야 하며 하나라도 놓치면 새 모순이 된다.

증거 2건:
1. **SQLite/file-first 모순**이 정확히 이 메커니즘으로 발생했고, 수정에 약 10개 위치(6·20·74·77·408·450·500·785·815…)를 건드려야 했다. 기존 리뷰가 잡았을 때 6곳 이상이 stale했다.
2. **AgentDyn 불일치**(F1) — 본문과 자체 증거 파일이 어긋난다.

→ 부록 A/B에 "NON-EXECUTABLE" 경고 라벨은 붙였으나 *중복 내용 자체는 그대로*(예: Candidate 1, 555행은 Q19~Q37을 ~2000단어로 재기술). 산문을 R3-ID 참조로 collapse하지 않는 한 이 기계는 계속 모순을 찍어낸다.

### 🟠 C2 — "Cross-Agent" 전제 vs 동시성 모델(R2.6) 긴장
제품명은 "Cross-Agent"인데 R2.6은 동시성을 "same-host 에이전트가 한 workspace 디렉터리 공유, 주로 순차 hand-off"로 잠근다. 운영 함정: SQLite-WAL은 네트워크 FS에서 동작 안 함(F-11). 2026 타깃 사용자(Codex, Claude Code)는 점점 격리 컨테이너에서 돌고, 두 에이전트가 Docker 볼륨을 공유하면 사실상 network-FS 락 의미론이라 WAL 손상 가능. 계획은 "same host (no network FS)"라고만 적고 *컨테이너화된 same-host*를 정의하지 않는다.

### 🟠 C3 — Q33 "live-first" + Q7/Q9 "autopilot"의 선지출(pre-spend) 게이트 공백
- Q33: credential 풀리면 기본값이 실제 유료 실행.
- Q7/Q9: 검증 통과 patch는 budget envelope 내 자동 실행.
- 인간 게이트는 review(지출 후)와 resume(handoff 후)에만.

→ intake 직후 첫 autopilot 배치가 사람이 결과를 보기 전에 envelope 전액을 태울 수 있다. 안전 논리는 "envelope ≤ provider cap"에만 의존. OWASP Agentic 2026 ASI Unbounded Consumption 관점에서 위험한 기본값. **첫 autopilot 배치 전 dry-cost-estimate 확인** 게이트 필요.

### 🟡 C4 — "checksum chain" 이중 존재의 미설명
R2.4는 "manual checksum chains를 SQLite가 대체"한다면서 event_log에 `prev_hash/curr_hash` audit chain을 유지(75행)하고 snapshot/migration/export도 checksum chain을 쓴다. 실제로는 모순이 아님(SQLite=프라이머리 무결성, hash chain=export 아티팩트 tamper-evidence). 그러나 왜 둘 다 필요한지 미설명 → 한 줄 근거 추가.

---

## 질문 3 — 2026 최신 기술/논문/OSS 대비 정합성 (독립 웹 검증)

**판정: R2.1 사실 테이블은 ~95% 정확 — 2026 휘발성 사실 13건 대부분이 1차 출처와 일치(드물게 높은 수준). 단 1건의 실제 오류 + 몇 건의 staleness/누락.**

### ✅ 독립 검증 통과
| 사실 | 계획 주장 | 검증 결과 |
|---|---|---|
| F-3 A2A | v1.0.0 최신, signed Agent Cards, "v1.2 오류" | ✅ 정확. v1.0 stable 2026-04, LF 거버넌스 |
| F-6 PresentBench | arXiv 2603.07244, 238 instances, ~54 checklist | ✅ 정확. 평균 54.1 binary checklist, 2026-03 |
| F-9 MCP | 최신 spec 2025-11-25 | ✅ 검증일 기준 정확 (F2 참조) |
| F-12 gpt-image-2 | OpenAI 공식 이미지 모델 | ✅ 정확. 2026-04-21 출시, DALL·E 2/3 2026-05-12 폐기 |
| F-13 CycloneDX | 1.7이 현재 primary, ML-BOM | ✅ 정확. 1.7 2026-03 |
| F-4 Comfy Cloud | `POST /api/prompt`, `GET /api/job/{id}/status`, X-API-Key, EXPERIMENTAL | ✅ 정확. status enum·실험적 경고 일치 |
| F-1 Agent Skills | `npx skills add`, SKILL.md+YAML, 엔드포인트 beta | ✅ 정확. manual fallback 결정 타당 |
| F-7 OWASP Agentic | 2026 리스트 실재 | ✅ 정확. ASI01~ASI10:2026 |
| R2.12 NSA MCP | "MCP Security Design Considerations (2026)" | ✅ 정확. NSA 2026-05/06 발행 |

### 🔴 F1 — AgentDyn을 "미확인"으로 잘못 강등 (105행)
R2.9는 "'WASP', 'AgentDyn' … not verified to a primary source … downgraded … re-confirmed or dropped"라고 적는다. 그러나:
- AgentDyn은 실재·검증 가능: arXiv **2602.03117**, 공식 GitHub 구현, AgentDojo 위에 구축(60 tasks/560 injection cases).
- 결정적으로, 이 프로젝트 자신의 증거 파일 `.omo/evidence/plan-consistency-review-20260621.md:25`가 정확한 arXiv URL로 이미 인용 중.

→ 본문(plan)과 자체 증거(evidence)가 정면 충돌. 단순 stale가 아니라 "verified 2026-06-21" 스탬프 신뢰도를 깎는다. (반대로 "WASP"는 1차 출처 미확인 → 강등 타당.)

### 🟠 F2 — 검증일 이후 이미 움직인 것들 (source-refresh가 잡아야)
- **MCP 차기 spec `2026-07-28`이 이미 release candidate**: stateless core, **Tasks**, **MCP Apps**, Extensions. 단순 버전 범프가 아니라 R2.5 아키텍처와 충돌 가능 — MCP "Tasks"가 provider-job/task 모델과, "MCP Apps"가 AG-UI surface 결정과 기능 중복. source-refresh에서 "broker가 MCP Tasks와 겹치는가"를 아키텍처 차원 재평가 필요.
- **AgentDojo 버전 stale**: 계획은 "v0.1.35, 2025-10" 고정, 최근 연구는 ~1.2.1 사용. 핵심 주장은 유효하나 버전 핀이 낡음.
- **deck 벤치마크 지형 이동**: PresentBench는 정확하나 더 이상 유일·최신 아님 — UniPPTBench(arXiv 2605.17356, 2026-05), ArcBench 등장.

### 🟠 F3 — OWASP Agentic 2026을 구체 매핑하지 않음
계획은 OWASP Agentic 2026을 인용(126행)하면서 열거 카테고리는 2025 LLM Top 10(26행)이다. 2026 agentic 리스트(ASI01~10)는 계획 cluster와 거의 1:1 매핑:

| OWASP ASI 2026 | 계획 대응 |
|---|---|
| ASI01 Agent Goal Hijack | prompt-carrier boundary (INV-8) |
| ASI02 Tool Misuse | capability broker (INV-1, CAP-*) |
| ASI03 Identity & Privilege Abuse | actor identity/trust (Q30, INV-4) |
| ASI04 Supply Chain Compromise | supply-chain attestation (INV-14, M-sec-aibom) |
| ASI05 Unexpected Code Execution | worker sandbox (INV-18) |
| ASI06 Memory & Context Poisoning | opt-in project memory gates |
| ASI07 Insecure Inter-Agent Comms | A2A signed cards (INV-23) |
| ASI09 Human-Agent Trust Exploit | resume confirmation (INV-12) |
| ASI10 Rogue Agents | trust registry (INV-3/4) |

→ INV-*를 ASI01~10에 명시 추적 매핑하면 2025 분류 대신 현재 표준에 정렬되고, 커버리지 공백(ASI08 Cascading Agent Failures는 현재 약함)이 드러난다.

### 🟠 F4 — "omni-scope 금지" 원칙의 비대칭 적용
R2.2는 "core가 이미 해결된 인프라를 재구현하는 omni-scope"를 1순위 리스크로 명명하고, R2.3~R2.6에서 상태(SQLite)·provider job(Comfy Cloud)·동시성은 기성품 재사용으로 잘 축소했다. 그러나 보안 인프라는 여전히 hand-roll: capability broker(idempotency/retry/cancel/terminal=미니 durable-execution 엔진), supply-chain(SBOM/AIBOM·vuln·revocation·typosquat), multi-backend sandbox(WASI/container/seccomp/Seatbelt), spend ledger(circuit breaker·price-drift·kill-switch), localhost UI 보안 스택. 각각 성숙 OSS 존재(provenance=Sigstore/cosign, SBOM·vuln=Syft/Grype, sandbox=기존 WASI 런타임/bubblewrap, auth=OAuth lib). "재구현 금지"가 state/job엔 적용되고 security엔 미적용 — 같은 원칙의 비대칭. Epic B를 "신규 구현"이 아닌 "기성 OSS 오케스트레이션"으로 재프레이밍하면 XL 규모·위험이 실질 감소.

---

## 우선순위 액션 (영향순)

1. **[C1·구조] 산문을 R3-ID 참조로 collapse.** Scope/Verification/Candidate/Success의 중복 재기술 제거, `→ R3.x / INV-n / M-*` 링크로 치환. 모든 모순의 근원 원인 제거.
2. **[F1·사실] AgentDyn 강등 즉시 정정**(105행). arXiv 2602.03117 + 공식 GitHub로 confirmed 처리, 자체 evidence와 정합화. F-1~F-13 + AgentDyn/WASP 일괄 재검증 권장.
3. **[L3·온톨로지] R2 사실에 `F-N` ID 부여 + R3 항목 역참조** → source-refresh 변경이 R3 재검증을 강제.
4. **[L2·온톨로지] R3.4를 `(from, event, guard, to)` 전이 테이블로** 타입화.
5. **[C3·안전] 첫 autopilot 배치 전 dry-cost-estimate 확인 게이트** 추가.
6. **[C2·운영] "same-host"를 컨테이너 맥락에서 정의** + 공유 볼륨=network-FS WAL 손상을 명시적 stop-condition으로.
7. **[F2/F4] source-refresh gate에** (a) MCP 2026-07-28 Tasks/Apps 아키텍처 중복 재평가, (b) Epic B 보안 컴포넌트 OSS 재사용 검토, (c) OWASP ASI01~10 ↔ INV-* 매핑 추가.

---

## 한 줄 결론
온톨로지 설계와 사실 검증의 *내용물*은 2026 기준 최상급이다. 약점은 전부 "포장"에 있다 — ~89K 토큰 monolith에 캐논(R2/R3)과 중복 산문이 공존하는 구조가 (이미 두 번) 모순을 생산했고, LLM 친화성 목표 자체를 갉아먹는다. 캐논을 분리·축소하고 산문을 ID 참조로 collapse하는 단 하나의 리팩터링이 발견된 문제 대부분을 동시에 닫는다. `/speckit-*` 진입 전 액션 1~2는 반드시 처리할 것.

---

## Sources (독립 웹 검증, 접근일 2026-06-21)
- PresentBench: https://arxiv.org/abs/2603.07244 · https://github.com/PresentBench/PresentBench
- A2A: https://a2a-protocol.org/latest/specification/ · https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year
- gpt-image-2: https://developers.openai.com/api/docs/models/gpt-image-2 · https://openai.com/index/introducing-chatgpt-images-2-0/
- MCP: https://modelcontextprotocol.io/specification/2025-11-25 · https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/
- CycloneDX: https://github.com/CycloneDX/specification/releases · https://cyclonedx.org/capabilities/mlbom/
- Comfy Cloud API: https://docs.comfy.org/development/cloud/overview
- AgentDyn: https://arxiv.org/abs/2602.03117 · https://github.com/SaFo-Lab/AgentDyn
- AgentDojo: https://github.com/ethz-spylab/agentdojo
- Agent Skills: https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills · https://github.com/anthropics/skills
- OWASP Top 10 for Agentic Applications 2026: https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
- NSA MCP Security: https://media.defense.gov/2026/Jun/02/2003943289/-1/-1/0/CSI_MCP_SECURITY.PDF
- UniPPTBench (freshness): https://arxiv.org/html/2605.17356
