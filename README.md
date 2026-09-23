# Git 명령어 치트시트

실제 작업하는 순서대로 정리한 Git 명령어 모음입니다.

<br>

## 핵심 개념

Git은 파일을 세 개의 공간으로 나눠 관리합니다. 이 구조를 알면 `add`와 `commit`이 왜 따로 있는지 이해됩니다.

```
[작업 디렉토리]  --git add-->  [스테이징]  --git commit-->  [로컬 저장소]  --git push-->  [원격(GitHub)]
   내가 고친 파일               커밋할 것만 골라둠           내 PC의 기록            서버의 기록
```

- **`.git` 폴더** — 저장소의 모든 기록이 담긴 곳. `git init` 으로 생성되며, 이 폴더가 있는 곳(또는 그 하위)에서만 git 명령어가 동작합니다.

<br>

---

## 0. 처음 한 번만 — 설정

| 명령어 | 설명 |
| --- | --- |
| `git config --global user.name "이름"` | 커밋에 기록될 작성자 이름 |
| `git config --global user.email "메일"` | 커밋에 기록될 이메일 (GitHub 잔디 기준) |
| `git config --global init.defaultBranch main` | 기본 브랜치 이름을 main으로 |
| `git config --list` | 현재 설정 전체 보기 |
| `git config user.email "메일"` | `--global` 없이 쓰면 **해당 저장소에만** 적용 |

> 계정이 여러 개면 저장소별 설정으로 덮어쓸 수 있습니다. 전역 설정이 잘못돼 있으면 엉뚱한 계정으로 커밋됩니다.

<br>

## 1. 저장소 시작하기

| 명령어 | 설명 |
| --- | --- |
| `git init` | 현재 폴더를 저장소로 만듦 (`.git` 생성) |
| `git init -b main` | main 브랜치로 시작 |
| `git clone <URL>` | 원격 저장소를 통째로 복제 |
| `git clone <URL> <폴더명>` | 다른 이름의 폴더로 복제 |

<br>

## 2. 상태 확인

| 명령어 | 설명 |
| --- | --- |
| `git status` | 변경된 파일, 스테이징 상태 확인 |
| `git status -s` | 짧게 요약해서 보기 |
| `git diff` | 아직 `add` 안 한 변경 내용 |
| `git diff --staged` | `add`는 했고 아직 커밋 안 한 변경 내용 |
| `git log --oneline` | 커밋 기록 한 줄씩 |
| `git log --oneline --graph --all` | 브랜치 흐름을 그래프로 |
| `git log -p <파일>` | 특정 파일의 변경 이력 |
| `git show <커밋>` | 특정 커밋의 상세 내용 |

<br>

## 3. 스테이징 (커밋할 파일 고르기)

| 명령어 | 설명 |
| --- | --- |
| `git add <파일>` | 특정 파일을 스테이징 |
| `git add .` | 현재 폴더 이하 변경분 전부 |
| `git add -A` | 삭제된 파일까지 포함해 전부 |
| `git add -p` | 변경 내용을 조각별로 골라서 추가 |
| `git rm <파일>` | 파일 삭제 + 스테이징 |
| `git rm --cached <파일>` | 파일은 두고 **git 추적만 해제** |
| `git mv <원본> <대상>` | 파일 이름 변경 / 이동 |

**`.gitignore`** — 추적하지 않을 파일을 적어두는 파일입니다.

```
.DS_Store       # macOS 시스템 파일
*.pem           # 인증서·키 (절대 커밋 금지)
.env            # 환경변수·비밀번호
node_modules/   # 용량 큰 의존성 폴더
```

<br>

## 4. 커밋

| 명령어 | 설명 |
| --- | --- |
| `git commit -m "메시지"` | 스테이징된 것을 커밋 |
| `git commit -am "메시지"` | 추적 중인 파일은 `add` 생략하고 커밋 |
| `git commit --amend` | **직전 커밋**을 수정 (메시지·내용) |
| `git commit --amend --no-edit` | 메시지는 그대로, 파일만 추가로 합침 |

> `--amend`는 커밋을 새로 만드는 것이라, **이미 푸시한 커밋에 쓰면 충돌**합니다. 푸시 전에만 쓰세요.

<br>

## 5. 브랜치

| 명령어 | 설명 |
| --- | --- |
| `git branch` | 브랜치 목록 (현재 위치는 `*`) |
| `git branch -a` | 원격 브랜치까지 전부 |
| `git switch <브랜치>` | 브랜치 이동 |
| `git switch -c <브랜치>` | 새로 만들면서 이동 |
| `git checkout <브랜치>` | 이동 (구버전 방식, `switch`와 동일) |
| `git merge <브랜치>` | 현재 브랜치에 대상 브랜치를 합침 |
| `git branch -d <브랜치>` | 병합 끝난 브랜치 삭제 |
| `git branch -D <브랜치>` | 병합 안 됐어도 강제 삭제 |
| `git branch -m <새이름>` | 현재 브랜치 이름 변경 |

<br>

## 6. 원격 저장소 (GitHub)

| 명령어 | 설명 |
| --- | --- |
| `git remote -v` | 연결된 원격 주소 확인 |
| `git remote add origin <URL>` | 원격 저장소 연결 |
| `git remote set-url origin <URL>` | 원격 주소 변경 |
| `git push -u origin main` | 첫 푸시 (`-u`로 기본 연결 지정) |
| `git push` | 이후부터는 이것만 |
| `git pull` | 원격 변경분을 받아서 **합치기** |
| `git fetch` | 받아오기만 하고 합치지는 않음 |
| `git push --force-with-lease` | 강제 푸시 (남의 작업이 있으면 중단) |

> `git push -f`(무조건 강제)는 동료 작업을 지웁니다. 꼭 필요하면 `--force-with-lease`를 쓰세요.

<br>

## 7. 되돌리기

**어디까지 되돌릴지**에 따라 명령어가 다릅니다.

| 명령어 | 되돌리는 대상 | 설명 |
| --- | --- | --- |
| `git restore <파일>` | 작업 디렉토리 | 수정한 내용을 마지막 커밋 상태로 |
| `git restore --staged <파일>` | 스테이징 | `add`만 취소 (수정 내용은 유지) |
| `git reset --soft HEAD~1` | 커밋 | 커밋만 취소, 변경 내용은 스테이징에 남김 |
| `git reset --mixed HEAD~1` | 커밋 + 스테이징 | 커밋·add 취소, 파일 수정은 유지 (기본값) |
| `git reset --hard HEAD~1` | 전부 | **변경 내용까지 완전 삭제** — 복구 어려움 |
| `git revert <커밋>` | 커밋 (안전) | 취소하는 **새 커밋**을 만듦. 이미 푸시했다면 이걸 사용 |
| `git reflog` | — | 모든 이동 기록. `reset --hard` 후 복구할 때 생명줄 |

**작업을 잠시 치워두기 (stash)**

| 명령어 | 설명 |
| --- | --- |
| `git stash` | 현재 작업을 임시 보관하고 깨끗한 상태로 |
| `git stash -u` | 추적 안 하는 새 파일까지 포함 |
| `git stash list` | 보관 목록 |
| `git stash pop` | 최근 보관분 꺼내고 목록에서 제거 |
| `git stash apply` | 꺼내되 목록에는 남김 |
| `git stash drop` | 보관분 삭제 |

<br>

---

## 이럴 땐 이렇게

| 상황 | 해결 |
| --- | --- |
| 커밋 메시지를 잘못 썼다 (푸시 전) | `git commit --amend -m "새 메시지"` |
| 파일 하나를 커밋에 빠뜨렸다 (푸시 전) | `git add <파일>` → `git commit --amend --no-edit` |
| `add`를 잘못했다 | `git restore --staged <파일>` |
| 수정한 걸 전부 버리고 싶다 | `git restore <파일>` |
| 방금 커밋을 취소하고 싶다 (푸시 전) | `git reset --soft HEAD~1` |
| 이미 푸시한 커밋을 취소하고 싶다 | `git revert <커밋해시>` |
| `reset --hard`로 날려버렸다 | `git reflog`로 해시 찾아 `git reset --hard <해시>` |
| 비밀번호·키를 커밋했다 | 즉시 **해당 키를 폐기**하고 재발급. 기록에서 지워도 유출된 것으로 간주 |
| 다른 브랜치로 옮겨야 하는데 작업 중이다 | `git stash` → 브랜치 이동 → `git stash pop` |
| `not a git repository` 오류 | `.git`이 있는 폴더로 `cd` 이동 후 실행 |
| 푸시했는데 잔디가 안 쌓인다 | `git config user.email`이 GitHub 계정 이메일과 일치하는지 확인 |

<br>

## 자주 쓰는 흐름

```bash
# 매일 작업 시작할 때
git pull

# 작업 후 올리기
git status                    # 뭐가 바뀌었나 확인
git add -A                    # 전부 스테이징
git commit -m "작업 내용"      # 커밋
git push                      # 업로드

# 새 기능 작업할 때
git switch -c feature/login   # 브랜치 만들고 이동
# ... 작업 ...
git add -A && git commit -m "로그인 구현"
git push -u origin feature/login
```

<br>

---

## GitHub CLI (gh) 참고

| 명령어 | 설명 |
| --- | --- |
| `gh auth login` | GitHub 계정 로그인 |
| `gh auth status` | 현재 로그인된 계정 확인 |
| `gh auth switch` | 등록된 계정 간 전환 |
| `gh repo create <이름> --public --source=. --push` | 리포 생성 + 연결 + 푸시 한 번에 |
| `gh repo list <계정>` | 리포 목록 |
| `gh repo delete <계정>/<리포>` | 리포 삭제 (`delete_repo` 권한 필요) |
