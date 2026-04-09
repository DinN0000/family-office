# Validation Rubric

이 문서는 general 채널에서 validate할 때 최소한 어떤 기준으로 승인 / 반려 / 수정 요청을 할지 정리한다.

validate는 단순 감상이 아니라 **결정 행위**다.

---

## 공통 질문

모든 부문은 최소한 아래를 본다.

1. 요청한 outcome이 실제로 충족되었는가?
2. 결과가 검증 가능하거나 근거가 충분한가?
3. 중요한 리스크와 불확실성이 드러나 있는가?
4. 다음 액션이 명확한가?
5. 지금 게시 / 실행 / 승인해도 되는가?

---

## Verdict Types

권장 verdict는 아래 4개 중 하나다.

- **Approved**: 이대로 진행 또는 게시 가능
- **Approved with caveats**: 진행 가능하지만 주의사항 명시 필요
- **Needs revision**: 수정 후 재검토 필요
- **Rejected**: 현재 방향으로는 진행 불가

---

## CIO Validation

### 핵심 질문
- upside가 무엇인가?
- downside가 무엇인가?
- 확신 수준은 어느 정도인가?
- 리스크가 특정 가정에 과도하게 집중되어 있는가?
- 대안 대비 매력도가 있는가?

### 체크포인트
- 기회 요인이 명확한가
- 리스크가 숨겨져 있지 않은가
- 불확실성이 수치/서술로 표현되었는가
- 비교 가능한 대안이 있는가

---

## CFO Validation

### 핵심 질문
- 비용 규모와 타이밍이 명확한가?
- 예산 영향이 설명되었는가?
- 되돌릴 수 있는가?
- 고정비 / 변동비 / 일회성 비용이 구분되었는가?
- 현금흐름 부담이 감당 가능한가?

### 체크포인트
- 비용 추정 근거가 있는가
- budget impact가 명시되었는가
- cash timing이 드러나는가
- reversibility가 평가되었는가

---

## CTO Validation

### 핵심 질문
- 구현 복잡도가 적절히 설명되었는가?
- 실패 모드가 드러났는가?
- 유지보수 부담이 감당 가능한가?
- 테스트 또는 검증 경로가 있는가?
- 기존 구조와 충돌하지 않는가?

### 체크포인트
- 아키텍처 영향이 설명되었는가
- known risk / failure mode가 있는가
- testability가 있는가
- 운영/배포 후 부담이 설명되었는가

---

## COO Validation

### 핵심 질문
- 반복 가능한 운영 절차인가?
- 사람이나 시스템이 지속적으로 감당 가능한가?
- 모호한 handoff가 남아 있지 않은가?
- 실제 운영 시 누가 무엇을 하는지 명확한가?
- 예외 상황 대응이 있는가?

### 체크포인트
- 절차가 재현 가능한가
- 운영 부하가 적절한가
- handoff가 명확한가
- ambiguity가 줄어들었는가

---

## Validation Response Format

권장 형식:

- **Verdict**:
- **Why**:
- **What is verified**:
- **What remains risky / uncertain**:
- **Required changes**:
- **Next action**:

---

## Example

- **Verdict**: Needs revision
- **Why**: 방향은 맞지만 CTO 관점에서 failure mode와 test path가 빠져 있음
- **What is verified**: 요구한 기능 범위와 구현 우선순위 초안은 정리됨
- **What remains risky / uncertain**: Slack direct response race condition 원인 확인 안 됨
- **Required changes**: known failure mode 2~3개와 검증 경로 추가
- **Next action**: #cto-exec에서 수정 후 재제출
