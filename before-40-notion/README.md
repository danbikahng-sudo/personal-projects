# BEFORE 40 — Notion 구축 가이드

40살(2033년)까지의 버킷리스트를 하나의 Notion 프로젝트로 만들기 위한 파일 모음.
**마스터 데이터베이스 1개 + 필터된 뷰 여러 개** 구조.

```
BEFORE 40                     ← 최상위 페이지 (BEFORE 40.md)
├── 00. Dashboard             ← 전체 현황 (링크된 뷰 모음)
├── 01. All Bucket List       ← 마스터 DB (All Bucket List.csv)
├── 02. Yearly Focus          ← 연도별 집중 목표
├── 03. Completed Memories    ← 달성한 것들의 갤러리
└── 04. Annual Review         ← 연말 회고 템플릿
Bucket Item Template.md       ← DB 항목 상세 페이지 템플릿 (복붙용)
```

---

## Step 1. 가져오기 (Import)

1. 이 `before-40-notion` 폴더를 zip으로 압축.
2. Notion 왼쪽 사이드바 → **Import** → **Text & Markdown** 선택 → zip 업로드.
   - `BEFORE 40` 페이지 아래에 하위 페이지들과 `All Bucket List` 데이터베이스가 생김.
   - CSV만 따로 넣고 싶으면 Import → **CSV**로 `All Bucket List.csv`를 올리면 됨.
3. 데이터베이스 이름을 `01. All Bucket List`로 바꾸고, 하위 페이지 순서를 00 → 04로 정렬.

> CSV에는 대화에서 나온 항목 8개만 들어 있음. 나머지 73개 항목은 CSV에 행을 추가하거나 Notion에 바로 입력하면 됨.

## Step 2. 속성 유형 바꾸기

CSV로 가져오면 대부분 Text로 들어오므로, 각 열 머리글 → **Edit property** → 유형 변경.

| 속성 | 유형 | 옵션 |
|---|---|---|
| Name | Title | — |
| Category | Select | 아래 9개 + 색상 |
| Status | Status | Wishlist / Planned / In Progress / Done |
| Priority | Select | Must (Red) / Want (Yellow) / Someday (Gray) |
| Target Year | Number | 형식: Number (쉼표 없이) |
| Due Date | Date | — |
| Completed Date | Date | — |
| Cover | Files & media | **새로 추가** (CSV로는 못 가져옴) |
| Memory | Text | — |
| Completion Score | Formula | **새로 추가** — Step 4 |

**Status 그룹 설정:** Status 속성 편집에서
- *To-do* 그룹: `Wishlist`, `Planned`
- *In progress* 그룹: `In Progress`
- *Complete* 그룹: `Done`
(기본 옵션 Not started / In progress / Done은 삭제하거나 이름 변경)

**Category 색상:**

| Category | 색상 |
|---|---|
| Career & Achievement | Blue |
| Financial Freedom | Green |
| Health & Body | Red |
| Surfing & Outdoor | Teal 계열 → Notion에 Teal이 없으므로 **Blue**와 겹치지 않게 **Brown** 또는 Default 추천 |
| Travel & Experiences | Purple |
| Intellectual Growth | Yellow |
| Taste & Lifestyle | Pink |
| Love & Relationships | Orange |
| Freedom & Life Design | Gray |

> Notion Select 색상은 Default, Gray, Brown, Orange, Yellow, Green, Blue, Purple, Pink, Red 10가지라 Teal이 없음.

## Step 3. 뷰 6개 만들기

데이터베이스 상단 **+ (Add view)**로 추가.

| # | 뷰 이름 | 유형 | 설정 |
|---|---|---|---|
| ① | All List | Gallery | Card preview: **Cover** · Card size: Medium · 표시: Category, Status |
| ② | Progress | Board | Group by: **Status** (Wishlist → Planned → In Progress → Done 순) · 표시: Category, Target Year |
| ③ | All Items | Table | 열: Name, Category, Status, Priority, Target Year, Due Date · Sort: Target Year ↑, Priority |
| ④ | By Year | Board | Group by: **Target Year** · Sub-group(선택): Status |
| ⑤ | Completed | Gallery | Filter: **Status = Done** · Card preview: Cover · 표시: Completed Date, Memory · Sort: Completed Date ↓ |
| ⑥ | By Category | Board | Group by: **Category** · 표시: Status, Priority |

Board에서 Number 속성(Target Year)으로 그룹화가 안 되는 경우, Target Year를 **Select**(2027~2033)로 만들어도 됨.

## Step 4. 전체 달성률 (Completion Score)

1. 새 속성 추가 → 이름 `Completion Score` → 유형 **Formula**
2. 수식:

   ```
   if(prop("Status") == "Done", 1, 0)
   ```

3. **All Items** 테이블 뷰 맨 아래에서 Completion Score 열 → **Calculate → Average**.
4. 속성 편집 → Number format → **Percent**.
   - 예: 73개 중 18개 완료 → **24.7%**
5. **By Category** 보드에서도 각 그룹 하단의 계산을 Average로 → 분야별 달성률.

추가 팁: Status 열 하단 Calculate → **Percent per group → Complete** 를 써도 수식 없이 달성률이 나옴.

진행률 바로 보고 싶다면 대시보드에 **Chart 뷰**(Donut, Group by Status) 추가.

## Step 5. 상세 페이지 템플릿

1. 데이터베이스 우측 상단 **New ▾ → + New template**.
2. 템플릿 이름: `Bucket Item`.
3. `Bucket Item Template.md`의 내용(Why / Planning / My Memories / Reflection)을 붙여 넣기.
4. 템플릿 메뉴 `···` → **Set as default** → 이후 모든 새 항목에 자동 적용.
5. 기존 항목에는 각 페이지를 열어 빈 본문에서 템플릿 선택.

## Step 6. Dashboard 연결

`00. Dashboard` 페이지에서 `/linked view of database` → `All Bucket List` 선택 후, 페이지에 적힌 안내대로 뷰를 하나씩 넣기:

- **My 40 Before Progress** — All Items(테이블), 하단 Average 표시. 또는 Chart(Donut).
- **This Year's Focus** — Filter: Target Year = 2027
- **In Progress** — Filter: Status = In Progress
- **Recent Memories** — Completed 갤러리, 3~6개만
- **Balance by Category** — By Category 보드

`02. Yearly Focus`, `03. Completed Memories` 페이지에도 같은 방식으로 링크된 뷰를 넣으면 됨.

---

## 구축 체크리스트

- [ ] BEFORE 40 상위 페이지 만들기 (Import)
- [ ] All Bucket List 데이터베이스 만들기
- [ ] 73개 버킷리스트 입력하기
- [ ] 9개 카테고리와 상태 설정하기
- [ ] 6개 데이터베이스 뷰 만들기
- [ ] Completion Score 수식 설정하기
- [ ] 상세 페이지 템플릿 만들기
- [ ] Dashboard에 전체 현황 연결하기

> This is not a to-do list. This is a list of things that make life worth living.
