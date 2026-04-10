# Family Office

이 저장소는 **Slack-first Family Office 운영체계**를 설계하고 구현하기 위한 문서 저장소다.

핵심 방향은 단순하다.

- **Harness**로 조직 구조와 협업 패턴을 설계한다.
- **Claude Managed Agents / agentic best practices**로 실행 레이어를 안정적으로 운영한다.
- **리**가 front door / supervisor / single intake가 된다.
- **CIO / CFO / CTO / COO**는 governance layer로서 판단, 승인, ownership을 맡는다.
- **exec 채널**은 visible execution lane으로 운영한다.
- **#fo-lounge**는 cross-functional discussion / idea exchange / briefing 공간으로 운영한다.
- 중요한 결과물은 **generate → validate → finalize**를 거친다.
- 최종 사용자-facing 결론은 가능한 한 **single-owner voice**로 정리한다.
- Slack에서는 **스레드 강제를 최소화**하고 채널 본문 중심으로 자연스럽게 운영한다.

## 핵심 문서

### 1. 전체 운영 설계
- `projects/family-office/DESIGN.md`

### 2. 실행 운영 문서
- `projects/family-office/docs/OPERATING_MODEL.md`
- `projects/family-office/docs/MANAGED_TASK_TEMPLATE.md`
- `projects/family-office/docs/VALIDATION_RUBRIC.md`

## 현재 Slack 구조

기준 채널:
- `#chief-of-staff` → `C0AR0DCSP7F`

생성된 Family Office 채널:
- `#fo-lounge` → `C0ASK6209BJ` (cross-functional discussion / briefing lounge)
- `#cio-general` → `C0ARMFEMYE9`
- `#cfo-general` → `C0ARUFFQERJ`
- `#cto-general` → `C0ARNRHKSQ6`
- `#coo-general` → `C0ARUF7TJPN`

정리된 채널:
- `이씨-가문-경영` → archived

## 이 레포가 정의하는 것

이 레포는 아래를 문서로 고정하려고 한다.

1. **누가 front door인지**
2. **누가 감독/승인자인지**
3. **어떤 요청에 어떤 orchestration pattern을 쓰는지**
4. **general / exec를 어떻게 분리하는지**
5. **장기 작업을 어떤 checkpoint와 handoff 형식으로 운영하는지**
6. **최종 결론을 누가 어떤 목소리로 게시하는지**

## FO 아키텍처 한 줄 요약

이 Family Office는:

- **리 = Supervisor**
- **C레벨 = Expert Pool + Governance Layer**
- **exec lane = Managed Task Runner / Generate Layer**
- **general lane = Validate / Approve Layer**
- **복합 과제 = Fan-out / Fan-in**
- **장기 과제 = Hierarchical Delegation + Checkpoint / Resume**

으로 운영한다.

즉 “여러 봇이 같이 떠드는 슬랙방”이 아니라,
**역할, 실행, 검증, 게시 책임이 분리된 운영체계**를 목표로 한다.

## 우선 구현 순서

1. `DESIGN.md` 기준으로 역할/라우팅 모델 확정
2. `OPERATING_MODEL.md` 기준으로 채널별 발화 규칙 고정
3. `MANAGED_TASK_TEMPLATE.md` 기준으로 exec lane 작업 형식 고정
4. `VALIDATION_RUBRIC.md` 기준으로 general lane 승인 기준 고정
5. welcome / operating prompt / role prompt로 실제 Slack 운영에 반영

## 현재 중요한 운영 원칙

- 오너는 어디로 말해야 할지 고민하지 않게 한다.
- 리가 기본 intake를 맡는다.
- C레벨은 실무 로그 생산보다 판단과 승인에 집중한다.
- `#fo-lounge`에서는 자유 논의와 아이디어 탐색이 가능하다.
- 다만 논의와 승인, 실행, 최종 게시 책임은 서로 분리한다.
- 실행은 숨기지 않지만, general 채널에 소음을 쌓지 않는다.
- 중간 산출물은 checkpoint 가능한 형태로 남긴다.
- 최종 결론은 한 명이 책임지고 게시한다.
- 검증 가능성(verification) 없는 작업은 품질이 급격히 떨어지므로, outcome / success criteria / validation 기준을 항상 명시한다.

## 다음 문서화 후보

- 채널별 welcome prompt
- 역할별 system prompt / role prompt
- exec lane status model
- publish workflow
- daily report routine
- long-running session resume protocol
