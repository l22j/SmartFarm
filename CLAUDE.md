# SmartFarm — Claude Code 작업 지침

## PPT / 슬라이드 제작 요청이 오면

문서(hwp/pdf/텍스트 등)를 주고 PPT·슬라이드·발표자료를 만들어달라는 요청이 오면:

1. 절대 혼자 인라인으로 요약·설계·문구작성·pptx제작을 다 하지 말고, 반드시 아래 4개 서브에이전트에게
   **이 순서 그대로** 위임한다 (각 단계 출력을 다음 단계 입력으로 전달):
   `report-reader` → `structure-planner` → `slide-writer` → `ppt-builder`
   - 정의: `.claude/agents/report-reader.md`, `structure-planner.md`, `slide-writer.md`, `ppt-builder.md`
   - 한 번에 자동 실행하려면 `.claude/commands/make-slides.md`(= `/make-slides`)를 그대로 따라도 됨
2. 각 서브에이전트를 호출하기 직전에 아래 "에이전트 픽셀 오피스" DB에 해당 에이전트의 상태를 갱신하고,
   호출이 끝나면 `done`으로 다시 갱신한다. (사용자가 실시간 진행 상황을 그 페이지에서 보고 싶어 함)
3. 최종 산출물(.pptx)은 완성 후 SendUserFile로 사용자에게 전달한다.

## 에이전트 픽셀 오피스 (실시간 시각화 대시보드)

두 버전이 있고, **아티팩트마다 db가 완전히 분리된 별도 저장소**라서 상태를 반영하려면
**두 URL 모두에 각각 write_db 해야 한다** (하나만 갱신하면 다른 쪽은 안 바뀜):

- 전체 버전 (설명 + 파이프라인 표시줄 + 활동 로그): https://claude.ai/code/artifact/49e27c43-71cb-488a-812f-b9edd4b2b971
- 미니 버전 (그림만, 설명 없음): https://claude.ai/code/artifact/eee767d7-bbd6-4153-aef9-3fd8c619b889
- 자세한 설명/문서: `docs/pixel-office.md`

claude.ai Artifact의 `db`(실시간 공유 문서 DB) 기능으로 동작하는, 4개 서브에이전트를 픽셀 아트
캐릭터로 시각화하는 페이지들. 이미 게시되어 있으므로 **새로 만들지 말고 이 URL들을 계속 재사용**한다
(Artifact 도구에 `url` 파라미터로 해당 주소를 넘겨서 `write_db`/재게시).

DB 스키마 (두 아티팩트 공통):
- `agents/report-reader`, `agents/structure-planner`, `agents/slide-writer`, `agents/ppt-builder`
  각 문서: `{ state, label, updatedAt }`
  - `state`: `idle | thinking | reading | typing | bash | waiting | done` 중 하나
  - 서브에이전트 호출 직전 = 진행 상태(reading/typing/thinking/bash 등)로 update,
    호출 완료 직후 = `done`으로 update. `label`은 사람이 읽을 짧은 한국어 설명.
- `activity/feed` 문서 (전체 버전에서만 표시됨): `{ stage, artifact, log: [{t, agent, icon, text}, ...] }`
  - `log`는 최신 항목이 배열 끝에 오도록 append하고 최근 30~40개만 유지(용량 제한 때문에 통 문서 하나에 배열로 관리).

페이지 자체(HTML)는 `.claude/agents/` 정의와 무관하게 독립적으로 존재하므로, 코드를 다시 안 짜도
DB만 갱신하면 시각화가 살아있다.

## 재사용 관련 참고

- 위 서브에이전트/커맨드 파일들은 이 저장소에 커밋되어 있어 이 저장소를 여는 세션이면 자동 로드된다.
- 현재는 `claude/clever-cori-mmvmwu` 브랜치에 있음 — `main`에도 항상 자동으로 있게 하려면 PR 머지가 필요하다.
