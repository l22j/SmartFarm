# 브라우저 Claude Code로 PPT/문서 자동화 온보딩 - Design Spec

## I. Project Information

| Item | Value |
| ---- | ----- |
| **Project Name** | pptx-guide-deck |
| **Canvas Format** | PPT 16:9 (1280×720) |
| **Page Count** | 14 |
| **Design Style** | instructional × soft-rounded |
| **Target Audience** | 브라우저 Claude Code로 문서 자동화(PPT/DOCX)를 처음 시작하는 사내 실무자 (개발 경험 적음) |
| **Use Case** | 사내 교육/온보딩 세션 발표 자료 (약 20분) |
| **Delivery Purpose** | `balanced` — 발표 + 이후 참고 문서로도 열람 |
| **Content Strategy** | 그대로 따르기 — 구조(structure-designer)·문구(copywriter) 단계에서 이미 순서·제목·불릿·발표자 노트가 확정됨. 사실·수치 추가 없이 시각 디자인만 새로 설계 |
| **Created Date** | 2026-09-11 |

---

## II. Canvas Specification

| Property | Value |
| -------- | ----- |
| **Format** | PPT 16:9 |
| **Dimensions** | 1280×720 |
| **viewBox** | `0 0 1280 720` |
| **Margins** | left/right 64px, top/bottom 56px |
| **Content Area** | 1152×608 |

---

## III. Visual Theme

### Theme Style

- **Mode**: instructional — 개념 분해 → 단계별 설명, 튜토리얼/온보딩 콘텐츠 표준 매칭
- **Visual style**: soft-rounded — 둥근 카드, 부드러운 elevation, 친근한 리듬 (Product/SaaS/training 권장 스타일)
- **Theme**: Light theme
- **Tone**: 실무 교육용, 친절하지만 군더더기 없음

### Color Scheme

| Role | HEX | Purpose |
| ---- | --- | ------- |
| **Background** | `#FFFFFF` | 페이지 배경 |
| **Secondary bg** | `#F4F7FB` | 카드/섹션 배경 |
| **Surface** | `#FFFFFF` | 카드 표면 (soft shadow로 배경과 구분) |
| **Primary** | `#1565C0` | 타이틀 강조, 주요 섹션, 아이콘 |
| **Accent** | `#FF6B35` | 핵심 정보, 강조 포인트 |
| **Secondary accent** | `#0D47A1` | 보조 강조, 그라데이션 |
| **Body text** | `#1D2733` | 본문 텍스트 |
| **Secondary text** | `#5B6B7C` | 캡션, 주석 |
| **Tertiary text** | `#94A3B3` | 부가정보, 푸터 |
| **Border/divider** | `#E2E8F0` | 카드 테두리, 구분선 |
| **Success** | `#2E7D32` | 체크/완료 표시 |
| **Warning** | `#C62828` | 주의(보안/기밀) 표시 |

> Note: Tech/개발도구 온보딩 콘텐츠에 맞춘 밝은 블루 계열 팔레트(사용자 확인 완료).

### Gradient Scheme

```xml
<linearGradient id="titleGradient" x1="0%" y1="0%" x2="100%" y2="100%">
  <stop offset="0%" stop-color="#1565C0"/>
  <stop offset="100%" stop-color="#0D47A1"/>
</linearGradient>
<radialGradient id="bgDecor" cx="85%" cy="15%" r="55%">
  <stop offset="0%" stop-color="#1565C0" stop-opacity="0.12"/>
  <stop offset="100%" stop-color="#1565C0" stop-opacity="0"/>
</radialGradient>
```

---

## IV. Typography System

### Font Plan

**Typography direction**: Pretendard 고정 (이 환경의 install-local 폰트 락) — 친근한 산세리프, soft-rounded 스타일의 둥근 letterform과 부합.

| Role | Chinese | English | Fallback tail |
| ---- | ------- | ------- | ------------- |
| **Title** | "Pretendard ExtraBold" | "Pretendard ExtraBold" | Pretendard, "Malgun Gothic", sans-serif |
| **Body** | Pretendard | Pretendard | "Malgun Gothic", sans-serif |
| **Emphasis** | "Pretendard SemiBold" | "Pretendard SemiBold" | Pretendard, "Malgun Gothic", sans-serif |
| **Code** | — | Consolas | "Courier New", monospace |

**Per-role font stacks**:

- Title: `"Pretendard ExtraBold", Pretendard, "Malgun Gothic", sans-serif`
- Body: `Pretendard, "Malgun Gothic", sans-serif`
- Emphasis: `"Pretendard SemiBold", Pretendard, "Malgun Gothic", sans-serif`
- Code: `Consolas, "Courier New", monospace`

> 이 배포본을 공유할 경우 수신자 환경에 Pretendard가 없으면 대체 폰트로 표시됩니다 (`assets/fonts/Pretendard/`에 폰트 파일 동봉 권장).

### Font Size Hierarchy

**Baseline**: body = 24px (`balanced`)

| Role | Size (px) | Weight |
| ---- | --------- | ------ |
| Cover title | 72 | ExtraBold |
| Page title | 42 | ExtraBold |
| Subtitle (cover only) | 32 | SemiBold |
| Lead (core message) | 30 | SemiBold |
| Body | 24 | Regular |
| Annotation / caption | 18 | Regular |
| Footnote / page number | 16 | Regular |

---

## V. Layout Principles

### Page Structure

- **Header area**: 상단 56px 여백 + 타이틀(및 core message lead 줄) 영역, 약 150-180px
- **Content area**: 나머지 영역 — 카드 그리드 / 리스트 / 다이어그램
- **Footer area**: 하단 40px, 페이지 번호 + 출처 각주 (footnote 16px)

### Layout Pattern Library 사용 계획

- 표지/마무리(P01, P14): Single column centered + 원형 데코 블롭 (soft-rounded 특징)
- 체크리스트/리스트형(P05, P08, P13): Vertical list, 아이콘 + 텍스트 행
- 비교/개요형(P02, P11): Asymmetric/Symmetric split — 좌우 2단 레인
- 표형(P03, P04): 카드 그리드 (3~4 컬럼)
- 프로세스형(P06, P07, P09, P10, P12): 좌→우 순차 스텝/체브론/파이프라인

### Spacing Specification

**Universal**

| Element | Current Project |
| ------- | --------------- |
| Safe margin from canvas edge | 64px (좌우) / 56px (상하) |
| Content block gap | 32px |
| Icon-text gap | 12px |

**Card-based layouts**

| Element | Current Project |
| ------- | --------------- |
| Card gap | 24px |
| Card padding | 24px |
| Card border radius | 16px |
| Three/four-column card width | 260-360px |

---

## VI. Icon Usage Specification

### Source

- **Built-in icon library**: `templates/icons/tabler-outline/` — stroke_width 2 고정

### Recommended Icon List

| Purpose | Icon Path | Page |
| ------- | --------- | ---- |
| 브라우저 | `tabler-outline/browser` | P01, P05 |
| 체크리스트/완료 | `tabler-outline/circle-check` | P05, P13 |
| GitHub 계정/저장소 | `tabler-outline/brand-github` | P05, P06 |
| Fork/브랜치 | `tabler-outline/git-branch` | P06, P09 |
| PR/머지 | `tabler-outline/git-pull-request` | P09 |
| 문서(원본) | `tabler-outline/file-text` | P02, P07 |
| DOCX 파일 | `tabler-outline/file-word` | P07, P12 |
| PPT 산출물 | `tabler-outline/presentation` | P02, P08 |
| AI/Claude | `tabler-outline/robot` | P01, P04 |
| Skill 개념 | `tabler-outline/puzzle-2` | P04 |
| Subagent 개념 | `tabler-outline/settings-2` | P04, P10 |
| 파이프라인/흐름 | `tabler-outline/route-2` | P02, P10, P11 |
| 청중/팀 | `tabler-outline/users-group` | P04 |
| 매뉴얼/가이드 | `tabler-outline/book` | P04 |
| 시작/실행 | `tabler-outline/rocket` | P14 |
| 다운로드 | `tabler-outline/download` | P07, P12 |
| 보안/Private | `tabler-outline/lock` | P06, P13 |

---

## VII. Visualization Reference List

Catalog read: 76 templates

| Page | Template | Path | Summary-quote (verbatim) | Usage |
| ---- | -------- | ---- | ------------------------- | ----- |
| P03 | agenda_list | `templates/charts/agenda_list.svg` | "Pick for table of contents, meeting agendas, or presentation roadmap — numbered items + brief description + duration / owner per row. Skip for substantive content lists (use vertical_list) or single-page section dividers (use a cover layout)." | Level 1/2/3 로드맵, 각 Level의 소요 시간을 duration으로 표기 |
| P04 | vertical_pillars | `templates/charts/vertical_pillars.svg` | "Pick for 1×3 / 1×4 / 1×5 vertical column layout where each pillar = one independent category with title + bullets — PEST (Political/Economic/Social/Technological), four-pillar strategy overview, side-by-side independent categories. Skip for 2×2 quadrant (use quadrant_text_bullets), pricing tiers (use comparison_columns), or 2×2 parallel aspects (use labeled_card)." | Claude/Skill/Subagent/Template 4개 개념을 독립 컬럼으로 비교 |
| P05 | vertical_list | `templates/charts/vertical_list.svg` | "Pick for 3-6 numbered key points each with a short description — design principles, core tenets, action items, key takeaways, recommendations, executive summary points. Skip for icon-style cards (use icon_grid) or sequential steps (use numbered_steps)." | 계정 3종 + GitHub 연동 체크리스트 4항목 |
| P06 | numbered_steps | `templates/charts/numbered_steps.svg` | "Pick for 3-6 horizontal sequential steps with numeric emphasis — how-it-works section, getting-started guide, methodology overview, implementation phases. Skip if steps need connector arrows (use process_flow) or named output artifacts (use pipeline_with_stages)." | Fork 버튼 클릭 → 저장소 선택 → 연결 확인 3단계 |
| P07 | numbered_steps | `templates/charts/numbered_steps.svg` | "Pick for 3-6 horizontal sequential steps with numeric emphasis — how-it-works section, getting-started guide, methodology overview, implementation phases. Skip if steps need connector arrows (use process_flow) or named output artifacts (use pipeline_with_stages)." | 설치 2줄 → 프롬프트 요청 → 다운로드 확인 4단계 |
| P08 | vertical_list | `templates/charts/vertical_list.svg` | "Pick for 3-6 numbered key points each with a short description — design principles, core tenets, action items, key takeaways, recommendations, executive summary points. Skip for icon-style cards (use icon_grid) or sequential steps (use numbered_steps)." | 짧은/자세한 프롬프트 + 조건 3가지, 5개 포인트를 체크형 리스트로 |
| P09 | process_flow | `templates/charts/process_flow.svg` | "Pick for 3-8 sequential steps connected by simple arrows — approval workflows, customer onboarding, request handling, lifecycle stages. Skip if cyclical (use circular_stages) or stages produce named outputs (use pipeline_with_stages)." | 격리 VM → 브랜치 → 커밋/push → PR → Merge 5단계 |
| P10 | pipeline_with_stages | `templates/charts/pipeline_with_stages.svg` | "Pick for 3-5 horizontal pipeline stages, each = title + 1-line description + output artifact, connected by arrows (data pipelines, ETL, build pipelines). Skip if any stage lacks an artifact (use process_flow or numbered_steps)." | content-planner→slide-writer→ppt-builder→ppt-reviewer, 각 단계별 산출물(json/json/pptx/리포트) 명시 |
| P12 | numbered_steps | `templates/charts/numbered_steps.svg` | "Pick for 3-6 horizontal sequential steps with numeric emphasis — how-it-works section, getting-started guide, methodology overview, implementation phases. Skip if steps need connector arrows (use process_flow) or named output artifacts (use pipeline_with_stages)." | docx 생성 → 다운로드 → 한컴 열기 → 다른 이름 저장(HWP) 4단계 |
| P13 | vertical_list | `templates/charts/vertical_list.svg` | "Pick for 3-6 numbered key points each with a short description — design principles, core tenets, action items, key takeaways, recommendations, executive summary points. Skip for icon-style cards (use icon_grid) or sequential steps (use numbered_steps)." | 매번 확인할 5개 체크리스트 항목 |

**Runners-up considered**:

- `process_flow` | rejected for P06: Fork 실습은 손실/분기 없는 단순 튜토리얼 스텝이라 화살표 흐름보다 `numbered_steps`의 번호 강조가 학습 맥락에 더 맞음
- `pipeline_with_stages` | rejected for P09: PR 반영 흐름은 각 단계에 "산출물(artifact)"이 없고 단순 순차 이벤트라 `process_flow`가 더 정확
- `agenda_list` | rejected for P13: 항목이 시간/오너가 있는 안건이 아니라 매번 반복되는 체크 항목이라 `vertical_list`가 더 맞음

P02(오늘 만드는 것 — PPT/HWP 두 흐름 분기)와 P11(최초 실행 vs 재사용 프롬프트 비교)은 `no-template-match`로 처리 — P02는 원본 문서 1개가 PPT 파이프라인과 한글 문서 파이프라인 두 갈래로 분기하는 비선형 구조라 76개 템플릿 중 분기 다이어그램에 맞는 항목이 없어 커스텀 2-레인 분기 레이아웃 사용. P11은 프롬프트 텍스트 두 개(최초/재사용)를 나란히 비교하는 것으로 수치/카드 비교가 아니라 커스텀 2단 텍스트 카드 비교 레이아웃 사용.

---

## VIII. Image Resource List

이미지(사진/AI 일러스트) 사용 안 함 (사용자 확인) — no image rows. 모든 시각 요소는 다이어그램/아이콘/카드로 표현.

---

## IX. Content Outline

> `pptx_structure.mode: flat` — Slide-local, 기본 Master/Blank Layout 사용.

#### Slide 01 - Cover

- **Cover impact**: 훅 = "설치 없이, 브라우저만으로" — 도구적 진입장벽 제거를 즉시 각인. 구성 전략 = 타이포그래피 중심 포스터형 (큰 타이틀 + 우상단 소프트 블롭 데코 + 브라우저 아이콘)
- **Layout**: Single column centered, 우상단 큰 원형 블롭(primary tint) 배경, 중앙 타이틀 + 서브타이틀
- **Title**: 브라우저 Claude Code로 문서 자동화 시작
- **Subtitle**: 설치 없이, 브라우저만으로
- **Info**: 사내 온보딩 세션 · 약 20분

---

#### Slide 02 - 오늘 만드는 것: PPT와 HWP

- **Layout**: no-template-match 커스텀 — 상단 "원본 문서" 카드 1개 → 좌우 2-레인 분기(좌: PPT 파이프라인 4단계 미니 체인, 우: HWP 파이프라인 3단계 미니 체인)
- **Core message**: 원본 문서 하나만 있으면 PPT와 한글 문서(HWP) 두 산출물을 브라우저에서 바로 만들 수 있다.
- **Content**:
  - 브라우저에서 Claude Code 실행 후 GitHub 저장소 연결
  - 원본 문서 첨부와 한 프롬프트로 PPT 생성
  - 서브에이전트 4개 파이프라인으로 다음 달까지 재사용
  - 회사 한글 문서(HWP) 양식 유지한 채 내용 교체

---

#### Slide 03 - 오늘의 3단계 로드맵

- **Layout**: agenda_list 적용 — 3행 로드맵 카드(Level/소요시간/설명)
- **Core message**: Level 1→2→3 순으로 난이도를 높이며, 단발성 작업이면 Level 2까지만 해도 충분하다.
- **Visualization**: agenda_list (see §VII)
- **Content**:
  - Level 1: Skill 하나로 DOCX 워밍업, 소요 10분
  - Level 2: GitHub Skill로 PPT 제작, 소요 20분
  - Level 3: 서브에이전트 파이프라인 구축, 소요 40분
- Caption: "Level 3까지 다 안 해도 됨 — 단발성이면 Level 2까지 충분"

---

#### Slide 04 - 핵심 개념: Skill과 Subagent

- **Layout**: vertical_pillars 적용 — 4컬럼(Claude/Skill/Subagent/Template)
- **Core message**: Skill은 AI가 참고하는 업무 매뉴얼이고, Subagent는 특정 역할을 전담하는 별도 담당자다.
- **Visualization**: vertical_pillars (see §VII)
- **Content**:
  - Claude = 신입 직원
  - Skill = 업무 매뉴얼
  - Subagent = 옆 팀 담당자
  - Template = 표준 양식

---

#### Slide 05 - 준비물: 계정 3종과 GitHub 연동

- **Layout**: vertical_list 적용 — 아이콘 + 체크 리스트 4행
- **Core message**: 이메일·GitHub 계정·Claude Pro 이상 플랜을 준비하고 GitHub 연동까지 마치면 실습 준비가 끝난다.
- **Visualization**: vertical_list (see §VII)
- **Content**:
  - 이메일 — GitHub·Claude 가입용 준비
  - GitHub 계정 — Claude Code 파일 저장소 역할
  - Claude Pro 이상 플랜 — 브라우저 Claude Code 사용 자격
  - claude.ai/code에서 GitHub 연동(Connect GitHub) 완료
- Caption: "이 준비는 최초 1회만 필요"

---

#### Slide 06 - 저장소 준비: slide-master Fork

- **Layout**: numbered_steps 적용 — 3단계 가로 스텝 + 하단 경고 배지
- **Core message**: 초보자는 slide-master 저장소를 Fork해서 오늘 실습용 저장소로 사용한다.
- **Visualization**: numbered_steps (see §VII)
- **Content**:
  - slide-master 저장소 우상단 Fork 버튼 클릭
  - Owner·Repository name 확인 후 Create fork 클릭
  - Claude Code에서 Fork한 저장소 선택 및 연결
- Caption: "회사 기밀 저장소는 반드시 Private 설정" (warning 색상 + lock 아이콘)

---

#### Slide 07 - Level 1: Skill로 DOCX 워밍업

- **Layout**: numbered_steps 적용 — 4단계 (설치 명령 카드 2개는 code_family 모노스페이스로 표기)
- **Core message**: docx 스킬 설치 후 원본을 첨부하고 한 문장 프롬프트만으로 한국어 업무보고 DOCX를 완성할 수 있다.
- **Visualization**: numbered_steps (see §VII)
- **Content**:
  - `/plugin marketplace add anthropics/skills` 실행
  - `/plugin install document-skills@anthropic-agent-skills` 실행
  - 원본 첨부 후 "docx 스킬로 업무보고 DOCX 만들어줘" 요청
  - 결과 다운로드 후 Word·한컴오피스에서 확인
- Caption: "이 단계만으로도 회의록·주간보고는 충분"

---

#### Slide 08 - Level 2: ppt-master로 PPT 만들기

- **Layout**: vertical_list 적용 — 5개 조건 포인트
- **Core message**: ppt-master 스킬에 장수·톤·색상·폰트 같은 조건을 구체적으로 붙일수록 결과 품질이 올라간다.
- **Visualization**: vertical_list (see §VII)
- **Content**:
  - 짧은 버전 — 경로 지정만으로 초안 생성
  - 자세한 버전 — 장수·톤·색상·폰트 조건 추가
  - 핵심 KPI·숫자 강조 지정
  - 표·차트 적합 구간 시각화 지정
  - 조건이 구체적일수록 결과 품질 향상

---

#### Slide 09 - 결과 확인과 GitHub 자동 반영

- **Layout**: process_flow 적용 — 5단계 순차 화살표
- **Core message**: Claude가 만든 PPT는 자동으로 브랜치·PR로 올라오며, 사용자는 PR 링크에서 Merge 한 번으로 최종 반영한다.
- **Visualization**: process_flow (see §VII)
- **Content**:
  - 격리 VM에서 파일 생성 후 브랜치 자동 생성
  - 커밋·push 후 PR(Pull Request) 초안 생성
  - PR 링크에서 Merge pull request로 반영
  - 부분 수정은 채팅으로 즉시 요청 가능
- Caption: "PR 확인과 Merge는 사람이 직접 클릭"

---

#### Slide 10 - Level 3: 4개 서브에이전트 파이프라인

- **Layout**: pipeline_with_stages 적용 — 4단계, 각 단계에 산출물 표기
- **Core message**: PPT 제작 과정을 content-planner → slide-writer → ppt-builder → ppt-reviewer 4개 서브에이전트로 나누면 단계별 재실행과 재사용이 가능해진다.
- **Visualization**: pipeline_with_stages (see §VII)
- **Content**:
  - content-planner — 원본 분석 후 슬라이드 구조 설계 (출력: structure.json)
  - slide-writer — 구조 JSON 기반 슬라이드별 문구 작성 (출력: copy.json)
  - ppt-builder — ppt-master 스킬로 PPTX 실제 생성 (출력: final.pptx)
  - ppt-reviewer — 원본과 PPTX 대조 검수 (출력: 검수 리포트)
- Caption: "단발성이면 생략 가능, 반복 업무·팀 표준화 시 필요"

---

#### Slide 11 - 파이프라인 실행과 다음 번 재사용

- **Layout**: no-template-match 커스텀 — 좌우 2단 카드(최초 실행 프롬프트 vs 재사용 3줄 프롬프트), code_family 모노스페이스 텍스트 블록
- **Core message**: 파이프라인을 한 번 만들어두면 다음 달부터는 원본 경로만 바꾼 세 줄 프롬프트로 같은 4단계가 그대로 재사용된다.
- **Content**:
  - 최초 실행 — 4단계 파이프라인 상세 지시 프롬프트
  - 재사용 — 원본 경로만 바꾼 세 줄 프롬프트
  - 검수 이슈 발견 시 해당 단계만 재실행
  - 에이전트는 저장소에 이미 존재, 재생성 불필요
- Caption: "에이전트는 이미 저장소에 있어 이름만 부르면 재사용"

---

#### Slide 12 - 한글 문서(HWP) 처리 흐름

- **Layout**: numbered_steps 적용 — 4단계
- **Core message**: HWP는 직접 생성하는 스킬이 없어, docx 스킬로 회사 양식을 유지한 DOCX를 만든 뒤 한컴오피스에서 다른 이름으로 저장해 HWP로 변환한다.
- **Visualization**: numbered_steps (see §VII)
- **Content**:
  - docx 스킬로 회사 양식 유지한 DOCX 생성
  - 완성 파일 다운로드
  - 한컴오피스에서 파일 열기
  - 다른 이름으로 저장 후 한글 문서(HWP) 선택
- Caption: "DOCX-HWP 서식은 완벽히 유지되지 않을 수 있어 직접 확인 필요"

---

#### Slide 13 - 매번 사람이 확인할 것

- **Layout**: vertical_list 적용 — 5행 체크리스트
- **Core message**: AI가 초안을 빠르게 만들어주더라도 숫자·핵심 메시지·폰트·법무 리뷰·기밀 보안은 매번 사람이 마지막으로 확인해야 한다.
- **Visualization**: vertical_list (see §VII)
- **Content**:
  - 숫자·날짜·고유명사의 원본 일치 확인
  - 핵심 메시지 누락 여부 확인
  - 폰트 깨짐 여부 확인(PowerPoint·한컴에서 직접 열람)
  - 대외 문서의 법무·홍보 리뷰
  - 회사 기밀 자료의 Private 저장소 보관
- Caption: "AI는 초안 생성과 반복 축소를 돕는 도구, 최종 책임은 사람"

---

#### Slide 14 - Closing

- **Closing impact**: 테이크어웨이 = "Skill은 일하는 방법, Subagent는 역할 담당자 — 한 번 만들면 다음부터는 이름만 불러 재사용" + 행동 유도(지금 바로 Fork해서 Level 1 실습). 구성 = 타이포그래피 중심 클로징 (핵심 문장 인용구 강조 + CTA 배지)
- **Layout**: Single column centered, 인용구 스타일 큰 텍스트 + 하단 CTA 배지 (rocket 아이콘)
- **Content**:
  - Skill — 일하는 방법
  - Subagent — 특정 역할 담당자
  - 한 번 만든 에이전트, 저장소에 축적
  - 다음부터는 이름만 호출해 재사용
  - CTA: "지금 바로 slide-master Fork 후 Level 1부터 시작"

---

## X. Speaker Notes Requirements

카피 JSON(`02-copy.json`)의 `speaker_notes` 필드를 페이지별로 그대로 사용. 파일명은 SVG명에 맞춰 `notes/01_cover.md` ~ `notes/14_closing.md`로 저장 예정.
