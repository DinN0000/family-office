# 패밀리 오피스 (FO) — Slack Harness 설계 문서

## 개요
이 Family Office는 두 가지 레퍼런스를 합쳐 설계한다.
- revfactory Harness: 조직 구조와 역할 분리 프레임
- Claude Managed Agents: 실행 런타임과 장기 작업 운영 철학

즉, Harness는 “누가 어떤 역할로 협업하는가”를 정의하고,
Managed Agents는 “그 협업을 어떤 방식으로 안정적으로 실행하는가”를 정의한다.

핵심 목표는 네 가지다.
- 리가 프론트도어가 되는 총괄 운영 체계
- C레벨은 관리감독과 판단에 집중하는 구조
- 실행은 숨기지 않되, exec 채널로 분리해 보이게 운영하는 구조
- 장기 작업, 병렬 작업, 추적 가능한 작업 실행을 기본 전제로 두는 구조

즉 이 시스템은 “여러 봇이 같은 방에서 동시에 떠드는 구성”이 아니다.
대신 아래 원칙으로 움직인다.
- 리가 intake, routing, synthesis를 담당한다.
- CIO/CFO/CTO/COO는 governance layer로서 판단과 승인에 집중한다.
- 실행 유닛은 exec 채널에서 generate 작업을 수행한다.
- 중요한 결과물은 validate 단계를 반드시 거친다.
- 최종 결론은 가능한 한 single-owner 방식으로 게시한다.
- 복합 과제는 장기 세션, 체크포인트, 실행 추적을 고려한 managed execution으로 다룬다.

## 참조 모델

### Harness에서 가져오는 것
- Supervisor 중심 구조
- Specialist Pool
- Worker / execution layer
- Generate / Validate
- Fan-out / Fan-in
- 역할별 발화 규칙
- 채널 기반 운영 설계

### Claude Managed Agents에서 가져오는 것
- outcome 중심 task execution
- long-running sessions
- checkpoint / resumability
- scoped permissions / governance
- execution tracing / observability
- 병렬 태스크 오케스트레이션
- 모델/도구 업그레이드에 덜 취약한 실행 루프 추상화

### 결론
이 FO는 “Harness로 조직을 설계하고, Managed Agents 방식으로 실행 레이어를 운영하는 Slack Family Office”로 간다.

## Harness 매핑
이 설계는 Harness 개념을 FO 조직에 다음처럼 매핑한다.

- Supervisor → 리 (Chief of Staff)
- Specialist Pool → CIO / CFO / CTO / COO
- Worker / Execution Layer → exec 채널의 visible execution agents
- Generate → exec 채널에서 조사, 초안, 구현, 분석 수행
- Validate → general 채널에서 C레벨 검토, 승인, 반려
- Fan-out / Fan-in → 복합 과제를 병렬 분해하고 리가 최종 취합
- Hierarchical Delegation → 큰 과업을 C레벨이 실행 유닛으로 쪼개 위임

## Managed Execution 매핑
Claude Managed Agents 관점에서 FO 실행 레이어는 다음처럼 해석한다.

- Outcome definition → 리 또는 C레벨이 “무엇이 성공인지” 먼저 정의
- Managed task runner → exec 채널의 실행 유닛이 작업을 수행
- Long-running session → 길게 가는 과제는 세션 단위로 이어서 수행
- Checkpoint / resumability → 중간 산출물과 상태를 남기고 재개 가능하게 설계
- Scoped permissions → 각 역할은 필요한 범위의 도구와 권한만 사용
- Execution tracing → 누가 무엇을 생성/검토/승인했는지 추적 가능해야 함
- Multi-agent coordination → 병렬 분석, 병렬 구현, 교차 검토를 구조적으로 허용

즉 FO는 “Harness를 Slack Family Office 조직 운영체계로 번역하고, Managed Agents 철학으로 실행 레이어를 안정화한 형태”로 간다.

## 설계 원칙

### 1. 리는 프론트도어다
오너가 어디로 말해야 할지 고민하지 않도록, 기본적으로 리가 먼저 받는다.
리의 역할은 다음과 같다.
- 요청 해석
- 담당 부문 지정
- 다부문 과제 오케스트레이션
- 우선순위 충돌 조정
- 최종 보고 편집

### 2. C레벨은 실무자가 아니라 감독자다
CIO/CFO/CTO/COO는 직접 모든 일을 수행하는 봇이 아니다.
이들은 다음 역할을 맡는다.
- 업무 배정
- 품질 기준 제시
- 리스크 판단
- 승인 / 반려
- 최종 책임

즉 C레벨은 관리감독, validation, ownership 레이어다.

### 3. 실행은 보이게 드러내되, managed execution으로 다룬다
내부 실행은 완전히 숨기지 않는다.
다만 general 채널을 더럽히지 않기 위해 exec 채널로 분리한다.
이로써 다음을 동시에 만족한다.
- 블랙박스처럼 안 보이지 않음
- 실행 과정이 추적 가능함
- 메인 판단 채널은 소음이 줄어듦
- 장기 과제와 병렬 과제를 안정적으로 이어갈 수 있음

exec 채널은 단순한 잡담방이 아니라 managed execution 레이어다.
즉 이 레이어는 다음 속성을 가져야 한다.
- 과업 단위로 명확한 목표를 받음
- 중간 상태를 남김
- 필요 시 이어서 재개 가능함
- 누가 어떤 결과를 냈는지 추적 가능함

### 4. Generate / Validate를 기본 파이프라인으로 쓴다
중요한 결과물은 “한 번 생성하고 끝”이 아니라 다음 순서를 따른다.
- Generate: 초안, 조사, 구현, 분석
- Validate: 검토, 리스크 평가, 승인 / 반려
- Finalize: 리 또는 해당 부문장이 최종 정리

### 5. 최종 결론은 한 명이 낸다
병렬 분석은 가능하지만, Slack에서 여러 에이전트가 동시에 최종결론을 말하면 피곤해진다.
따라서 final owner를 명확히 둔다.
- 전체 과제면 리
- 단일 부문 과제면 해당 C레벨

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
     │   ├─ #cfo-general
     │   └─ #cfo-exec
     ├─ CTO layer
     │   ├─ #cto-general
     │   └─ #cto-exec
     └─ COO layer
         ├─ #coo-general
         └─ #coo-exec
```

## 채널 구조

### 1) 총괄 채널

#### #chief-of-staff
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

#### #fo-lounge
역할:
- 전체 공용 브리핑
- 주요 결과 공유
- 가벼운 전체 논의
- 상태 업데이트

주 발화자:
- 리
- C레벨
- 필요 시 실행 결과의 요약본

원칙:
- 공용 라운지 성격
- 실행 로그를 길게 쌓는 곳은 아님

### 2) 부문별 general / exec 쌍

#### #cio-general
역할:
- 투자 전략 판단
- 기회 / 리스크 검토
- 우선순위 결정
- 승인 / 반려

#### #cio-exec
역할:
- 시장 조사
- 투자 후보 생성
- 데이터 정리
- 초안 리서치

#### #cfo-general
역할:
- 예산 판단
- 비용 승인
- 재무 리스크 검토
- 보고서 승인

#### #cfo-exec
역할:
- 지출 정리
- 현금흐름 집계
- 재무 초안 작성
- 수치 검증

#### #cto-general
역할:
- 기술 방향성 판단
- 구현 우선순위 결정
- 아키텍처 승인
- 기술 리스크 검토

#### #cto-exec
역할:
- 구현
- 디버깅
- 실험
- 인프라 작업
- 기술 초안 작성

#### #coo-general
역할:
- 운영 정책 판단
- 루틴 승인
- 프로세스 설계 승인
- 운영 리스크 판단

#### #coo-exec
역할:
- 체크리스트 작성
- 루틴 초안
- 운영 절차 정리
- 실행 관리 작업

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
- 충돌 시 우선순위를 강하게 정리함

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

Managed Agents 관점에서 실행 유닛은 사실상 managed task runner다.
즉 단순히 말만 하는 봇이 아니라 아래를 충족해야 한다.
- 명시된 outcome / success criteria를 기준으로 움직임
- 길게 가는 작업을 이어서 수행 가능함
- 중간 산출물과 상태를 남김
- 필요 시 병렬로 실행 가능함
- 누가 무엇을 했는지 추적 가능함

원칙:
- exec 채널 중심으로 발화
- general 채널에 직접 난입하지 않음
- 결과는 구조적으로, 짧고 명확하게 보고
- 중간 상태와 완료 상태를 구분해서 남김

## Generate / Validate 파이프라인
이 FO의 기본 운영 패턴은 다음과 같다.

### 기본 파이프라인
1. 리 또는 C레벨이 과업 정의
2. exec 채널에서 generate 수행
3. 결과를 general 채널에 요약 제출
4. C레벨이 validate
5. 리 또는 C레벨이 finalize

### Generate 단계
Generate 단계에서 수행되는 일:
- 리서치
- 옵션 생성
- 초안 문서 작성
- 구현 / 실험
- 수치 집계

이 단계는 주로 exec 채널이 담당한다.

### Validate 단계
Validate 단계에서 수행되는 일:
- 사실 검증
- 리스크 검토
- 기준 충족 여부 확인
- 수정 요청
- 승인 / 반려

이 단계는 주로 general 채널의 C레벨이 담당한다.

### Finalize 단계
Finalize는 최종 소유자가 결론을 내는 단계다.
- 단일 부문 과제 → 해당 C레벨
- 다부문 과제 → 리

## Fan-out / Fan-in 규칙
복합 과제는 병렬 분해 후 취합한다.

### Fan-out을 쓰는 경우
- 여러 부문이 동시에 관여하는 문제
- 리서치 / 비교 / 병렬 검토가 필요한 문제
- 투자 + 재무 + 기술 + 운영이 동시에 얽힌 문제

예시:
- “새 제품/서비스 런칭 준비”
  - CTO: 제품 / 기술 구현
  - CFO: 비용 / 예산
  - COO: 운영 준비
  - CIO: 시장성 / 기회비용 관점
  - 리: 최종 통합

### Fan-in 방식
- 각 부문 general owner가 자기 부문 관점을 요약
- 리가 충돌과 우선순위를 정리
- 최종안은 한 메시지로 취합

## 발화 정책

### 공통 원칙
- Slack에서는 과잉 발화 금지
- 중간 도구 로그, 장황한 내부 과정 노출 금지
- 한 요청에는 가능한 한 대표자 한 명만 최종 답변
- 실행은 보이게 하되, 노이즈는 억제

### 리의 발화 규칙
- #chief-of-staff 에서는 리가 기본 응답권을 가진다.
- 다부문 과제는 리가 orchestration을 명시한다.
- 다른 부문이 참여하더라도 리가 최종 정리한다.

### C레벨의 발화 규칙
- 자기 general 채널에서는 직접 응답한다.
- 호출받은 경우 총괄 채널에서도 응답 가능하다.
- 호출받지 않았으면 끼어들지 않는다.
- exec 채널의 세부 로그를 general로 복붙하지 않는다. 요약만 올린다.

### Execution layer의 발화 규칙
- 주 발화 공간은 exec 채널이다.
- 상태 / 결과 중심으로 짧게 보고한다.
- general 채널에는 직접 결론을 내리지 않는다.
- 승인권을 행사하지 않는다.

## 메시지 라우팅 규칙

### Rule 1. 프론트도어는 리
다음 채널은 리가 기본 triage 한다.
- #chief-of-staff
- #fo-lounge
- Telegram DM

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

## 초기 운영 버전
v1에서는 현재 채널 구조를 기준으로 작게 시작한다.

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
- 리의 라우팅과 synthesis 안정화
- C레벨의 감독 역할 정착
- visible execution이 과소음 없이 작동하는지 검증

## 다음 단계
다음 구현 단계는 아래 순서로 진행한다.

1. 각 채널의 welcome / operating prompt 메시지 작성
2. 리 / CIO / CFO / CTO / COO 역할 프롬프트 정비
3. execution layer 역할군 정의
4. 각 exec 채널에서 사용할 managed task template 정의
   - outcome
   - success criteria
   - constraints
   - checkpoint format
   - final handoff format
5. Slack 상의 발화 규칙 테스트
6. chief-of-staff 채널에서 자연어 직답 흐름 안정화
7. DM thread 버그 및 Slack direct response 동작 보정
8. 장기 작업 / 병렬 작업 상태 추적 방식 정리

## 결론
이 Family Office는 Harness 기반의 Slack 조직으로 운영된다.
하지만 목표는 “복잡한 멀티에이전트 데모”가 아니다.
목표는 다음과 같다.
- 리가 앞에서 받고
- C레벨이 관리감독하며
- 실행은 visible execution으로 드러나고
- Generate / Validate를 통해 품질을 높이며
- 최종 결론은 한 명이 책임지는
실용적인 운영체계를 만드는 것.
