# collab-playground

**Git/GitHub 협업하기** 실습 레포입니다.
쓰기 권한이 없는 남의 레포에 **Fork**로 기여하고, 짝과 **코드 리뷰**를 주고받고,
**GitHub Actions**가 내 PR을 검사하는 과정을 두 번에 나눠 해 봅니다.

| 실습                     | 언제                       | 하는 일                                               |
| ------------------------ | -------------------------- | ----------------------------------------------------- |
| **2-A** Fork · PR · 리뷰 | 4교시 (챕터 3 코드 리뷰)   | Fork → 내 파일 추가 → PR 만들기 → 짝과 리뷰 주고받기  |
| **2-B** CI 실패와 통과   | 6교시 (챕터 5 협업 자동화) | 같은 PR에 규칙을 어긴 커밋 올리기 → ✕ 확인 → 고쳐서 ✓ |

각자 `members/` 폴더에 **자기 파일 하나만** 추가하므로 PR끼리 충돌하지 않습니다.
2-A에서 만든 PR은 **2-B가 끝날 때까지 머지하지 않고 열어 둡니다.**

## 준비

- GitHub에 로그인해 둡니다.
- Windows는 **Git Bash**, Mac은 터미널을 씁니다.
- 파일은 VS Code로 만들고 고칩니다. PowerShell의 `echo`로 만들면 UTF-16으로 저장되어 CI가 읽지 못합니다.
- 강사가 정해 준 **짝의 GitHub 아이디**를 메모해 둡니다.

## 실습 2-A · Fork해서 PR 보내고 리뷰하기 (4교시)

| 단계 | 어디서  | 할 일                                                                                                  |
| ---- | ------- | ------------------------------------------------------------------------------------------------------ |
| 1    | GitHub  | 이 레포 오른쪽 위 **Fork** → **Create fork**                                                           |
| 2    | 터미널  | **내 계정의** 복사본을 clone하고 `feat/intro` 브랜치 만들기                                            |
| 3    | VS Code | `members/_template.html`을 복사해 `members/<내 아이디>.html`로 저장하고 글자를 내 정보로 바꾸기        |
| 4    | 터미널  | add · commit · push                                                                                    |
| 5    | GitHub  | **Compare & pull request** → base가 `seoyong-lee/collab-playground`의 `main`인지 확인 → PR 만들기      |
| 6    | GitHub  | PR 아래 검사 칸이 **✓ style-check**가 될 때까지 기다리기 (승인 대기 문구가 보이면 강사가 승인합니다)   |
| 7    | GitHub  | 짝의 PR → **Files changed** → 한 줄 이상 코멘트 → **Review changes** → **Comment** → **Submit review** |
| 8    | GitHub  | 받은 코멘트에 답하거나 Suggestion을 **Commit suggestion**으로 반영하기                                 |

### 터미널 명령 (2 · 4단계)

```bash
cd ~/Desktop
git clone https://github.com/<내 아이디>/collab-playground.git
cd collab-playground
git remote -v                      # origin이 <내 아이디>인지 확인
git switch -c feat/intro

cp members/_template.html members/<내 아이디>.html
# VS Code로 파일을 열어 글자를 내 정보로 바꾸고 저장 (태그는 그대로)

git add members/
git commit -m "feat(members): add <내 아이디> intro"
git push origin feat/intro
```

### 터미널이 막히면 — 웹에서만 PR 만들기

clone이나 push에서 막혀 시간이 부족하면 브라우저만으로도 같은 PR을 만들 수 있습니다.

1. **내 Fork** 화면에서 `members` 폴더로 들어가 **Add file** → **Create new file**을 누릅니다.
2. 파일 이름에 `<내 아이디>.html`을 쓰고 `_template.html` 내용을 붙여 넣은 뒤 글자를 바꿉니다.
3. **Commit changes…** → **Create a new branch** 선택 → 브랜치 이름 `feat/intro` → **Propose changes**를 누릅니다.
4. 이어지는 화면에서 base가 `seoyong-lee/collab-playground`의 `main`인지 확인하고 **Create pull request**를 누릅니다.

2-B도 같은 방법으로 Fork의 `feat/intro` 브랜치에서 파일을 열어 연필 아이콘으로 고치면 됩니다.

### PR 만들 때 확인할 것

```
base repository: seoyong-lee/collab-playground   base: main
head repository: <내 아이디>/collab-playground   compare: feat/intro
```

본문은 PR 템플릿이 자동으로 채워 줍니다. 체크리스트의 `[ ]`를 `[x]`로 바꾸면 체크됩니다.

## 실습 2-B · CI가 막고 고쳐서 통과하기 (6교시)

워크플로우와 CI를 배운 뒤, 2-A에서 열어 둔 **같은 PR**에서 이어 합니다.

| 단계 | 어디서           | 할 일                                                                                 |
| ---- | ---------------- | ------------------------------------------------------------------------------------- |
| 1    | VS Code          | 내 파일의 `한 줄 소개` 문단 끝 `</p>`를 지워 **일부러 규칙 어기기**                   |
| 2    | 터미널           | add · commit · push → 새 PR이 생기지 않고 기존 PR에 커밋이 추가됨                     |
| 3    | GitHub           | 검사 칸이 **✕ style-check**로 바뀌면 **Details**에서 HTMLHint 메시지(`tag-pair`) 읽기 |
| 4    | VS Code · 터미널 | 지운 `</p>`를 다시 넣고 commit · push → **✓**                                         |

```bash
git add members/
git commit -m "feat(members): edit intro"
git push origin feat/intro

# ✕ 확인 후 </p>를 다시 넣고
git add members/
git commit -m "fix(members): close p tag"
git push origin feat/intro
```

강사가 PR을 머지하면 보라색 **Merged**로 바뀝니다.

## 규칙

- `members/` 안의 **내 파일만** 추가하고 고칩니다. 다른 파일을 바꾼 PR은 머지하지 않습니다.
- 파일 이름은 **GitHub 아이디**와 같게 씁니다. 예: `members/hong-gildong.html`
- 태그는 템플릿 그대로 두고 **글자만** 바꿉니다. 여는 태그와 닫는 태그는 짝이 맞아야 합니다.
- 커밋 메시지는 `type(scope): subject` 형식으로 씁니다. 예: `feat(members): add hong-gildong intro`
- 리뷰는 **코드**를 봅니다. 사람을 평가하지 않습니다.

## 자주 막히는 곳

| 증상                                                                                 | 원인과 해결                                                                                          |
| ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| push하면 `403` 또는 `Permission denied`                                              | 원본(`seoyong-lee`) 주소를 clone했습니다. `git remote -v`로 확인하고 **내 Fork**를 다시 clone합니다. |
| PR의 base가 내 레포로 잡힘                                                           | base repository 드롭다운에서 `seoyong-lee/collab-playground`를 고릅니다.                             |
| 검사 칸에 **Waiting for approval** 또는 승인 대기 문구                               | 처음 기여하는 계정은 원본 관리자가 실행을 승인해야 CI가 돕니다. 강사가 승인할 때까지 기다립니다.     |
| `Tag must be paired, missing: [ </p> ]` (tag-pair)                                   | 닫는 태그가 빠졌습니다. 2-B에서 일부러 지운 `</p>`를 다시 넣습니다. 메시지의 `L12`가 줄 번호입니다.  |
| `Doctype must be declared before any non-comment content.` (doctype-first)           | 파일 맨 위 `<!DOCTYPE html>` 줄을 지웠습니다. 템플릿에서 다시 복사합니다.                            |
| `<title> must be present in <head> tag.` (title-require)                             | `<title>` 줄을 지웠습니다. 템플릿처럼 되돌립니다.                                                    |
| `The value of attribute [ ... ] must be in double quotes` (attr-value-double-quotes) | 속성값을 큰따옴표 `"..."`로 감쌉니다.                                                                |
| 짝이 Approve했는데 머지 버튼이 회색                                                  | 브랜치 보호 규칙은 **쓰기 권한이 있는 사람**의 승인만 셉니다. 이 레포에서는 강사 승인이 필요합니다.  |
| 머지된 뒤 내 Fork가 뒤처짐                                                           | 내 Fork 화면의 **Sync fork** → **Update branch**를 누르고 `git switch main` → `git pull origin main` |

## 확인 질문

1. 원본 레포에 쓰기 권한이 없는데도 PR을 보낼 수 있었던 이유는 무엇일까요?
2. 고친 커밋을 push했더니 새 PR이 생기지 않고 기존 PR에 커밋이 추가되었습니다. 왜 그럴까요?
3. PR을 만들자마자 리뷰어가 자동으로 지정된 이유는 무엇일까요?
4. ✕가 떴을 때 머지 버튼이 막힌 것은 어떤 설정 때문일까요?

<details>
<summary>정답 보기</summary>

1. 내 계정에 복사한 Fork에는 쓰기 권한이 있어서 거기에 push했고, PR은 원본에 **합쳐 달라는 요청**만 보내기 때문입니다.
2. PR은 브랜치 단위로 열립니다. 같은 `feat/intro` 브랜치에 push한 커밋은 그 브랜치의 PR에 계속 쌓입니다.
3. `.github/CODEOWNERS`에 책임자가 지정되어 있어서 해당 파일이 바뀐 PR에 책임자가 리뷰어로 자동 할당됩니다.
4. 원본 `main`의 브랜치 보호 규칙에서 **Require status checks to pass before merging**을 켜고 `style-check`를 필수로 골랐기 때문입니다.

</details>

## 파일 구성

```
collab-playground/
├── .github/
│   ├── CODEOWNERS                  # 리뷰어 자동 지정
│   ├── PULL_REQUEST_TEMPLATE.md    # PR 본문 양식
│   └── workflows/
│       └── html-code-style.yaml    # PR마다 HTMLHint 검사 (job: style-check)
├── members/
│   ├── _template.html              # 복사해서 쓰는 틀
│   └── seoyong-lee.html            # 예시
├── .htmlhintrc                     # HTMLHint 규칙 (CI와 로컬이 같은 규칙을 씀)
├── INSTRUCTOR.md                   # 강사용 레포 설정
└── README.md
```
