# fs17-collab-playground

풀스택 17기 **Git/GitHub 협업하기** 실습 레포입니다.
쓰기 권한이 없는 남의 레포에 **Fork**로 기여하고, 짝과 **코드 리뷰**를 주고받고,
**GitHub Actions**가 내 PR을 검사하는 과정을 한 번에 해 봅니다.

각자 `members/` 폴더에 **자기 파일 하나만** 추가하므로 PR끼리 충돌하지 않습니다.

## 준비

- GitHub에 로그인해 둡니다.
- Windows는 **Git Bash**, Mac은 터미널을 씁니다.
- 파일은 VS Code로 만들고 고칩니다. PowerShell의 `echo`로 만들면 UTF-16으로 저장되어 CI가 읽지 못합니다.

## 실습 순서

| 단계 | 어디서 | 할 일 |
|---|---|---|
| 1 | GitHub | 이 레포 오른쪽 위 **Fork** → **Create fork** |
| 2 | 터미널 | **내 계정의** 복사본을 clone하고 `feat/intro` 브랜치 만들기 |
| 3 | VS Code | `members/_template.py`를 복사해 `members/<내 아이디>.py`로 저장하고 내용 채우기 |
| 4 | VS Code | 마지막 줄을 `print(   intro    )`로 바꿔 **일부러 규칙 어기기** |
| 5 | 터미널 | add · commit · push |
| 6 | GitHub | **Compare & pull request** → base가 `seoyong-lee/fs17-collab-playground`의 `main`인지 확인 → PR 만들기 |
| 7 | GitHub | PR 아래 검사 칸이 **✕ style-check**로 바뀌면 **Details**에서 flake8 메시지 읽기 |
| 8 | VS Code · 터미널 | 공백을 지워 고친 뒤 commit · push → 같은 PR에 커밋이 추가되고 **✓** |
| 9 | GitHub | 짝의 PR → **Files changed** → 한 줄 이상 코멘트 → **Submit review** |
| 10 | GitHub | 받은 코멘트에 답하거나 Suggestion을 **Commit suggestion**으로 반영 |

강사가 PR을 머지하면 보라색 **Merged**로 바뀝니다.

### 터미널 명령 (2 · 5 · 8단계)

```bash
cd ~/Desktop
git clone https://github.com/<내 아이디>/fs17-collab-playground.git
cd fs17-collab-playground
git remote -v                      # origin이 <내 아이디>인지 확인
git switch -c feat/intro

cp members/_template.py members/<내 아이디>.py
# VS Code로 파일을 열어 값을 바꾸고 마지막 줄에 공백 넣기

git add members/
git commit -m "feat(members): add <내 아이디> intro"
git push origin feat/intro

# CI ✕ 확인 후 공백을 지우고
git add members/
git commit -m "fix(members): remove extra spaces"
git push origin feat/intro
```

### PR 만들 때 확인할 것

```
base repository: seoyong-lee/fs17-collab-playground   base: main
head repository: <내 아이디>/fs17-collab-playground       compare: feat/intro
```

본문은 PR 템플릿이 자동으로 채워 줍니다. 체크리스트의 `[ ]`는 `[x]`로 바꾸면 체크됩니다.

## 규칙

- `members/` 안의 **내 파일만** 추가하고 고칩니다. 다른 파일을 바꾼 PR은 머지하지 않습니다.
- 파일 이름은 **GitHub 아이디**와 같게 씁니다. 예: `members/hong-gildong.py`
- 한 줄은 **79자**를 넘기지 않습니다. flake8 기본 규칙입니다.
- 커밋 메시지는 `type(scope): subject` 형식으로 씁니다. 예: `feat(members): add hong-gildong intro`
- 리뷰는 **코드**를 봅니다. 사람을 평가하지 않습니다.

## 자주 막히는 곳

| 증상 | 원인과 해결 |
|---|---|
| push하면 `403` 또는 `Permission denied` | 원본(`seoyong-lee`) 주소를 clone했습니다. `git remote -v`로 확인하고 **내 Fork**를 다시 clone합니다. |
| PR의 base가 내 레포로 잡힘 | base repository 드롭다운에서 `seoyong-lee/fs17-collab-playground`를 고릅니다. |
| 검사 칸에 **Waiting for approval** 또는 승인 대기 문구 | 처음 기여하는 계정은 원본 관리자가 실행을 승인해야 CI가 돕니다. 강사가 승인할 때까지 기다립니다. |
| `W292 no newline at end of file` | 파일 마지막 줄 끝에서 Enter를 한 번 누르고 저장합니다. |
| `W391 blank line at end of file` | 파일 끝의 빈 줄을 하나만 남깁니다. |
| `E501 line too long` | 79자를 넘은 줄을 짧게 줄입니다. |
| 짝이 Approve했는데 머지 버튼이 회색 | 브랜치 보호 규칙은 **쓰기 권한이 있는 사람**의 승인만 셉니다. 이 레포에서는 강사 승인이 필요합니다. |
| 머지된 뒤 내 Fork가 뒤처짐 | 내 Fork 화면의 **Sync fork** → **Update branch**를 누르고 `git switch main` → `git pull origin main` |

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
fs17-collab-playground/
├── .github/
│   ├── CODEOWNERS                  # 리뷰어 자동 지정
│   ├── PULL_REQUEST_TEMPLATE.md    # PR 본문 양식
│   └── workflows/
│       └── python-code-style.yaml  # PR마다 flake8 검사 (job: style-check)
├── members/
│   ├── _template.py                # 복사해서 쓰는 틀
│   └── seoyong-lee.py                # 예시
├── INSTRUCTOR.md                   # 강사용 레포 설정
└── README.md
```
