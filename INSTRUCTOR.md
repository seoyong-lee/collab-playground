# 강사용 레포 설정

수업 전에 한 번 해 둡니다. 순서가 중요합니다. `style-check`는 워크플로우가 한 번 돌아야
브랜치 보호 설정의 검사 목록에 나타납니다.

## 1. 레포 만들고 올리기

1. GitHub에서 **New** → 이름 `fs17-collab-playground` → **Public** → README 체크 없이 **Create repository**
2. 이 폴더에서 아래를 실행합니다.

```bash
git init
git add .
git commit -m "chore: 협업 실습 레포 초기 세팅"
git branch -M main
git remote add origin https://github.com/seoyong-lee/fs17-collab-playground.git
git push -u origin main
```

3. **Actions** 탭에서 `Python CI with flake8`이 한 번 돌고 ✓가 뜨는지 확인합니다.

## 2. Settings → General

- **Pull Requests**: **Allow squash merging**만 켭니다. PR 하나가 커밋 하나로 남아 히스토리가 깔끔합니다.

## 3. Settings → Branches → main 보호 규칙

화면에 Rulesets가 먼저 보이면 **Add classic branch protection rule**을 누릅니다. 항목은 같습니다.

| 설정 | 값 |
|---|---|
| Branch name pattern | `main` |
| Require a pull request before merging | 켬 |
| Require approvals | 1 |
| Require review from Code Owners | 켬 |
| Require status checks to pass before merging | 켬 → 검사 항목에 `style-check` 추가 |

**Do not allow bypassing the above settings**는 끄고 둡니다. 켜면 강사도 규칙을 우회하지 못해 급할 때 곤란합니다.

## 4. Settings → Actions → General

- Fork에서 온 PR의 워크플로우 실행 승인 설정을 확인합니다.
  새로 만든 계정은 첫 PR에서 승인이 필요할 수 있습니다.
- 수업 중에는 **Pull requests** 목록을 수시로 보고, 승인 대기 중인 PR 화면에서 **Approve and run**을 누릅니다.

## 5. 수업 중

1. 레포 주소를 채팅에 올립니다.
2. 8분쯤 PR 목록에서 ✕ → ✓로 바뀐 PR을 골라 화면에 띄우고 flake8 메시지를 함께 읽습니다.
3. 짝끼리 리뷰가 끝난 PR은 **Approve** → **Squash and merge**로 합칩니다.
4. 다른 파일을 바꾼 PR은 **Request changes**로 되돌려 보냅니다.

## 6. 수업 뒤

- 남은 PR은 머지하거나 코멘트를 남기고 닫습니다.
- 다음 기수에 다시 쓰려면 `members/`에서 `_template.py`와 `seoyong-lee.py`만 남기고 지운 커밋을 push합니다.
