# 패밀리 오피스 (FO) — Slack Harness × Managed Agents 설계 문서

## 개요
이 Family Office는 두 가지 레퍼런스를 합쳐 설계한다.

- **revfactory Harness**: 조직 구조, 역할 분리, orchestration pattern 설계 프레임
- **Claude Managed Agents / agentic best practices**: 실행 런타임, 검증, 장기 작업 운영 철학

즉,
- Harness는 **누가 어떤 역할로 협업하는가**를 정의하고
- Managed Agents는 **그 협업을 어떤 방식으로 안정적으로 실행하는가**를 정의한다.

핵심 목표는 네 가지다.

- 리가 프론트도어가 되는 총괄 운영 체계
- C레벨이 관리감독과 판단에 집중하는 구조
- 실행은 숨기지 않되, exec 채널로 분리해 보이게 운영하는 구조
- 장기 작업, 병렬 작업, 검증 가능한 작업 실행을 기본 전제로 두는 구조

이 시스템은 “여러 봇이 같은 방에서 동시에 떠드는 구성”이 아니다.
대신 아래 원칙으로 움직인다.

- 리가 intake, routing, orchestration, synthesis를 담당한다.
- CIO/CFO/CTO/COO는 governance layer로서 판단과 승인에 집중한다.
- 실행 유닛은 exec 채널에서 generate 작업을 수행한다.
- 중요한 결과물은 validate 단계를 반드시 거친다.
- 최종 결론은 가능한 한 single-owner 방식으로 게시한다.
- 복합 과제는 장기 세션, 체크포인트, 실행 추적을 고려한 managed execution으로 다룬다.

---

## 참조 모델

### Harness에서 가져오는 것
- Supervisor 중심 구조
- Specialist Pool
- Worker / execution layer
- Generate / Validate
- Fan-out / Fan-in
- Producer-Reviewer
- Hierarchical Delegation
- 역할별 발화 규칙
- 채널 기반 운영 설계

### Managed Agents / Claude Code Best Practices에서 가져오는 것
- outcome 중심 task execution
- explore → plan → implement 분리
- verification-first 작업 방식
- long-running sessions
- checkpoint / resumability
- scoped permissions / governance
- execution tracing / observability
- 병렬 태스크 오케스트레이션
- context 관리와 요약 handoff

### 결론
이 FO는 **“Harness로 조직을 설계하고, Managed Agents 방식으로 실행 레이어를 운영하는 Slack Family Office”**로 간다.

---

## 핵심 정의

### 1. FO는 Slack 조직이다
FO는 단순 채널 묶음이 아니라,
요청 intake → task routing → execution → validation → publish까지 이어지는 운영체계다.

### 2. 리는 Supervisor다
리는 기본 front door이자 supervisor다.
오너가 어느 채널, 어느 역할, 어느 실행 유닛을 직접 골라야 하는 구조를 피한다.

### 3. C레벨은 Governance Layer다
CIO/CFO/CTO/COO는 실무 로그 생산자가 아니라,
품질 기준, 판단, 승인, ownership을 맡는 관리감독 레이어다.

### 4. exec 채널은 Managed Execution Layer다
exec는 단순 잡담 공간이 아니라,
명시된 outcome / success criteria / constraints / checkpoint를 가진 작업이 수행되는 공간이다.

### 5. general 채널은 Validate Layer다
general은 단순 상태 공유방이 아니라,
exec 결과물을 검토하고 승인 / 반려 / 수정 요청을 내리는 판단 레이어다.

### 6. 최종 게시자는 한 명이다
최종 사용자-facing 결론은 single-owner 원칙으로 게시한다.
여러 에이전트가 동시에 최종 결론을 쏟아내지 않는다.

---

## Harness 매핑
이 설계는 Harness 개념을 FO 조직에 다음처럼 매핑한다.

- **Supervisor** → 리 (Chief of Staff)
- **Expert Pool** → CIO / CFO / CTO / COO
- **Worker / Execution Layer** → exec 채널의 visible execution agents
- **Generate** → exec 채널에서 조사, 초안, 구현, 분석 수행
- **Validate** → general 채널에서 C레벨 검토, 승인, 반려
- **Fan-out / Fan-in** → 복합 과제를 병렬 분해하고 리가 최종 취합
- **Producer-Reviewer** → exec가 초안/구현을 만들고 general이 리뷰/승인
- **Hierarchical Delegation** → 큰 과업을 C레벨이 실행 유닛 또는 하위 작업으로 쪼개 위임

---

## Managed Execution 매핑
Managed Agents 관점에서 FO 실행 레이어는 다음처럼 해석한다.

- **Outcome definition** → 리 또는 C레벨이 먼저 “무엇이 성공인지” 정의
- **Explore first** → 실행 전에 필요한 조사와 맥락 수집 수행
- **Plan before implementation** → 작은 작업이 아니면 구현 전에 계획을 세움
- **Managed task runner** → exec 채널의 실행 유닛이 task template 기반으로 작업 수행
- **Verification loop** → 가능한 테스트, 체크리스트, 비교, 검토 기준으로 결과를 검증
- **Long-running session** → 길게 가는 과제는 세션 단위로 이어서 수행
- **Checkpoint / resumability** → 중간 산출물과 상태를 남기고 재개 가능하게 설계
- **Scoped permissions** → 각 역할은 필요한 범위의 도구와 권한만 사용
- **Execution tracing** → 누가 무엇을 생성/검토/승인했는지 추적 가능해야 함
- **Context hygiene** → 긴 조사 로그 대신 요약 handoff를 통해 main/general 문맥을 얇게 유지

즉 FO는 “잘 말하는 다중 봇”이 아니라,
**검증 가능하고 이어서 할 수 있는 실행 시스템**이어야 한다.

---

## 설계 원칙

### 1. 리는 프론트도어다
오너가 어디로 말해야 할지 고민하지 않도록, 기본적으로 리가 먼저 받는다.

리의 역할:
- 요청 해석
- 담당 부문 지정
- 다부문 과제 orchestration
- 우선순위 충돌 조정
- 최종 보고 편집
- cross-functional fan-in

### 2. C레벨은 실무자가 아니라 감독자다
CIO/CFO/CTO/COO는 직접 모든 일을 수행하는 봇이 아니다.
이들은 다음 역할을 맡는다.

- 업무 배정
- 품질 기준 제시
- 리스크 판단
- 승인 / 반려
- 최종 책임

즉 C레벨은 **관리감독, validation, ownership 레이어**다.

### 3. 실행은 보이게 드러내되, 관리 가능한 형태로 다룬다
내부 실행은 완전히 숨기지 않는다.
다만 general 채널을 오염시키지 않기 위해 exec 채널로 분리한다.

이로써 다음을 동시에 만족한다.
- 블랙박스처럼 안 보이지 않음
- 실행 과정이 추적 가능함
- 메인 판단 채널은 소음이 줄어듦
- 장기 과제와 병렬 과제를 안정적으로 이어갈 수 있음

### 4. Generate / Validate / Finalize를 기본 파이프라인으로 쓴다
중요한 결과물은 “한 번 생성하고 끝”이 아니라 다음 순서를 따른다.

1. Generate: 초안, 조사, 구현, 분석
2. Validate: 검토, 리스크 평가, 승인 / 반려
3. Finalize: 리 또는 해당 부문장이 최종 정리 및 게시

### 5. Verify 가능한 작업을 우선한다
verification이 없는 작업은 품질이 급격히 떨어진다.
따라서 가능한 경우 아래를 반드시 정의한다.

- expected output
- 성공 조건
- 테스트 / 점검 방법
- 비교 기준
- 실패 조건 또는 rollback 조건

### 6. 최종 결론은 한 명이 낸다
병렬 분석은 가능하지만, Slack에서 여러 에이전트가 동시에 최종결론을 말하면 피곤해진다.
따라서 final owner를 명확히 둔다.

- 전체 과제면 리
- 단일 부문 과제면 해당 C레벨

### 7. 긴 작업은 checkpoint / resume를 기본으로 한다
길게 가는 과제는 한 번에 끝내는 것보다,
중간 상태를 남기고 이어서 진행할 수 있어야 한다.

---

## 어떤 패턴을 언제 쓰는가

### 1. Supervisor 패턴
사용 상황:
- 오너가 애매하거나 복합적인 요청을 던질 때
- 어느 부문이 맡아야 할지 아직 명확하지 않을 때

FO 매핑:
- 리가 intake, triage, routing, synthesis 담당

### 2. Expert Pool 패턴
사용 상황:
- 요청은 명확하지만 어느 부문 owner가 맞는지 선택이 필요할 때
- 단일 부문 전문 판단이 핵심일 때

FO 매핑:
- CIO / CFO / CTO / COO 중 적절한 부문으로 라우팅

### 3. Fan-out / Fan-in 패턴
사용 상황:
- 여러 부문이 동시에 관여하는 문제
- 비교 / 병렬 검토 / 다각도 분석이 필요한 문제

FO 매핑:
- 리가 부문별 작업을 병렬 분해
- 각 부문이 요약 반환
- 리가 최종 취합

### 4. Producer-Reviewer 패턴
사용 상황:
- 초안/구현 생성 후 검토가 필요한 작업
- 품질 보증이 중요한 작업

FO 매핑:
- exec 채널이 generate
- general 채널이 review / validate

### 5. Pipeline 패턴
사용 상황:
- 단계 순서가 명확하고 앞 단계 결과가 다음 단계 입력이 되는 작업
- 예: 조사 → 초안 → 검토 → 게시

FO 매핑:
- exec에서 조사/작성
- general에서 승인
- 리 또는 C레벨이 게시

### 6. Hierarchical Delegation 패턴
사용 상황:
- 큰 과제를 하위 과업으로 계속 쪼개야 할 때
- 제품 개발, 장기 운영 구축, 대규모 변화 작업

FO 매핑:
- 리 또는 C레벨이 상위 outcome 정의
- exec에 하위 과업 위임
- 하위 과업은 checkpoint 기반으로 운영

---

## 조직 구조

```text
화 (Owner)
 ├─ Telegram DM
 │   └─ 리 (개인 지시 / 긴급 채널)
 └─ Slack
     ├─ #chief-of-staff
     │   └─ 리 (Supervisor / Chief of Staff)
     ├─ #fo-lounge
     │   └─ 전체 브리핑 / 공용 공유
     ├─ CIO layer
     │   ├─ #cio-general   (판단 / 승인 / 전략)
     │   └─ #cio-exec      (조사 / 아이디어 생성 / 검토 작업)
     ├─ CFO layer
     │   ├─ #cfo-general   (예산 / 비용 / 재무 판단)
     │   └─ #cfo-exec      (집계 / 분석 / 초안 작업)
     ├─ CTO layer
     │   ├─ #cto-general   (기술 판단 / 승인 / 우선순위)
     │   └─ #cto-exec      (구현 / 실험 / 디버깅)
     └─ COO layer
         ├─ #coo-general   (운영 정책 / 루틴 / 승인)
         └─ #coo-exec      (프로세스 초안 / 운영 실행)
```

---

## 채널 구조

## 1) 총괄 채널

### #chief-of-staff
역할:
- 전체 요청 intake
- 라우팅
- 부문 간 조율
- 최종 결론 통합

주 발화자:
- 리
- 필요 시 각 C레벨

원칙:
- 메인 프론트도어
- 오너는 여기서 그냥 요청하면 됨
- 최종 synthesis는 기본적으로 리가 담당
- 병렬 작업이 필요한 경우 리가 orchestration 사실을 명시

### #fo-lounge
역할:
- 전체 공용 브리핑
- cross-functional 자유 논의
- 초기 아이디어 탐색
- 가벼운 전체 토론
- 상태 업데이트

주 발화자:
- 리
- C레벨
- 필요 시 execution layer의 요약 참여

원칙:
- 공용 라운지이자 discussion lane 성격
- 여러 부문이 가볍게 의견을 주고받는 공간으로 사용 가능
- 아직 owner가 확정되지 않은 아이디어를 탐색하기 좋음
- 다만 최종 승인 장소는 아님
- 실행 로그를 길게 쌓는 곳도 아님
- 논의가 과업이 되면 chief-of-staff 또는 해당 general / exec로 넘김

## 2) 부문별 general / exec 쌍

### #cio-general
역할:
- 투자 전략 판단
- 기회 / 리스크 검토
- 우선순위 결정
- 승인 / 반려

### #cio-exec
역할:
- 시장 조사
- 투자 후보 생성
- 데이터 정리
- 초안 리서치

### #cfo-general
역할:
- 예산 판단
- 비용 승인
- 재무 리스크 검토
- 보고서 승인

### #cfo-exec
역할:
- 지출 정리
- 현금흐름 집계
- 재무 초안 작성
- 수치 검증

### #cto-general
역할:
- 기술 방향성 판단
- 구현 우선순위 결정
- 아키텍처 승인
- 기술 리스크 검토

### #cto-exec
역할:
- 구현
- 디버깅
- 실험
- 인프라 작업
- 기술 초안 작성

### #coo-general
역할:
- 운영 정책 판단
- 루틴 승인
- 프로세스 설계 승인
- 운영 리스크 판단

### #coo-exec
역할:
- 체크리스트 작성
- 루틴 초안
- 운영 절차 정리
- 실행 관리 작업

---

## 역할과 책임

### 리 (Chief of Staff / Supervisor)
리의 책임:
- 모든 요청의 1차 triage
- 어느 부문이 맡을지 결정
- 다부문 작업이면 fan-out 설계
- 각 부문 결과 fan-in
- 최종 메시지 정리
- 오너와 조직 전체의 컨텍스트 유지

리의 기본 동작:
- 단순 요청은 바로 답함
- 단일 부문 요청은 해당 C레벨로 전달
- 복합 요청은 병렬 분해 후 취합
- 충돌 시 우선순위를 정리함
- final owner가 불명확하면 일단 리가 책임지고 정리함

### CIO / CFO / CTO / COO (Governance Layer)
각 C레벨의 공통 책임:
- 자신의 general 채널에 대한 owner
- exec 채널에 작업을 배정
- 결과물 검토 및 승인 / 반려
- 기준과 정책 정의
- 최종 책임 보유

중요 원칙:
- C레벨은 직접 실무 로그를 길게 생산하지 않는다.
- C레벨은 관리감독과 validation에 집중한다.
- 필요한 경우 교차 검토를 요청할 수 있다.

### Execution Layer
실행 유닛은 상시 고정된 인격일 수도 있고, task-driven worker일 수도 있다.
중요한 건 “exec 채널에서 visible execution을 수행한다”는 점이다.

대표 유형:
- researcher
- analyst
- builder
- operator
- reviewer
- scribe

실행 유닛의 공통 책임:
- 자료 수집
- 초안 생성
- 구현
- 검토 의견 제시
- 상태 보고
- checkpoint 남기기

Managed Agents 관점에서 실행 유닛은 사실상 managed task runner다.
즉 단순히 말만 하는 봇이 아니라 아래를 충족해야 한다.

- 명시된 outcome / success criteria를 기준으로 움직임
- 작은 작업이 아니면 explore → plan → implement를 따름
- verification 방법을 알고 있거나 요청받음
- 길게 가는 작업을 이어서 수행 가능함
- 중간 산출물과 상태를 남김
- 필요 시 병렬로 실행 가능함
- 누가 무엇을 했는지 추적 가능함

원칙:
- exec 채널 중심으로 발화
- general 채널에 직접 난입하지 않음
- 결과는 구조적으로, 짧고 명확하게 보고
- 중간 상태와 완료 상태를 구분해서 남김
- 승인권을 행사하지 않는다.

---

## Generate / Validate / Finalize 파이프라인
이 FO의 기본 운영 패턴은 다음과 같다.

### 기본 파이프라인
1. 리 또는 C레벨이 과업 정의
2. 필요 시 explore / clarify 수행
3. exec 채널에서 generate 수행
4. 결과를 general 채널에 요약 제출
5. C레벨이 validate
6. 리 또는 C레벨이 finalize
7. 필요 시 publish

### Generate 단계
Generate 단계에서 수행되는 일:
- 리서치
- 옵션 생성
- 초안 문서 작성
- 구현 / 실험
- 수치 집계
- 비교안 정리

이 단계는 주로 exec 채널이 담당한다.

### Validate 단계
Validate 단계에서 수행되는 일:
- 사실 검증
- 리스크 검토
- 기준 충족 여부 확인
- 수정 요청
- 승인 / 반려
- 필요 시 추가 조사 요청

이 단계는 주로 general 채널의 C레벨이 담당한다.

### Finalize 단계
Finalize는 최종 소유자가 결론을 내는 단계다.

- 단일 부문 과제 → 해당 C레벨
- 다부문 과제 → 리

Finalize 산출물은 보통 다음 중 하나다.
- 최종 권고안
- 승인/반려 결정
- 게시용 요약
- 다음 액션 명세

---

## 메시지 라우팅 규칙

### Rule 1. 프론트도어는 리
다음 채널은 리가 기본 triage 한다.
- #chief-of-staff
- Telegram DM

`#fo-lounge`도 리가 흐름을 정리할 수 있지만,
기본 역할은 front door라기보다 cross-functional discussion lane에 가깝다.

### Rule 2. 부문 채널은 직통 ownership
- #cio-general → CIO owner
- #cfo-general → CFO owner
- #cto-general → CTO owner
- #coo-general → COO owner

### Rule 3. exec 채널은 generate 공간
- #cio-exec → 투자 조사 / 아이디어 생성
- #cfo-exec → 재무 집계 / 분석 초안
- #cto-exec → 구현 / 실험 / 디버깅
- #coo-exec → 운영 절차 / 루틴 초안

### Rule 4. general은 validate 공간
exec에서 만든 결과물은 general에서 검토한다.
즉 general은 “판단의 장소”다.

### Rule 5. 최종 응답은 single-owner
병렬 작업을 했더라도 최종결론은 리 또는 담당 C레벨 한 명이 올린다.

### Rule 6. 긴 로그는 요약해서 올린다
exec의 탐색 로그, 구현 세부, 디버깅 기록은 그대로 general로 복사하지 않는다.
요약, 리스크, 결정 포인트만 handoff 한다.

---

## 발화 정책

### 공통 원칙
- Slack에서는 과잉 발화 금지
- 도구 로그, 장황한 내부 과정 노출 금지
- 한 요청에는 가능한 한 대표자 한 명만 최종 답변
- 실행은 보이게 하되, 노이즈는 억제
- 자유 논의는 허용하되, owner / approval / execution responsibility는 흐리지 않음
- 불확실하면 불확실성을 명시
- 검증이 안 되었으면 완료처럼 말하지 않음

### 리의 발화 규칙
- #chief-of-staff 에서는 리가 기본 응답권을 가진다.
- 다부문 과제는 리가 orchestration을 명시한다.
- 다른 부문이 참여하더라도 리가 최종 정리한다.
- 진행 중 상태는 짧고 운영적으로 알린다.

### C레벨의 발화 규칙
- 자기 general 채널에서는 직접 응답한다.
- 호출받은 경우 총괄 채널에서도 응답 가능하다.
- 호출받지 않았으면 끼어들지 않는다.
- exec 채널의 세부 로그를 general로 복붙하지 않는다. 요약만 올린다.
- 승인 / 반려 / 추가 요청은 명시적이어야 한다.

### Execution layer의 발화 규칙
- 주 발화 공간은 exec 채널이다.
- 상태 / 결과 중심으로 짧게 보고한다.
- general 채널에는 직접 결론을 내리지 않는다.
- 승인권을 행사하지 않는다.
- 체크포인트와 최종 handoff는 정해진 형식을 따른다.

---

## 장기 작업 운영 규칙

### 장기 작업이 필요한 경우
- 하루 이상 이어질 수 있는 작업
- 구현 / 문서 / 분석 범위가 큰 작업
- 여러 번의 검토와 수정이 필요한 작업
- 병렬 하위 과업이 필요한 작업

### 장기 작업의 필수 요소
모든 장기 작업은 최소한 아래를 가져야 한다.
- task id 또는 식별 가능한 이름
- owner
- assigned by
- outcome
- success criteria
- constraints
- 현재 상태
- 마지막 checkpoint
- 다음 액션

### 상태 모델
권장 상태:
- `queued`
- `exploring`
- `planning`
- `in_progress`
- `awaiting_validation`
- `blocked`
- `done`
- `cancelled`

### resume 원칙
resume 시에는 전체 대화를 다시 길게 풀지 않는다.
아래만 복구하면 된다.
- 현재 목표
- 마지막 checkpoint
- 남은 이슈
- 다음 액션
- 필요한 의사결정 포인트

---

## Validation 기준
validate는 “좋아 보여요”가 아니라,
결정 가능한 수준의 검토여야 한다.

### 공통 검토 항목
- 요청한 outcome에 맞는가
- 사실/수치/구현이 검증 가능한가
- 핵심 리스크가 드러났는가
- 추가 작업 없이 게시 가능한가
- 수정 요청이 필요한가

### 부문별 기본 관점
- **CIO**: upside / downside / uncertainty / concentration risk
- **CFO**: cost / budget impact / cash timing / reversibility
- **CTO**: complexity / failure modes / maintenance burden / testability
- **COO**: repeatability / operational load / ambiguity / handoff quality

세부 기준은 별도 문서(`VALIDATION_RUBRIC.md`)로 관리한다.

---

## 예시 시나리오

### 시나리오 A — 투자 아이디어 검토
1. 화가 #chief-of-staff 에 요청
2. 리가 CIO 과제로 라우팅
3. #cio-exec 에서 시장 조사 / 후보 생성
4. 결과를 #cio-general 에 요약 제출
5. CIO가 validate
6. 리가 전체 맥락에 맞게 최종 정리 가능

### 시나리오 B — 기능 개발 + 비용 검토
1. 화가 #chief-of-staff 에 기능 요청
2. 리가 CTO + CFO 병렬 fan-out
3. #cto-exec 에서 구현 초안 / 난이도 정리
4. #cfo-exec 에서 비용 영향 분석
5. #cto-general, #cfo-general 에서 validate
6. 리가 최종 선택지와 권고안 정리

### 시나리오 C — 운영 루틴 설계
1. 화가 #coo-general 또는 #chief-of-staff 에 요청
2. #coo-exec 에서 루틴 / 체크리스트 초안 생성
3. #coo-general 에서 COO validate
4. 필요 시 CTO / CFO 에 교차 검토 요청
5. 리가 전체 루틴으로 묶어 최종 제안

### 시나리오 D — 장기 제품/시스템 구축
1. 화가 #chief-of-staff 에 큰 목표를 요청
2. 리가 CTO 또는 다부문 과제로 상위 outcome 정의
3. general에서 success criteria와 constraints 확정
4. exec에서 explore → plan → implement 순으로 진행
5. checkpoint를 남기며 재개 가능 상태 유지
6. general에서 단계별 validate
7. 리가 전체 상태와 다음 액션을 정리

---

## 초기 운영 버전 (v1)
현재는 채널 구조를 기준으로 작게 시작한다.

### 현재 운영 채널
- #chief-of-staff
- #fo-lounge
- #cio-general
- #cio-exec
- #cfo-general
- #cfo-exec
- #cto-general
- #cto-exec
- #coo-general
- #coo-exec

### v1 운영 목표
- general / exec 역할 분리를 습관화
- 리의 routing / synthesis 안정화
- C레벨의 감독 역할 정착
- visible execution이 과소음 없이 작동하는지 검증
- managed task template 사용 습관 정착
- validation 기준을 명시적으로 적용

---

## 다음 구현 단계
다음 구현 단계는 아래 순서로 진행한다.

1. 각 채널의 welcome / operating prompt 메시지 작성
2. 리 / CIO / CFO / CTO / COO 역할 프롬프트 정비
3. execution layer 역할군 정의
4. 각 exec 채널에서 사용할 managed task template 적용
5. validation rubric을 운영 문장으로 정리
6. Slack 상의 발화 규칙 테스트
7. chief-of-staff 채널에서 자연어 직답 흐름 안정화
8. DM thread / direct response 동작 보정
9. 장기 작업 / 병렬 작업 상태 추적 방식 정리
10. publish workflow와 daily report routine 고정

---

## 결론
이 Family Office는 Harness 기반의 Slack 조직으로 운영된다.
하지만 목표는 “복잡한 멀티에이전트 데모”가 아니다.

목표는 다음과 같다.
- 리가 앞에서 받고
- C레벨이 관리감독하며
- 실행은 visible execution으로 드러나고
- Generate / Validate / Finalize를 통해 품질을 높이며
- 검증 가능성과 재개 가능성을 확보하고
- 최종 결론은 한 명이 책임지는
실용적인 운영체계를 만드는 것.
