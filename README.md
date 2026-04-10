# Family Office

이 저장소는 Slack-first multi-agent product org를 위한 설계 문서 저장소다.

핵심 문서:
- `projects/family-office/DESIGN.md`

## 현재 역할 구조

- 리 = Lead + PO
- FE
- BE
- Designer
- QA

기존에 구성해둔 봇 인스턴스는 재활용하되,
사용자-facing 이름, 역할, 채널 구조는 위 모델로 재편한다.

## 현재 목표 채널 구조

- `#lead`
- `#product-room`
- `#fe`
- `#be`
- `#designer`
- `#qa`
- `#execution-log`
- `#shipping`

## 운영 원칙

- 리가 front door 역할을 맡는다.
- 실행은 보이되 긴 로그는 분리한다.
- generate → validate → publish 흐름을 기본으로 쓴다.
- 최종 사용자-facing 결론은 가능한 한 single-owner voice로 정리한다.
- Slack에서는 스레드 강제를 최소화하고 채널 본문 중심으로 운영한다.
