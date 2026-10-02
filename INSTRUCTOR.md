# 강사용 레포 설정

수업 전에 한 번 해 둡니다. 순서가 중요합니다. `style-check`는 워크플로우가 한 번 돌아야
브랜치 보호 설정의 검사 목록에 나타납니다.

## 1. 레포 만들고 올리기

레포 이름은 `fs17-collab-playground`입니다. 슬라이드와 README가 모두 이 주소를 씁니다.
이미 레포가 있으면 이름만 확인하고 2번으로 넘어갑니다.

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

3. **Actions** 탭에서 `HTML CI with HTMLHint`가 한 번 돌고 ✓가 뜨는지 확인합니다.

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

수강생 전원이 이 레포에 처음 기여하는 계정입니다. 승인 설정이 엄격하면 PR마다 강사가
**Approve and run**을 눌러야 CI가 돌아서 실습 시간 안에 끝나지 않습니다.

- **Approval for running fork pull request workflows from contributors**에서
  가장 느슨한 옵션인 **Require approval for first-time contributors who are new to GitHub**를 고릅니다.
  (가입한 지 얼마 안 된 계정만 승인이 필요합니다.)
- 그래도 승인 대기 PR이 생기면 **Pull requests** 목록에서 해당 PR을 열어 **Approve and run**을 누릅니다.
- `pull_request` 이벤트로 도는 워크플로우는 Fork PR에서 읽기 권한만 받으므로 이 설정을 느슨하게 해도 레포에 쓰기는 하지 못합니다.

## 5. 수업 중

### 실습 2-A (4교시 · 챕터 3 코드 리뷰)

1. 수업 전에 짝을 정해 채팅에 올립니다. 홀수면 3인 1조로 서로 한 명씩 리뷰합니다.
2. 레포 주소를 채팅에 올립니다.
3. 터미널에서 막힌 수강생은 README의 **웹에서만 PR 만들기**로 보냅니다.
4. 이 단계에서는 **머지하지 않습니다.** 2-B에서 같은 PR을 다시 씁니다.
5. 다른 파일을 바꾼 PR은 **Request changes**로 되돌려 보냅니다.

### 실습 2-B (6교시 · 챕터 5 협업 자동화)

1. CI 장을 설명한 뒤 같은 PR에 `</p>` 하나를 지운 커밋을 올리게 합니다.
2. ✕로 바뀐 PR 하나를 화면에 띄우고 **Details**의 HTMLHint 메시지(`Tag must be paired` · `tag-pair`)를 함께 읽습니다. `L12`가 줄 번호, `^`가 위치입니다.
3. 머지 버튼이 **Merging is blocked**로 막힌 모습을 보여 주고 "CI 통과를 필수로 만들기" 장으로 연결합니다.
4. ✓로 돌아온 PR은 **Approve** → **Squash and merge**로 합칩니다.

### 6교시 CI 시연 (실습 흐름 장)

슬라이드의 `feat/create-test` 시연용 파일입니다. 이 레포에서 브랜치를 만들어 그대로 씁니다.

```bash
git switch -c feat/create-test
# test.html 을 아래 내용으로 만들고 <h1>codeit 뒤의 </h1>을 일부러 뺍니다
git add test.html
git commit -m "test: add test.html without closing tag"
git push origin feat/create-test
# PR → ✕ 확인 → </h1>을 넣어 다시 push → ✓ → PR은 머지하지 않고 Close
```

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>CI 테스트</title>
</head>
<body>
  <h1>codeit
</body>
</html>
```

## 6. 수업 뒤

- 남은 PR은 머지하거나 코멘트를 남기고 닫습니다.
- 다음 기수에 다시 쓰려면 `members/`에서 `_template.html`과 `seoyong-lee.html`만 남기고 지운 커밋을 push합니다.
