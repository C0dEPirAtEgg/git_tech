# Git 연습 과제

`practice` 브랜치에서 진행하는 실습입니다. 명령어를 직접 쳐보면서 익히는 것이 목적이라, **결과를 눈으로 확인하는 단계**를 꼭 거치세요.

> 여기서 무엇을 망가뜨려도 `main` 브랜치의 치트시트는 안전합니다. 마음껏 실험하세요.

<br>

## 시작 전 확인

```bash
cd /Users/jaehyun/codepirate/git_tech
git switch practice          # practice 브랜치인지 확인
git status                   # 깨끗한 상태에서 시작
```

<br>

---

## 과제 1 — 기본 흐름 익히기

**목표** : 파일 수정 → 확인 → 스테이징 → 커밋 → 업로드의 한 사이클을 완주합니다.

**해볼 것**

`practice/learning-log.md` 표에 오늘 날짜와 "과제 1"을 한 줄 적고 저장하세요. 그 다음:

```bash
git status                   # 어떤 파일이 바뀌었는지 확인
git diff                     # 무엇이 바뀌었는지 내용 확인
git add practice/learning-log.md
git status                   # 초록색으로 바뀐 것 확인 (스테이징됨)
git commit -m "학습 로그에 과제 1 기록"
git push -u origin practice  # 첫 푸시는 -u 로 연결까지
```

**확인 포인트**

- `git add` 전후로 `git status` 의 색과 문구가 어떻게 달라지는지
- `git diff` 는 `add` 하고 나면 **아무것도 안 나옵니다.** 이미 스테이징된 변경은 `git diff --staged` 로 봐야 합니다

<br>

## 과제 2 — 일부만 골라서 커밋하기

**목표** : 변경한 파일이 여럿일 때 원하는 것만 커밋합니다.

**해볼 것**

`glossary.md` 와 `learning-log.md` **둘 다** 수정하세요. 그리고 용어집만 커밋합니다.

```bash
git status                            # 두 파일 모두 수정됨으로 표시
git add practice/glossary.md          # 하나만 스테이징
git status                            # 한쪽은 초록, 한쪽은 빨강
git commit -m "용어집에 항목 추가"     # 스테이징된 것만 커밋됨
git status                            # learning-log.md 는 아직 남아 있음
```

**실수로 둘 다 add 했다면**

```bash
git restore --staged practice/learning-log.md   # 스테이징만 취소 (수정 내용은 유지)
```

**확인 포인트**

- `restore --staged` 는 파일 내용을 건드리지 않습니다. `add` 만 되돌립니다

<br>

## 과제 3 — 커밋 고치기와 되돌리기

**목표** : 푸시 전에 실수를 바로잡는 세 가지 방법을 구분합니다.

**(1) 커밋 메시지를 잘못 썼을 때**

```bash
git commit -m "오타 있는 메시지"
git commit --amend -m "제대로 된 메시지"   # 직전 커밋을 덮어씀
git log --oneline                          # 커밋이 늘지 않고 메시지만 바뀜
```

**(2) 파일을 빠뜨렸을 때**

```bash
git add practice/learning-log.md
git commit --amend --no-edit    # 메시지는 그대로, 파일만 직전 커밋에 합침
```

**(3) 커밋 자체를 취소하고 싶을 때**

```bash
git log --oneline               # 되돌리기 전 상태 기억해두기
git reset --soft HEAD~1         # 커밋만 취소, 변경 내용은 스테이징에 남음
git status                      # 파일이 초록색으로 살아있음
```

`--soft` / `--mixed` / `--hard` 를 직접 비교해보세요.

| 옵션 | 커밋 | 스테이징 | 파일 내용 |
| --- | --- | --- | --- |
| `--soft` | 취소 | 유지 | 유지 |
| `--mixed` (기본) | 취소 | 취소 | 유지 |
| `--hard` | 취소 | 취소 | **삭제** |

**확인 포인트**

- `--amend` 와 `reset` 모두 **기록을 새로 쓰는** 명령입니다. 이미 푸시했다면 다음 과제의 `revert` 를 쓰세요

<br>

## 과제 4 — 이미 푸시한 커밋 되돌리기 (revert)

**목표** : 남과 공유된 기록을 안전하게 취소합니다.

**해볼 것**

아무 파일이나 고쳐서 커밋하고 **푸시까지** 한 뒤, 그 커밋을 취소해봅니다.

```bash
echo "- 잘못 추가한 줄" >> practice/glossary.md
git commit -am "실수로 넣은 내용"
git push

git log --oneline               # 취소할 커밋의 해시 확인 (앞 7자리)
git revert <해시>                # 편집기가 열리면 그대로 저장 후 종료
git log --oneline               # 취소 커밋이 새로 생김
git push
```

**확인 포인트**

- `reset` 은 기록을 **지우고**, `revert` 는 취소하는 커밋을 **새로 만듭니다**
- 그래서 `revert` 는 강제 푸시가 필요 없습니다. 협업 중에는 이쪽이 원칙입니다

<br>

## 과제 5 — 브랜치 만들고 합치기

**목표** : 작업을 분리했다가 되돌려 합치는 흐름을 익힙니다.

```bash
git switch -c feature/glossary       # 브랜치 생성 + 이동
git branch                           # * 가 새 브랜치에 있는지 확인

# glossary.md 에 용어를 2~3개 추가하고 저장
git commit -am "용어집 항목 보강"

git switch practice                  # 원래 브랜치로 복귀
cat practice/glossary.md             # 추가한 내용이 없음 (분리되어 있음)

git merge feature/glossary           # 합치기
cat practice/glossary.md             # 이제 내용이 반영됨

git branch -d feature/glossary       # 다 쓴 브랜치 정리
```

**확인 포인트**

- 브랜치를 옮기면 **작업 폴더의 파일 내용이 실제로 바뀝니다.** 직접 열어서 확인해보세요

<br>

## 과제 6 — 충돌 만들고 해결하기

**목표** : 충돌이 왜 생기는지 이해하고 직접 해결합니다. 가장 중요한 과제입니다.

**(1) 충돌 상황 만들기**

서로 다른 두 브랜치가 **같은 줄**을 다르게 고치면 충돌이 납니다.

```bash
git switch practice
git switch -c conflict-a
```

`glossary.md` 의 `- 스테이징이란: (여기를 고칠 예정)` 줄을 이렇게 바꾸세요.

```
- 스테이징이란: 커밋할 파일을 미리 골라두는 공간
```

```bash
git commit -am "A: 스테이징 설명 추가"

git switch practice
git switch -c conflict-b
```

이번엔 **같은 줄**을 다르게 바꿉니다.

```
- 스테이징이란: commit 하기 전 대기실
```

```bash
git commit -am "B: 스테이징 설명 추가"
```

**(2) 충돌 일으키기**

```bash
git switch conflict-a
git merge conflict-b        # CONFLICT 메시지가 뜨면 성공
git status                  # both modified 로 표시됨
```

**(3) 해결하기**

`glossary.md` 를 열면 이런 표시가 들어가 있습니다.

```
<<<<<<< HEAD
- 스테이징이란: 커밋할 파일을 미리 골라두는 공간
=======
- 스테이징이란: commit 하기 전 대기실
>>>>>>> conflict-b
```

- `<<<<<<< HEAD` ~ `=======` : **현재 브랜치(A)** 의 내용
- `=======` ~ `>>>>>>>` : **합치려는 브랜치(B)** 의 내용

원하는 내용만 남기고 **`<<<<<<<`, `=======`, `>>>>>>>` 세 줄을 모두 지우세요.** 양쪽을 합쳐서 새로 써도 됩니다.

```bash
git add practice/glossary.md
git commit                   # 병합 커밋 메시지가 자동으로 채워짐
git log --oneline --graph    # 갈라졌다 합쳐진 모양 확인
```

**정리**

```bash
git switch practice
git branch -D conflict-a conflict-b
```

**확인 포인트**

- 충돌은 **오류가 아닙니다.** git이 판단할 수 없으니 사람이 정하라는 요청입니다
- 해결 중 포기하려면 `git merge --abort` 로 병합 전 상태로 돌아갑니다

<br>

## 과제 7 — 작업 중 브랜치 옮기기 (stash)

**목표** : 커밋하기 애매한 작업을 잠시 치워둡니다.

```bash
# learning-log.md 를 수정만 하고 커밋하지 않은 상태에서
git switch -c other-work     # 에러가 나거나 변경분이 따라옵니다

git stash                    # 작업을 임시 보관
git status                   # 깨끗해짐
git stash list               # 보관 목록 확인

git switch -c other-work     # 이제 자유롭게 이동
git switch practice

git stash pop                # 보관한 작업 복원
git status                   # 수정 내용이 돌아옴
```

**확인 포인트**

- `pop` 은 꺼내면서 목록에서 지우고, `apply` 는 목록에 남겨둡니다
- 추적되지 않는 새 파일까지 치우려면 `git stash -u`

<br>

---

## 막혔을 때

| 상황 | 탈출 방법 |
| --- | --- |
| 병합 중인데 그만두고 싶다 | `git merge --abort` |
| 뭘 했는지 모르겠다 | `git reflog` — 모든 이동 기록이 남아 있습니다 |
| `reset --hard` 로 날렸다 | `git reflog` 로 해시 찾아 `git reset --hard <해시>` |
| 편집기가 열렸는데 못 빠져나가겠다 | `Esc` → `:wq` → `Enter` (vim 기준) |
| 완전히 꼬였다 | `git switch practice` 후 이상한 브랜치를 `git branch -D` |

<br>

## 연습이 끝나면

```bash
git switch main       # 치트시트가 있는 브랜치로 복귀
git branch            # practice 브랜치는 그대로 남겨두면 됩니다
```

명령어가 기억나지 않으면 `main` 브랜치의 [README.md](../README.md) 치트시트를 참고하세요.
