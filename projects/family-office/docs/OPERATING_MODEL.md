# FO Operating Model

이 문서는 Slack Family Office를 실제 운영할 때의 채널별 동작 원칙을 정리한다.

## 운영 철학

- front door는 단순해야 한다.
- final owner는 항상 명확해야 한다.
- execution은 visible해야 하지만 noisy하면 안 된다.
- validation은 의견 표명이 아니라 승인/반려 행위여야 한다.
- 긴 작업은 checkpoint 가능해야 한다.

---

## 채널 유형 정의

### Front Door
대상:
- `#chief-of-staff`
- Telegram DM to 리

역할:
- intake
- triage
- routing
- orchestration
- synthesis

기본 owner:
- 리

### General Channels
대상:
- `#cio-general`
- `#cfo-general`
- `#cto-general`
- `#coo-general`

역할:
- validate
- approve / reject
- priority / policy / risk judgment
- final departmental ownership

기본 owner:
- 해당 C레벨

### Exec Channels
대상:
- `#cio-exec`
- `#cfo-exec`
- `#cto-exec`
- `#coo-exec`

역할:
- explore
- generate
- implement
- analyze
- draft
- checkpoint

기본 owner:
- 실행 유닛

### Lounge
대상:
- `#fo-lounge`

역할:
- 전체 공유
- 주요 브리핑
- 가벼운 공용 논의
- 중요한 결과의 축약본 게시

---

## 채널별 speaking policy

## #chief-of-staff
해야 하는 것:
- 요청 접수
- 담당 부문 판단
- 다부문이면 fan-out 선언
- 최종 권고안 또는 다음 액션 제시

하지 말아야 하는 것:
- 세부 실행 로그 장문 전개
- 검증 안 된 확정 어조
- C레벨 owner를 무시한 독단적 승인

권장 응답 형식:
- 요청 이해
- 담당 lane
- 필요한 경우 진행 방식
- 다음 업데이트 약속 또는 바로 답변

## #fo-lounge
해야 하는 것:
- 전체 공유 가치가 있는 내용 게시
- 부문별 작업 결과의 짧은 브리핑
- 전체 운영 상태 업데이트

하지 말아야 하는 것:
- exec 로그 dump
- 세부 디버깅 토론

## *-general
해야 하는 것:
- 결과 검토
- 승인 / 반려 / 수정 요청
- 우선순위와 리스크 판단
- 부문 관점의 최종 의견 명시

하지 말아야 하는 것:
- 실무 로그를 직접 길게 생산
- exec에서 해야 할 조사/구현을 general에서 풀로 수행

권장 응답 형식:
- verdict
- 근거
- 남은 리스크
- 다음 액션

## *-exec
해야 하는 것:
- 조사
- 초안 생성
- 구현
- 실험
- 상태 보고
- checkpoint 남기기

하지 말아야 하는 것:
- 최종 승인 선언
- general을 우회한 최종 결론 게시

권장 응답 형식:
- task
- current status
- findings / output
- blockers
- next step

---

## Orchestration rules

### 단일 부문 요청
- 리가 해당 general owner로 라우팅
- 필요 시 general owner가 exec에 작업 위임
- 최종 답변은 해당 C레벨 또는 리가 정리

### 다부문 요청
- 리가 fan-out 선언
- 각 부문은 자기 lane에서 처리
- fan-in은 리가 수행
- 최종 답변은 리가 single-owner로 게시

### 장기 작업
- general에서 outcome / success criteria 확정
- exec에서 checkpoint 기반 진행
- 단계별 validate 후 계속 진행

---

## Noise control rules

- 원문 로그보다 요약을 우선한다.
- general에는 결정에 필요한 정보만 올린다.
- lounge에는 더 축약된 버전만 올린다.
- 같은 내용을 여러 채널에 중복 장문으로 쓰지 않는다.

---

## Publish rules

최종 게시 전 확인:
- 누가 최종 owner인가?
- validation이 끝났는가?
- 미검증 가정이 남아 있는가?
- 게시 대상 채널이 맞는가?
- 후속 액션이 명확한가?
