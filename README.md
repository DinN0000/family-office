# Family Office

이 저장소는 Slack-first Family Office 구현을 위한 설계/문서 저장소다.

현재 기준 핵심 문서는 아래 하나다.
- `projects/family-office/DESIGN.md`

## 현재 방향

이 Family Office는 다음 원칙으로 구현한다.

- 리 = front door / supervisor / single intake
- CIO / CFO / CTO / COO = governance layer
- 실행은 visible execution layer에서 분리
- 중요한 결과물은 generate 이후 validate 단계를 거쳐 게시
- 최종 사용자-facing 결론은 가능한 한 single-owner voice로 정리
- Slack에서는 스레드 강제를 최소화하고 채널 본문 중심으로 운영

## 현재 Slack 구조

기준 채널:
- `#chief-of-staff` → `C0AR0DCSP7F`

생성된 Family Office 채널:
- `#fo-lounge` → `C0ASK6209BJ`
- `#cio-general` → `C0ARMFEMYE9`
- `#cfo-general` → `C0ARUFFQERJ`
- `#cto-general` → `C0ARNRHKSQ6`
- `#coo-general` → `C0ARUF7TJPN`

정리된 채널:
- `이씨-가문-경영` → archived

## DESIGN.md 작업에 중요한 포인트

`projects/family-office/DESIGN.md`에서 계속 다듬어야 할 핵심은 아래다.

1. Slack-first 구조를 전제로 문서화할 것
2. 리 / C레벨 / 실행 레이어의 역할을 분명히 나눌 것
3. general / exec 채널 분리 원칙을 정의할 것
4. validate 단계와 single-owner 게시 규칙을 적을 것
5. daily report, long-running work, checkpoint/resume 흐름을 포함할 것
6. 사용자의 Slack 선호(스레드 강제 지양, 짧고 자연스러운 응답)를 운영 원칙에 반영할 것

## 다음 문서화 후보

- channel map
- role speaking policy
- exec lane 운영 규칙
- validate / publish workflow
- daily report routine
