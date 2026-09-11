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
- `agents/report-reader`, `agents/structure-planner`, `agents/slide-writer`, `agents/ppt-builder`,
  `agents/data-analyst` 각 문서: `{ state, label, updatedAt }`
  - `state`: `idle | thinking | reading | typing | bash | waiting | done` 중 하나
  - 서브에이전트 호출 직전 = 진행 상태(reading/typing/thinking/bash 등)로 update,
    호출 완료 직후 = `done`으로 update. `label`은 사람이 읽을 짧은 한국어 설명.
- `activity/feed` 문서 (전체 버전에서만 표시됨): `{ stage, artifact, log: [{t, agent, icon, text}, ...] }`
  - `log`는 최신 항목이 배열 끝에 오도록 append하고 최근 30~40개만 유지(용량 제한 때문에 통 문서 하나에 배열로 관리).

페이지 자체(HTML)는 `.claude/agents/` 정의와 무관하게 독립적으로 존재하므로, 코드를 다시 안 짜도
DB만 갱신하면 시각화가 살아있다.

`data-analyst`는 make-slides 파이프라인(위 4개)에는 포함되지 않는 별도 서브에이전트다. 데이터/수치
분석 요청이 오면 이 에이전트를 쓰고, 호출 전후로 역시 이 픽셀 오피스 DB를 갱신한다 (정의:
`.claude/agents/data-analyst.md`).

## 캐릭터 스프라이트 (실제 이미지)

**세 번째 개정판.** 이전에 두 차례(정면 데스크형 개별 시트 → 탑뷰 걷기 사이클) 시도했던 에셋은
스타일이 서로 안 맞아서 전부 버리고, **하나의 통합 레퍼런스 시트**에서 5명 전원(+방 배경)을
동일한 톤으로 잘라낸 "카드형" 이미지로 교체했다. 지금 커밋된 게 최종본이다 — 이전 두 스타일로
되돌아가지 말 것.

- 원본 소스: `webview-ui/assets/characters/_source/master-reference-sheet.png`
  (5명 × 8포즈 + 방 배경 + 가구/이펙트가 한 장에 다 들어있는 레퍼런스 시트, 사용자 제공)
- 방 배경: `webview-ui/assets/office/room.png` (같은 시트에서 크롭, 탑뷰 회의실 일러스트를
  배너 이미지로 그대로 사용 — 더 이상 캔버스로 방을 코드로 그리지 않음)
- 캐릭터별 최종 파일: `webview-ui/assets/characters/{agent-id}/{pose}.png`
  - agent-id: `report-reader`(청록) · `structure-planner`(보라) · `slide-writer`(골드) ·
    `ppt-builder`(코랄) · `data-analyst`(블루, 새로 추가)
  - pose: `idle | typing | reading | bash | waiting | thinking | done` (7개, db `state`와 1:1 대응)
  - 각 파일은 레퍼런스 시트에서 그 칸을 크롭(칼럼 폭 93px, `GRID_X0=668, GRID_Y0=58, ROW_H=120.4`)
    → 3배 업스케일 → **둥근 사각 카드**로 마스킹한 것 (배경을 투명 처리하려 했으나 그을음
    아티팩트가 생겨서, 대신 원본 배경을 살린 카드 형태로 타협함 — 이게 지금 기준의 "깔끔한" 버전).
  - **걷는 모션도 있음**: `webview-ui/assets/characters/{agent-id}/walk{0-3}.png` (4프레임 걷기
    사이클, 레퍼런스 시트의 desk 칸들 바로 뒤쪽 `x = GRID_X0+651`부터 폭 `[54,53,53,55]`px로
    크롭). 카드 아래 방 배너(`.room-banner`) 위에 `.walker` `<img>`로 절대 위치시켜, 각 에이전트가
    자기 자리 앞에서 `xMin~xMax`(%) 구간을 좌우로 계속 왕복하는 **상시 배경 애니메이션**이다
    (한 번 걸어 들어오는 입장 연출이 아니라 끊임없는 patrol). `startWalker(a)`가
    `window.SPRITES_WALK[agent-id]`(4장 배열)로 220ms마다 프레임을 바꾸고, 1550ms마다 방향을
    바꾸며 `scaleX(-1)`로 좌우 반전한다. 로스터 카드 쪽 이미지(포즈 7종)는 그대로 `state` 변화에
    따라 pop-in 애니메이션과 함께 교체되는 정적 카드로 유지 — 걷기는 방 배너, 상태 아이콘은
    카드, 이렇게 역할이 분리되어 있다.

이미지를 새로 바꾸거나 포즈를 추가할 때:
1. `webview-ui/assets/characters/{agent}/{pose}.png`, `walk{0-3}.png` (그리고 필요하면
   `webview-ui/assets/office/room.png`) 갱신
2. 아래 파이썬으로 base64 재생성 후 두 아티팩트 HTML에 주입, 각각 재게시
   ```python
   import base64, json, os
   base = 'webview-ui/assets/characters'
   AGENTS = ['report-reader','structure-planner','slide-writer','ppt-builder','data-analyst']
   POSES = ['idle','typing','reading','bash','waiting','thinking','done']
   sprites = {}
   sprites_walk = {}
   for a in AGENTS:
       sprites[a] = {p: 'data:image/png;base64,' + base64.b64encode(
           open(os.path.join(base,a,p+'.png'),'rb').read()).decode()
           for p in POSES}
       sprites_walk[a] = ['data:image/png;base64,' + base64.b64encode(
           open(os.path.join(base,a,f'walk{i}.png'),'rb').read()).decode()
           for i in range(4)]
   room_uri = 'data:image/png;base64,' + base64.b64encode(
       open('webview-ui/assets/office/room.png','rb').read()).decode()
   # -> js = 'window.SPRITES = ' + json.dumps(sprites) + ';\n' \
   #         'window.SPRITES_WALK = ' + json.dumps(sprites_walk) + ';'
   #    를 HTML의 <script>/*__SPRITES__*/</script> 자리에, room_uri는 room-banner
   #    <img src="..."> 자리(또는 ROOM_SRC_PLACEHOLDER)에 주입
   ```
3. `Artifact` 도구로 각 URL에 재게시 (`url` 파라미터로 기존 주소 지정, `file_path`는 새로 만든 HTML)

**중요 — 이 환경엔 Artifact의 `upload_asset` 액션이 없다** (문서엔 있다고 나오지만 실제 도구 스키마엔
없음, 확인됨). 그래서 이미지는 asset 업로드가 아니라 **base64 data URI로 HTML에 직접 인라인**한다
(파일당 약 3.7MB, 16MB 한도 내).

## 재사용 관련 참고

- 위 서브에이전트/커맨드 파일들은 이 저장소에 커밋되어 있어 이 저장소를 여는 세션이면 자동 로드된다.
- 현재는 `claude/clever-cori-mmvmwu` 브랜치에 있음 — `main`에도 항상 자동으로 있게 하려면 PR 머지가 필요하다.
