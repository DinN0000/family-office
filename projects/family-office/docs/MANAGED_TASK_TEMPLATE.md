# Managed Task Template

exec 채널 작업은 가능한 한 아래 형식으로 관리한다.

이 문서의 목적은 작업을 “그때그때 말로 시키는 것”에서
**검증 가능하고 재개 가능한 단위 작업**으로 바꾸는 것이다.

---

## Task Header

- **Task**:
- **Task ID**:
- **Owner**:
- **Assigned by**:
- **Channel**:
- **Priority**:
- **Status**:

---

## Outcome Definition

- **Goal / Outcome**:
- **Why this matters**:
- **In scope**:
- **Out of scope**:

---

## Success Criteria

- [ ] 요구한 산출물이 존재함
- [ ] 핵심 품질 기준을 충족함
- [ ] validation 가능한 근거가 있음
- [ ] 필요한 handoff 정보가 포함됨

추가 성공 기준:
- 
- 

---

## Constraints

- 시간 제약:
- 도구 / 권한 제약:
- 형식 제약:
- 정책 / 리스크 제약:

---

## Dependencies

- 선행 결정:
- 필요한 입력 자료:
- 관련 채널 / 관련 owner:

---

## Working Plan

### 1. Explore
- 확인할 것:
- 수집할 정보:
- 모르는 점:

### 2. Plan
- 접근 방식:
- 대안:
- 선택 이유:

### 3. Execute
- 실행 단계:
- 중간 산출물:

### 4. Verify
- 테스트 / 검증 방법:
- 비교 기준:
- 실패 조건:

---

## Checkpoint Format

checkpoint는 아래 형식으로 남긴다.

- **Status**:
- **What changed since last update**:
- **Current findings / outputs**:
- **Open questions / blockers**:
- **Next step**:
- **Needs validation?** yes / no

---

## Final Handoff Format

완료 또는 validate 요청 시 아래 형식을 사용한다.

- **Task**:
- **Outcome delivered**:
- **What was verified**:
- **What remains uncertain**:
- **Risks / caveats**:
- **Recommended decision**:
- **Recommended next action**:

---

## Minimal Example

- **Task**: CTO lane에서 Slack bot direct response 흐름 정리
- **Task ID**: CTO-2026-04-FO-001
- **Owner**: CTO
- **Assigned by**: 리
- **Channel**: #cto-exec
- **Priority**: high
- **Status**: in_progress

### Outcome Definition
- **Goal / Outcome**: direct response 흐름과 known bug를 정리한 운영 초안 작성
- **In scope**: 현재 동작 정리, 문제 구간 식별, 수정 우선순위 제안
- **Out of scope**: 실제 코드 수정

### Success Criteria
- [x] 현재 흐름 설명
- [x] known bug 후보 정리
- [x] 우선순위 제안
- [ ] validation 대기

### Checkpoint
- **Status**: awaiting_validation
- **What changed since last update**: 현재 흐름 문서화 완료
- **Current findings / outputs**: direct response 경로 3개와 bug 후보 2개 정리
- **Open questions / blockers**: Slack side event ordering 확인 필요
- **Next step**: CTO validate
- **Needs validation?** yes
