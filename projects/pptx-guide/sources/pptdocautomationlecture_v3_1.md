# 브라우저 Claude Code로 PPT 만들기 — 완전 초보자용 가이드
### GitHub 가입부터 서브에이전트 재사용 파이프라인까지, 클릭·프롬프트 스텝 바이 스텝

> **업데이트 기준: 2026-09-11**
> Claude Code·Skills·GitHub UI는 업데이트가 빠릅니다. 화면 문구가 조금 다르면 비슷한 버튼을 찾으세요.
> **대상 독자**: ChatGPT·Gemini·Claude를 채팅으로 써본 적은 있지만 GitHub·Claude Code는 처음인 분.

---

## 목차

- [이 강의로 할 수 있게 되는 것](#이-강의로-할-수-있게-되는-것)
- [오늘 만드는 것 (그림)](#오늘-만드는-것-그림)
- [0부. 완전 처음부터 — 계정·저장소 준비](#0부-완전-처음부터--계정저장소-준비)
- [1부. 개념 5분 — Skill과 Subagent](#1부-개념-5분--skill과-subagent)
- [2부. Level 1 — Skill 하나로 워밍업 (docx)](#2부-level-1--skill-하나로-워밍업-docx)
- [3부. Level 2 — GitHub Skill로 PPT 만들기](#3부-level-2--github-skill로-ppt-만들기)
- [4부. Level 3 — PPT 파이프라인 서브에이전트 만들기](#4부-level-3--ppt-파이프라인-서브에이전트-만들기)
- [5부. 한글 문서(HWP) — Skill 위주](#5부-한글-문서hwp--skill-위주)
- [6부. 두 번째부터는 이렇게](#6부-두-번째부터는-이렇게)
- [부록 A. 자주 막히는 상황](#부록-a-자주-막히는-상황)
- [부록 B. 프롬프트 치트시트](#부록-b-프롬프트-치트시트)
- [부록 C. 심화 — 원하는 사람만](#부록-c-심화--원하는-사람만)

---

## 이 강의로 할 수 있게 되는 것

- 브라우저에서 **Claude Code**를 열고 GitHub 저장소와 연결
- 원본 문서(PDF/DOCX)를 넣고 **한 번 프롬프트로 PPT 파일** 뽑기
- 이 작업을 **재사용 가능한 파이프라인(서브에이전트 4개)**으로 만들어 다음 달에도 그대로 쓰기
- 회사 **한글 문서 양식(HWP)** 유지하면서 새 내용 채우기

**설치 필요 없음.** 브라우저만 있으면 됨.

---

## 오늘 만드는 것 (그림)

```text
                        원본 문서
                (PDF / DOCX / Markdown)
                          │
     ┌────────────────────┴────────────────────┐
     │                                         │
     ▼                                         ▼
[PPT 만들기]                             [한글 문서 만들기]
                                        회사 HWP 양식 유지

┌────────────────────────┐               ┌───────────────┐
│ content-planner        │               │  docx 스킬    │
│  ↓                     │               │       │       │
│ slide-writer           │               │       ▼       │
│  ↓                     │               │  DOCX 생성    │
│ ppt-builder            │               │       │       │
│ (ppt-master 스킬 사용) │               │       ▼       │
│  ↓                     │               │  한컴에서     │
│ ppt-reviewer           │               │  HWP로 저장   │
└────────────────────────┘               └───────────────┘
     │                                         │
     ▼                                         ▼
   PPTX                                       HWP
```

---

# 0부. 완전 처음부터 — 계정·저장소 준비

이 부분은 **한 번만** 하면 됨. 두 번째 사용부터는 3부(또는 4부)로 바로.

## 0-1. 필요한 계정 세 개

| # | 뭐가 필요 | 어디에 쓰나 | 비용 |
|---|---|---|---|
| 1 | 이메일 | GitHub·Claude 가입 시 | 무료 |
| 2 | **GitHub 계정** | Claude Code가 파일을 저장하는 창고 | 무료 |
| 3 | **Claude 계정 (Pro 이상)** | 브라우저 Claude Code 사용 자격 | 유료 (월 20$ 정도) |

> **왜 Pro가 필요한가**: 브라우저 Claude Code(claude.ai/code)는 Pro·Max·Team 플랜에서 열립니다. Free 플랜에서는 일반 채팅만 가능.

---

## 0-2. GitHub 가입 (이미 있으면 스킵)

### 순서

1. 브라우저 주소창에 `github.com` 입력하고 Enter
2. 화면 오른쪽 위 **"Sign up"** 버튼 클릭
3. 이메일 주소 입력 → **Continue**
4. 비밀번호 입력 (8자 이상, 숫자·소문자 포함) → **Continue**
5. **username** (사용자 이름) 입력 — 이건 나중에 URL에 들어가니 짧고 기억하기 좋게
   - 예: `eunju-lee-2026` 같은 형식
6. 이메일 수신 여부 y/n → **Continue**
7. 로봇 확인 (퍼즐) 통과
8. **Create account** 클릭
9. 이메일함으로 가서 **인증 코드 6자리** 확인 → GitHub 화면에 입력
10. "Welcome!" 화면이 뜨면 → **완료**

### 확인

- 다시 `github.com` 접속
- 우상단에 내 프로필 아이콘(동그란 이미지)이 보이면 → **로그인 됨**

---

## 0-3. Claude Pro 확인 (또는 가입)

### 순서

1. 브라우저 주소창에 `claude.ai` 입력하고 Enter
2. **Continue with Google** 또는 이메일로 로그인
3. 로그인 후 좌측 하단 프로필 클릭 → **Settings** → **Plans & Billing** 이동
4. 현재 플랜 확인:
   - `Pro`, `Max`, `Team` 중 하나면 → **바로 다음 단계로**
   - `Free`면 → **Upgrade to Pro** 클릭 → 결제 진행

### 확인

- `claude.ai/code` 주소로 접속했을 때 **Claude Code 페이지가 열리면** 준비 완료
- "이 기능은 Pro 이상에서 사용 가능" 같은 메시지가 뜨면 아직 Free 상태

---

## 0-4. Claude Code 웹 열고 GitHub 연동

### 순서

1. 브라우저에서 `claude.ai/code` 접속
2. 처음이면 GitHub 연결 안내가 뜸 → **"Connect GitHub"** (또는 비슷한 버튼) 클릭
3. GitHub 로그인 화면으로 이동 → 이미 로그인 상태면 바로 권한 요청 화면
4. **"Authorize Anthropic"** (또는 "Install & Authorize") 클릭
5. 저장소 접근 범위 선택:
   - **"All repositories"** — 내 모든 저장소 접근 가능 (편함)
   - **"Only select repositories"** — 특정 저장소만 (안전함, 나중에 추가 가능)
   - 처음이면 **All repositories** 선택이 편함
6. **Install** 클릭
7. Claude Code 화면으로 자동 복귀

### 확인

- Claude Code 첫 화면에 **저장소 목록**이 뜨거나 "저장소를 선택하세요" 안내가 보이면 → 연동 성공
- 저장소 목록이 비어 있으면 → 아직 저장소를 안 만든 것 (0-5에서 만듦)

---

## 0-5. 오늘 사용할 저장소 준비

두 가지 방법. **초보자는 A를 추천**.

### A. `slide-master` 저장소를 내 계정으로 Fork (권장)

Fork = 남의 GitHub 저장소를 내 계정으로 **복사**하는 것.

#### 순서

1. 브라우저에서 `https://github.com/byungjunjang/slide-master` 접속
2. 화면 **우상단**에 있는 **"Fork"** 버튼 클릭
3. "Create a new fork" 화면이 뜸:
   - **Owner**: 내 GitHub username (자동 선택되어 있음)
   - **Repository name**: `slide-master` (그대로 두면 됨)
   - 나머지 옵션은 건드리지 말고
4. 아래쪽 **"Create fork"** 버튼 클릭
5. 5~10초 기다리면 URL이 `https://github.com/<내username>/slide-master`로 바뀜
6. 화면에 파일 목록이 보이면 → **완료**

### B. 완전히 빈 새 저장소 만들기 (Fork 안 하고 처음부터)

Claude Code 채팅창에 이 프롬프트 던지면 됨:

```text
내 GitHub 계정에 my-ppt-workspace 라는 새 저장소를 만들어줘.
Private으로 만들고, README만 하나 있는 빈 저장소로 시작해줘.
```

Claude가 만들어주면 → 저장소 이름 확인 후 다음 단계로.

> **⚠️ 회사 자료 주의**: 회사 기밀이 들어갈 저장소는 반드시 **Private**로 만드세요. Fork한 저장소도 Settings → General → Danger Zone → "Change visibility"에서 Private로 바꿀 수 있음.

---

## 0-6. Claude Code에서 저장소 열기

### 순서

1. `claude.ai/code` 로 다시 이동
2. 상단 또는 좌측의 **저장소 선택 드롭다운** 클릭
3. Fork(또는 방금 만든) 저장소 이름 찾아서 선택
   - 목록에 안 뜨면 우측 상단의 새로고침 아이콘 클릭
4. 저장소가 선택되면 **"New task"** (또는 채팅 시작) 버튼 클릭
5. 채팅창이 열림

### 확인 — 준비 완료 테스트

채팅창에 아래 프롬프트 그대로 복사해서 붙여넣고 Enter:

```text
지금 내가 어떤 저장소에 연결되어 있는지 알려줘.
저장소 안에 어떤 파일과 폴더가 있는지 목록도 보여줘.
```

**성공 예시 답변**:
> "현재 `<내username>/slide-master` 저장소에 연결되어 있습니다. 파일 목록은 다음과 같습니다: README.md, .claude/, ..."

이런 답이 나오면 → **0부 끝. 이제 진짜 시작 가능**.

**실패 시**:
- "저장소를 찾을 수 없습니다" → 0-4로 돌아가 권한 재확인
- 답이 아예 안 옴 → 페이지 새로고침 후 재시도

---

# 1부. 개념 5분 — Skill과 Subagent

3부·4부에서 사용할 두 단어만 알면 됨.

## 1-1. 한 문장 정의

> **Skill** = AI에게 "이 일 이렇게 해"를 가르치는 **매뉴얼**
> **Subagent** = 특정 역할을 맡는 **별도 담당자**

### 회사 비유

| 개념 | 회사에서 뭐에 해당 |
|---|---|
| Claude | 신입 직원 |
| Skill | 그 직원이 참고하는 업무 매뉴얼 |
| Subagent | 옆 팀 담당자 (검수 담당, 카피 담당 등) |
| Template | 회사 표준 양식 (PPT/DOCX 템플릿) |

---

## 1-2. 오늘 3단계로 배움

| Level | 뭘 배우나 | 걸리는 시간 |
|---|---|---|
| **Level 1** (2부) | Skill 하나로 DOCX 만들기 — 워밍업 | 10분 |
| **Level 2** (3부) | GitHub에서 가져온 Skill로 PPT 하나 만들기 | 20분 |
| **Level 3** (4부) | PPT 파이프라인 서브에이전트 4개 만들어 재사용 | 40분 |

**Level 3까지 다 안 해도 됨.** 단발성 PPT면 Level 2까지로 충분. 매달 반복할 거면 Level 3까지.

---

# 2부. Level 1 — Skill 하나로 워밍업 (docx)

**목표: 아무 문서나 넣고 "DOCX로 만들어줘" 한 문장으로 완성.**

## 2-1. Document Skills 설치

Claude Code 채팅창에 두 줄 순서대로 입력.

### ① 마켓플레이스 추가

채팅창에 이거 복사·붙여넣기 후 Enter:

```text
/plugin marketplace add anthropics/skills
```

**성공 답변 예시**:
> "Marketplace 'anthropics/skills'가 추가되었습니다."

### ② Document Skills 설치

바로 이어서:

```text
/plugin install document-skills@anthropic-agent-skills
```

**성공 답변 예시**:
> "document-skills가 설치되었습니다. 사용 가능한 스킬: docx, pdf, pptx, xlsx"

### 확인

```text
지금 사용 가능한 스킬 목록 보여줘.
```

`docx`가 목록에 있으면 → **설치 완료**.

---

## 2-2. 원본 자료 업로드

### 순서

1. 채팅창 아래에 있는 **파일 첨부 아이콘**(클립 모양 또는 +) 클릭
2. 내 컴퓨터에서 PDF 또는 DOCX 하나 선택
3. 업로드 완료되면 파일 이름이 채팅창 위에 표시됨

또는:

1. Claude Code 저장소 안 파일 탐색기에서 `projects/` 폴더 생성
2. 그 안에 파일 드래그해서 넣기
3. 채팅에서 파일 경로로 참조

**초보자 추천: 그냥 파일 첨부 아이콘 사용.**

---

## 2-3. 첫 프롬프트

채팅창에 복사·붙여넣기:

```text
docx 스킬을 사용해서 방금 첨부한 보고서로
한국어 업무보고 DOCX 만들어줘.

구조:
- 제목
- 요약 (3~5줄)
- 주요 내용
- 향후 계획

숫자와 날짜는 원문 그대로 유지해줘.
```

Enter 누르고 30초~2분 기다리면 결과가 나옴.

---

## 2-4. 결과 확인 + 다운로드

### 순서

1. Claude가 "DOCX 파일을 만들었습니다. 다운로드 가능합니다"라고 하면 → 답변 안 파일 링크 클릭
2. 다운로드된 파일을 Word/한컴오피스에서 열어 확인
3. 마음에 안 들면 채팅창에서 바로 수정 요청:

```text
2번 섹션이 너무 짧아. 원문에 있는 세부 항목까지 다 넣어줘.
```

**이게 Level 1 끝.** 이 상태로도 회의록·주간보고 정도는 충분함.

---

# 3부. Level 2 — GitHub Skill로 PPT 만들기

**목표: `slide-master` 저장소의 `ppt-master` 스킬로 PPT 하나 뽑기.**

## 3-1. slide-master가 뭐야?

- 위치: `https://github.com/byungjunjang/slide-master`
- 정체: PPT를 만드는 데 필요한 **Skill 묶음**이 들어있는 GitHub 저장소
- 오늘 쓸 핵심 Skill: **`ppt-master`**

0-5에서 이미 Fork 했으면 → 바로 3-3으로.

---

## 3-2. Fork 안 했으면 지금 Fork

0-5의 A 방법 다시 보고 Fork 완료 후 이 섹션으로 돌아옴.

---

## 3-3. 저장소가 뭐 하는지 먼저 물어보기

**아무 저장소나 바로 실행하면 안 됨.** 먼저 뭐 하는지 확인.

Claude Code 채팅창에:

```text
이 저장소에 어떤 스킬들이 있는지 요약해줘.
설치·수정은 아직 하지 말고, 구조와 각 스킬이 뭐 하는지만 알려줘.

특히:
- PPT를 새로 만드는 핵심 스킬 이름
- 회사 템플릿을 유지하면서 채우는 스킬이 따로 있는지
- 실행 전에 설치해야 하는 게 있는지
- 초보자가 조심할 점
```

Claude가 요약해줌. `ppt-master` 스킬이 있다는 확인이 나오면 다음 단계.

---

## 3-4. 원본 자료 넣기

### 방법 1 — 파일 첨부 (간단)

2-2와 같음. 채팅창 아래 파일 첨부 아이콘 → 원본 PDF 선택.

### 방법 2 — 저장소 안에 넣기 (재사용용)

채팅창에:

```text
저장소 안에 projects/first-ppt/sources/ 폴더 만들고,
방금 첨부한 파일을 실적보고서.pdf 라는 이름으로 그 안에 저장해줘.
```

Claude가 폴더 만들고 파일 넣어줌. 다음부턴 경로로 참조 가능.

---

## 3-5. PPT 만들기 — 짧은 버전

```text
ppt-master 스킬을 사용해서
projects/first-ppt/sources/실적보고서.pdf로 PPT 만들어줘.
```

이 정도만 해도 초안이 나옴. 30초~3분 기다림.

---

## 3-6. PPT 만들기 — 자세한 버전 (실무용)

조건을 붙이는 버전:

```text
ppt-master 스킬을 사용해서
projects/first-ppt/sources/실적보고서.pdf로
10장 내외의 임원 보고용 PPT를 만들어줘.

조건:
- 한국어
- 한 슬라이드에 핵심 메시지 하나
- 핵심 KPI와 숫자는 크게 강조
- 표나 차트가 적합한 부분은 시각화
- 화이트 배경 + 네이비 포인트
- 여백 넓게, 장식보다 정보 전달 우선
- 자료에 어울리는 한국어 지원 폰트 선택
- 필요한 폰트가 없으면 신뢰할 수 있는 오픈소스에서 준비
- 사용한 폰트 파일과 출처를 outputs/fonts/ 에 함께 저장
- 최종 결과는 PowerPoint에서 편집 가능한 PPTX
- 완성 후 글자 겹침, 넘침, 폰트 깨짐 검사
- 결과 파일: outputs/first-ppt.pptx
```

---

## 3-7. 결과 확인

### 순서

1. Claude가 "PPT를 생성했습니다"라고 하면 → **outputs/first-ppt.pptx** 링크 확인
2. 파일 다운로드
3. PowerPoint(또는 Keynote)에서 열어보기
4. 슬라이드 수·내용·폰트 확인

### 문제 있으면

부분 수정 프롬프트:

```text
3페이지 숫자가 잘 안 보여. 폰트 크기 두 배로 키워줘.
5페이지 표를 막대그래프로 바꿔줘.
7페이지가 너무 텍스트 많아. 두 장으로 나눠줘.
수정 후 outputs/first-ppt.pptx 다시 만들어줘.
```

---

## 3-8. GitHub에 자동 저장되는 흐름

Claude Code가 파일을 만들면 자동으로 이런 일이 벌어짐:

```text
1. 격리 VM에서 파일 생성
2. 자동으로 새 브랜치 생성 (예: claude/first-ppt-xxx)
3. 브랜치에 커밋 + push
4. GitHub에 PR(Pull Request) 초안 생성
5. Claude가 "PR 링크: https://github.com/..." 알려줌
```

### 사용자가 할 일 (한 번)

1. 알려준 **PR 링크** 클릭 → GitHub 웹에서 열림
2. **"Files changed"** 탭에서 변경 내용 확인
3. **"Merge pull request"** → **"Confirm merge"** 클릭
4. 이제 main 브랜치에 반영됨

**두 번째부터는 자동 merge 옵션으로 넘길 수도 있음 (부록 A 참조).**

---

## 3-9. Level 2 정리

| 배운 것 | 프롬프트 |
|---|---|
| 저장소 살펴보기 | `이 저장소 스킬들 요약만 해줘` |
| 자료 넣기 | 파일 첨부 or `~ 경로에 저장해줘` |
| PPT 만들기 | `ppt-master 스킬로 ~ 만들어줘` |
| 부분 수정 | `N페이지 ~ 바꿔줘` |

**여기까지가 Level 2.** 단발성 PPT면 여기서 끝내도 됨. 매달 반복할 거면 Level 3으로.

---

# 4부. Level 3 — PPT 파이프라인 서브에이전트 만들기

**목표: PPT 만드는 과정을 4개 역할로 나눠 서브에이전트로 만들어두기 → 다음 달에도 그대로 재사용.**

## 4-1. 왜 파이프라인으로 나누나?

Level 2에서는 `ppt-master` 스킬 하나가 다 처리했음. 이걸 왜 나누냐면:

| 나누면 좋은 점 | 설명 |
|---|---|
| 각 단계 재실행 | "슬라이드 구조는 좋은데 문구만 다시" 가능 |
| 중간 산출물 저장 | JSON으로 저장되니 나중에 수정·비교 편함 |
| 놓치는 게 줄어듦 | 각 담당자가 자기 일에만 집중 |
| 재사용 | 다음 달에도 같은 파이프라인 그대로 사용 |
| 확장 가능 | 원하는 역할(번역·톤 체크 등) 추가 쉬움 |

**단발성이면 굳이 안 나눠도 됨.** 여기서부터는 매달 반복하거나 팀 표준화가 목표인 사람용.

---

## 4-2. 4개 에이전트 개요

| # | 이름 | 뭐 하는 애 | 입력 | 출력 |
|---|---|---|---|---|
| 1 | **content-planner** | 원본 읽고 핵심 뽑고 슬라이드 구조까지 설계 | 원본 문서 | 슬라이드 구조 JSON |
| 2 | **slide-writer** | 슬라이드별 실제 문구 작성 | 구조 JSON | 문구 JSON |
| 3 | **ppt-builder** | `ppt-master` 스킬로 실제 PPTX 파일 생성 | 문구 JSON | PPTX 파일 |
| 4 | **ppt-reviewer** | 원본과 대조해 검수 | 원본 + PPTX | 검수 리포트 |

### 흐름도

```text
원본 문서
    ↓
[1. content-planner]  → 01-structure.json
    ↓
[2. slide-writer]     → 02-copy.json
    ↓
[3. ppt-builder]      → outputs/final.pptx
    ↓
[4. ppt-reviewer]     → 검수 리포트
    ↓
문제 있으면 특정 단계만 다시 실행
```

---

## 4-3. 만들기 전에 — 저장 위치 이해

서브에이전트는 저장소 안 특정 폴더에 `.md` 파일로 저장됨.

```text
slide-master/                        ← 내가 Fork한 저장소
├── .claude/
│   ├── skills/
│   │   └── ppt-master/              ← 이미 있는 스킬
│   │       └── SKILL.md
│   │
│   └── agents/                      ← 여기 4개 파일이 생김
│       ├── content-planner.md
│       ├── slide-writer.md
│       ├── ppt-builder.md
│       └── ppt-reviewer.md
│
└── projects/
    └── first-ppt/
        └── sources/
            └── 실적보고서.pdf
```

### 폴더 미리 만들어야 하나?

**아니.** Claude Code가 알아서 `.claude/agents/` 폴더 만들고 파일 넣어줌. 니가 브랜치·커밋·PR도 신경 안 써도 됨 (3-8과 같음).

---

## 4-4. Step 1: content-planner 만들기

### 프롬프트 (복사·붙여넣기)

```text
content-planner 라는 프로젝트용 서브에이전트를 만들어줘.

저장 위치:
.claude/agents/content-planner.md

역할:
- 원본 문서(PDF/DOCX/Markdown)를 읽고 핵심 정보를 추출한 뒤
  슬라이드 구조까지 설계한다.
- 결과는 JSON 형식으로 반환:
  {
    "title": "문서 제목",
    "audience": "예상 청중 (임원/실무 등)",
    "key_messages": ["핵심 메시지 1", "핵심 메시지 2", ...],
    "key_numbers": [{"label": "매출", "value": "120억"}, ...],
    "slides": [
      {"no": 1, "role": "표지", "title": "...", "note": "..."},
      {"no": 2, "role": "목차", ...},
      {"no": 3, "role": "핵심 메시지", "key_message_ref": 0, ...},
      ...
    ]
  }

설계 원칙:
- 한 슬라이드에 핵심 메시지 하나
- KPI/숫자는 별도 슬라이드로 강조
- 표지·목차·마무리 슬라이드 포함
- 총 10장 내외 (필요시 조정)

사용 도구: Read

중요:
- 실제 문구를 쓰지 말 것 (slide-writer의 일)
- 원본에 없는 사실을 만들지 말 것
- 오직 정보 추출 + 구조 설계만
```

Enter 누르면 Claude가 파일 만들고 PR 링크 알려줌.

### 확인

Claude가 PR 만들었다고 하면:
1. PR 링크 클릭 → **Merge pull request** → **Confirm**
2. 채팅에서 아래 프롬프트로 확인:

```text
지금 이 저장소에 어떤 서브에이전트가 있는지 목록 보여줘.
```

`content-planner`가 목록에 뜨면 → **완료**.

---

## 4-5. Step 2: slide-writer 만들기

### 프롬프트

```text
slide-writer 라는 프로젝트용 서브에이전트를 만들어줘.

저장 위치:
.claude/agents/slide-writer.md

역할:
- content-planner의 구조 JSON을 받아
  슬라이드별 실제 문구를 작성한다.
- 결과는 JSON 형식:
  {
    "slides": [
      {
        "no": 1,
        "title": "슬라이드 제목",
        "subtitle": "부제 (선택)",
        "bullets": ["불릿 1", "불릿 2"],
        "caption": "출처·주석 (선택)"
      },
      ...
    ]
  }

카피 원칙:
- 한국어 기업/기관 보고 톤
- 제목은 짧게 (2~7 단어)
- 불릿은 한 줄 원칙, 최대 두 줄
- 숫자는 원본 그대로 유지
- 청중 관점에서 "그래서 뭐?"에 답하는 문장

사용 도구: Read

중요:
- 원본에 없는 사실을 만들지 말 것
- 디자인·폰트를 결정하지 말 것 (ppt-builder의 일)
- 슬라이드 개수·순서를 바꾸지 말 것 (구조는 content-planner가 이미 결정)
```

같은 방식으로 PR merge까지 진행.

---

## 4-6. Step 3: ppt-builder 만들기

**이 에이전트는 `ppt-master` 스킬을 사용함.** 프롬프트에 "사용 스킬" 항목이 들어가는 게 앞의 두 개와 다른 점.

### 프롬프트

```text
ppt-builder 라는 프로젝트용 서브에이전트를 만들어줘.

저장 위치:
.claude/agents/ppt-builder.md

사용 스킬:
- ppt-master (.claude/skills/ppt-master/SKILL.md)

역할:
- slide-writer의 문구 JSON을 입력받아
- ppt-master 스킬을 사용해 실제 PPTX 파일을 생성한다.
- 결과를 outputs/ 폴더에 저장.
- 사용한 폰트 파일도 outputs/fonts/ 에 함께 저장.

디자인 규칙:
- 자료 성격에 맞는 한국어 지원 폰트 선택
- 필요한 폰트가 없으면 신뢰할 수 있는 오픈소스에서 준비
- 화이트 배경 + 포인트 컬러 하나 (기본: 네이비)
- 여백 넓게, 장식보다 정보 전달 우선
- 완성 후 글자 겹침·잘림 자체 검사

사용 도구: Read, Write, Bash

중요:
- 슬라이드 내용을 새로 작성하지 말 것 (slide-writer 결과 그대로 사용)
- 오직 JSON → PPTX 변환만 담당
```

### 왜 "사용 스킬" 항목이 필요한가

`ppt-builder`는 파일을 만드는 일을 함. 근데 "어떻게 만드는지"의 세부 절차는 `ppt-master` 스킬에 이미 잘 정리되어 있음. 그래서 **이 에이전트는 매번 그 스킬을 참조해서 일함**. 프롬프트에 "사용 스킬"을 명시하면 Claude가 에이전트 파일 안에 그 참조를 넣어줌.

---

## 4-7. Step 4: ppt-reviewer 만들기

### 프롬프트

```text
ppt-reviewer 라는 프로젝트용 서브에이전트를 만들어줘.

저장 위치:
.claude/agents/ppt-reviewer.md

역할:
- ppt-builder가 만든 PPTX를 원본 문서와 대조해 검수한다.
- 다음을 확인:
  * 숫자·날짜·고유명사가 원본과 일치하는지
  * 핵심 메시지가 빠지지 않았는지
  * 중복된 내용이 없는지
  * 글자 겹침·잘림·폰트 깨짐이 없는지
- 문제를 슬라이드 번호별로 리포트:
  {
    "issues": [
      {"slide": 3, "type": "숫자 불일치", "detail": "매출 120억 → 원문은 121억"},
      {"slide": 7, "type": "폰트 깨짐", "detail": "제목 폰트가 시스템 기본으로 대체됨"},
      ...
    ],
    "summary": "총 N개 이슈. 즉시 수정 권장 M개."
  }

사용 도구: Read

중요:
- PPTX를 새로 만들지 말 것
- 오직 검수 리포트만 반환
- 발견하지 못한 문제까지 있다고 지어내지 말 것
```

---

## 4-8. 확인 — 4개 다 만들어졌는지

채팅창에:

```text
지금 이 저장소에 있는 모든 서브에이전트 목록과
각각의 역할을 한 줄씩 요약해서 보여줘.
```

4개 다 뜨면:

```text
✓ content-planner  — 원본에서 핵심 추출 + 슬라이드 구조 설계
✓ slide-writer     — 슬라이드별 문구 작성
✓ ppt-builder      — ppt-master 스킬로 실제 PPTX 생성
✓ ppt-reviewer     — 원본과 대조 검수
```

**여기까지 오면 재사용 준비 완료.**

---

## 4-9. 파이프라인 한 번에 실행

이제 진짜 사용. 채팅창에:

```text
아래 파이프라인을 실행해줘.

원본: projects/first-ppt/sources/실적보고서.pdf

1단계: content-planner로 원본 분석 + 슬라이드 구조 설계.
       결과를 projects/first-ppt/artifacts/01-structure.json 에 저장.

2단계: slide-writer로 위 결과를 받아 슬라이드별 문구 작성.
       결과를 projects/first-ppt/artifacts/02-copy.json 에 저장.

3단계: ppt-builder로 위 결과와 ppt-master 스킬을 사용해
       outputs/first-ppt.pptx 생성. 폰트도 outputs/fonts/ 에 저장.

4단계: ppt-reviewer로 원본과 최종 PPTX를 대조 검수.
       리포트를 projects/first-ppt/artifacts/04-review.json 에 저장.

검수에서 문제 발견되면 문제 있는 단계만 다시 실행해서 수정해줘.
```

Claude가 순차로 실행 (5~15분 소요). 각 단계 결과가 파일로 저장됨.

### 파이프라인 실행 후 확인

1. `outputs/first-ppt.pptx` 다운로드 → PowerPoint에서 확인
2. `projects/first-ppt/artifacts/04-review.json` 확인 → 검수 리포트
3. 리포트에 이슈가 있으면 Claude가 자동으로 수정 실행

---

## 4-10. 다음 번 재사용 — 딱 세 줄

두 번째부터는 이 짧은 프롬프트면 끝:

```text
projects/2026-10/실적보고서.pdf 를 원본으로 해서
지난번과 같은 4단계 파이프라인 실행해줘.
결과는 outputs/2026-10-report.pptx.
```

**에이전트는 이미 저장소에 있으니 새로 만들 필요 없음.** 이름만 부르면 그대로 작동.

---

## 4-11. 원하는 에이전트를 더 추가하려면

위 4개는 예시. 니 상황에 필요한 다른 에이전트도 같은 방식으로 만들 수 있음.

### 만드는 프롬프트 템플릿

```text
[에이전트이름] 라는 프로젝트용 서브에이전트를 만들어줘.

저장 위치:
.claude/agents/[에이전트이름].md

(사용 스킬:            ← 스킬을 쓰는 에이전트만
- [스킬이름])

역할:
- [뭘 할지 한 문장]
- [세부 지시 1]
- [세부 지시 2]
- [결과 형식 — JSON이면 스키마 예시까지]

사용 도구: Read (Write, Bash는 필요할 때만)

중요:
- [하지 말 것 1]
- [하지 말 것 2]
```

### 확장 예시

| 상황 | 만들 에이전트 | 뭐 하는 애 |
|---|---|---|
| 영문 청중용 | `slide-translator` | 완성된 슬라이드 JSON을 영어로 번역 |
| 회사 톤 체크 | `brand-tone-checker` | 문구가 회사 브랜드 가이드에 맞는지 검수 |
| 그래프 자동 생성 | `chart-generator` | 숫자 데이터를 받아 차트 이미지 생성 |
| 발표 대본 | `speaker-notes-writer` | 슬라이드마다 3~5줄 발표 대본 작성 |
| 법무 검수 | `legal-reviewer` | 대외 발표용 표현 법무 리스크 체크 |

**뭐가 필요한지 니가 결정하고, 위 템플릿에 넣어서 프롬프트 던지면 끝.**

---

# 5부. 한글 문서(HWP) — Skill 위주

**한글 문서는 서브에이전트 안 만들고 스킬로만 처리.** 단발성이면 이게 더 빠름.

## 5-1. HWP 흐름 이해

Anthropic Document Skills에는 **HWP 직접 생성 스킬이 없음**. 대신:

```text
1. docx 스킬로 DOCX 생성 (회사 양식 유지)
        ↓
2. 다운로드
        ↓
3. 한컴오피스에서 열기
        ↓
4. 다른 이름으로 저장 → HWP 선택
```

이 과정을 그대로 씀.

---

## 5-2. 회사 양식 업로드 (한 번만)

Fork한 저장소에 `templates/` 폴더 만들고 회사 DOCX 양식 업로드.

### 순서

1. Claude Code 채팅창에:

```text
저장소에 templates/ 폴더를 만들고
방금 첨부한 회사양식.docx 파일을
templates/회사보고서양식.docx 로 저장해줘.
```

2. 첨부 아이콘으로 회사 양식 파일 첨부 → 전송

3. Claude가 저장하면 → 다음부턴 이 경로로 참조 가능

---

## 5-3. 만들기 프롬프트

```text
docx 스킬을 사용해서
templates/회사보고서양식.docx 양식을 기반으로
이번 달 실적보고서 내용을 채워줘.

원본 자료: projects/2026-10/실적자료.pdf

규칙:
- 기존 머리말·꼬리말 유지
- 로고 유지
- 제목 스타일 유지
- 표 스타일 유지
- 문서 구조 유지
- 한국어 내용만 새 자료에 맞게 교체
- 숫자·날짜는 원문과 대조

결과: outputs/2026-10-실적보고서.docx
```

Claude가 양식 유지하면서 내용 채워서 DOCX 반환.

---

## 5-4. DOCX → HWP 변환

내 컴퓨터에서 수동으로:

1. 다운로드한 `2026-10-실적보고서.docx` 를 한컴오피스로 열기
2. 상단 메뉴 → **파일** → **다른 이름으로 저장**
3. 파일 형식: **한글 문서 (*.hwp)** 선택
4. 저장

**끝.** DOCX와 HWP 간 서식이 완벽하게 유지되지 않을 수 있으니 열어서 한 번 눈으로 확인.

---

## 5-5. 한글 문서에도 검수 에이전트 붙이려면

4부에서 만든 `ppt-reviewer`는 PPT 전용. DOCX/HWP 검수용을 원하면 같은 패턴으로 만들면 됨:

```text
doc-reviewer 라는 프로젝트용 서브에이전트를 만들어줘.

저장 위치:
.claude/agents/doc-reviewer.md

역할:
- 완성된 DOCX를 원본 자료와 회사 양식(templates/회사보고서양식.docx)에 대조해 검수
- 제목/표/문단 스타일이 양식과 다른 부분
- 숫자·날짜·고유명사 원본 대조
- 발견 문제를 페이지·항목별로 리포트

사용 도구: Read

중요:
- 문서를 새로 작성하지 말 것
- 검수 리포트만 반환
```

이후 사용:

```text
docx 스킬로 이번 달 실적보고서 DOCX 만들고
doc-reviewer 서브에이전트로 검수까지 해줘.
```

---

# 6부. 두 번째부터는 이렇게

강의 후 실제로 반복 사용할 때의 최소 순서.

## 6-1. PPT 만들기 (파이프라인 사용)

```text
projects/2026-11/실적보고서.pdf 를 원본으로
지난번과 같은 파이프라인(content-planner → slide-writer → ppt-builder → ppt-reviewer)
실행해줘.

결과: outputs/2026-11-report.pptx
```

## 6-2. 한글 문서 만들기

```text
docx 스킬로 templates/회사보고서양식.docx 양식 유지하면서
projects/2026-11/실적자료.pdf 내용 채워줘.

결과: outputs/2026-11-실적보고서.docx
```

DOCX 다운로드 → 한컴에서 HWP로 저장.

## 6-3. 매번 사람이 마지막으로 확인할 것

| # | 확인 항목 |
|---|---|
| 1 | 숫자·날짜·고유명사가 원본과 맞나 |
| 2 | 핵심 메시지가 빠지지 않았나 |
| 3 | 폰트 깨짐 (PowerPoint/한컴에서 실제로 열어봄) |
| 4 | 대외 문서면 법무·홍보 리뷰 |
| 5 | 회사 기밀은 Private 저장소에만 |

AI 자동화는 **초안을 빠르게 만들고 반복을 줄이는 도구**. 최종 문서는 반드시 사람이 확인.

---

# 부록 A. 자주 막히는 상황

| 증상 | 원인 | 해결 |
|---|---|---|
| Claude Code 저장소 목록에 Fork한 게 안 뜸 | 권한 새로고침 필요 | 저장소 선택 화면에서 새로고침 아이콘 클릭. 안 되면 0-4의 5번을 다시 진행 |
| 프롬프트 던졌는데 5분 넘게 답 없음 | VM 시작 지연 | 새로고침. 그래도 안 되면 새 task 열기 |
| PR이 안 만들어짐 | main 브랜치 보호 규칙 | GitHub 저장소 Settings → Branches → main 보호 해제 (개인 저장소면 불필요) |
| PPT 열었더니 폰트가 시스템 기본으로 대체됨 | 폰트 임베드 미지원 | outputs/fonts/ 에서 폰트 파일 다운로드 → 내 컴퓨터에 설치 후 PPT 재열기 |
| 에이전트가 스킬을 못 찾음 | 저장소 다른 것 열려 있음 | Claude Code 상단에서 저장소가 slide-master인지 확인 |
| PR 매번 눌러 merge하기 귀찮음 | 자동 merge 미설정 | GitHub 저장소 → Settings → General → "Allow auto-merge" 켜기. 이후 PR 만들 때 자동 merge 옵션 사용 가능 |
| 채팅창에 이전 대화가 이어짐 | 세션 지속 | 완전 새 작업하려면 좌측 "New task" 클릭 |

---

# 부록 B. 프롬프트 치트시트

## 셋업

```text
# 저장소 확인
지금 어떤 저장소에 연결되어 있는지, 파일 목록 보여줘.

# 에이전트 목록
이 저장소에 있는 서브에이전트 목록과 역할 한 줄씩 요약해줘.
```

## Level 1 — Skill만

```text
# Document Skills 설치 (한 번만)
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills

# DOCX 만들기
docx 스킬로 첨부한 자료를 한국어 업무보고 DOCX로 만들어줘.
```

## Level 2 — GitHub Skill

```text
# 저장소 스킬 살펴보기
이 저장소 스킬들을 요약만 해줘. 설치·수정은 하지 마.

# PPT 만들기 (짧게)
ppt-master 스킬로 [경로/파일명] 로 PPT 만들어줘.

# 부분 수정
N페이지 [뭘] [어떻게] 바꿔줘.
```

## Level 3 — 서브에이전트

```text
# 에이전트 만들기 (템플릿)
[이름] 서브에이전트 만들어줘.
저장 위치: .claude/agents/[이름].md
(사용 스킬: [스킬이름])
역할:
- ...
사용 도구: Read
중요:
- 새로 만들지 말고 결과만 반환

# 에이전트 호출
[이름] 서브에이전트로 [뭐] 해줘.

# 파이프라인 실행
[원본] 을 [1번 에이전트] → [2번] → [3번] → [4번] 순서로 실행해줘.
각 단계 결과는 artifacts/ 폴더에 저장.
```

## 한글 문서

```text
# 양식 유지 DOCX
docx 스킬로 templates/[양식파일] 유지하면서 [원본] 내용 채워줘.
```

---

# 부록 C. 심화 — 원하는 사람만

## C-1. 에이전트를 다른 프로젝트에서도 재사용

지금 만든 에이전트는 `slide-master` 저장소 안에만 있음. 다른 프로젝트에서도 쓰려면:

### 방법 1. 글로벌 에이전트로 옮기기

```text
지금 .claude/agents/ 안에 있는 4개 에이전트를
~/.claude/agents/ (사용자 글로벌 폴더)로 복사해줘.
```

이후 어떤 저장소를 열어도 이 4개 에이전트 사용 가능.

### 방법 2. 에이전트 전용 저장소 만들기

```text
my-agents 라는 새 저장소를 만들어서
지금 만든 4개 에이전트 md 파일을 복사해줘.
다른 프로젝트에서 참조할 수 있게.
```

---

## C-2. 파이프라인 각 단계만 재실행

전체를 다시 돌리지 않고 특정 단계만:

```text
projects/2026-10/artifacts/02-copy.json 을 열어서 3번 슬라이드 문구를 수정하고,
ppt-builder 부터 다시 실행해서 outputs/2026-10-report.pptx 재생성해줘.
```

## C-3. 여러 원본을 한 번에

```text
projects/2026-Q3/sources/ 폴더 안에 있는 3개 PDF 각각에 대해
파이프라인 실행해줘. 결과는 outputs/2026-Q3/ 폴더에 개별 저장.
```

## C-4. 팀에 파이프라인 공유

- Fork한 저장소를 팀 GitHub Org으로 옮김
- 팀원이 이 저장소 열면 → 4개 에이전트 자동 사용 가능
- 에이전트 업데이트하면 모든 팀원에게 반영

## C-5. 멀티에이전트 확장

지금 파이프라인은 순차 실행. 더 나아가면:

```text
lead-orchestrator 에이전트를 만들어서
필요에 따라 위 4개 에이전트를 병렬 호출하도록 만들어줘.
```

이 방식이 진짜 멀티에이전트 시스템. 초보 단계에서는 필요 없음.

---

## 참고 저장소

- `slide-master`: https://github.com/byungjunjang/slide-master
- Anthropic Skills: https://github.com/anthropics/skills

## 오늘 핵심 한 줄

> **Skill은 일하는 방법, Subagent는 특정 역할 담당자.**
> 잘 만든 Skill은 가져와 쓰고, 필요한 Agent는 프롬프트 한 번으로 만들어 저장소에 축적한다.
> 두 번째부턴 이름만 부르면 그대로 작동.
