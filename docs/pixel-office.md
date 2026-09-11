# 에이전트 픽셀 오피스

이 세션에서 만든 4개의 서브에이전트(`report-reader` → `structure-planner` → `slide-writer` → `ppt-builder`)가
호출되면 문에서 소환되어 각자 책상으로 걸어가 앉고, 지금 무엇을 하고 있는지 픽셀 아트로 보여줍니다.

실제 [pixel-agents](https://github.com/pixel-agents-hq/pixel-agents)는 로컬 훅으로 자동 계측되지만,
이 페이지는 원격 세션에서 동작하도록 제(오케스트레이터)가 각 서브에이전트를 호출할 때마다 직접
claude.ai Artifact의 실시간 db에 상태를 기록하는 방식입니다.

## 링크

- 전체 버전 (설명 + 파이프라인 표시줄 + 활동 로그 포함): https://claude.ai/code/artifact/49e27c43-71cb-488a-812f-b9edd4b2b971
- 미니 버전 (그림만, 설명 없음): 아래 "미니 버전" 참고

## DB 스키마 (두 버전이 같은 데이터를 구독함)

- `agents/{report-reader|structure-planner|slide-writer|ppt-builder}`: `{ state, label, updatedAt }`
  - `state`: `idle | thinking | reading | typing | bash | waiting | done`
- `activity/feed`: `{ stage, artifact, log: [{t, agent, icon, text}, ...] }`

갱신 규칙과 재사용 방법은 `CLAUDE.md`에 있습니다.
