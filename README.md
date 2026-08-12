# 2026 Git Start - GitHub & 로컬 저장소 Merge 충돌 해결 실습

이 저장소는 **GitHub 웹 저장소**와 **로컬 개발 환경** 간에서 동일한 파일을 수정할 때 발생하는 **Merge 충돌(Merge Conflict)** 상황을 의도적으로 만들고, 이를 `git fetch`, `git merge`를 활용해 해결하는 과정을 기록한 실습 프로젝트입니다.

---

## 📄 최종 README 내용 (병합 결과)

GitHub 웹에서 추가한 내용입니다.
로컬 컴퓨터에서 추가한 내용입니다.

---

## 🎯 실습 목표

- **저장소 원격 복제:** GitHub 저장소를 로컬 컴퓨터로 Clone하는 방법 습득
- **충돌 원인 이해:** 원격 저장소와 로컬 저장소의 커밋 이력이 갈라졌을 때(`diverged`) `git push`가 거절되는 이유 파악
- **단계별 병합 과정:** `git pull` 대신 `git fetch`와 `git merge`를 분리하여 원격 저장소의 변경사항 확인 후 병합
- **충돌 해결 능력:** README.md의 충돌 기호(`<<<<<<<`, `=======`, `>>>>>>>`) 해석 및 해결 후 Push

---

## 🔄 작업 및 충돌 해결 흐름 (Sequence Diagram)

작업자 A(GitHub 웹)와 작업자 B(로컬 개발자)가 동일 파일의 같은 위치를 동시에 수정하여 충돌을 발생시키고 해결하는 과정입니다.

```mermaid
sequenceDiagram
    autonumber
    actor Web as GitHub 웹 (작업자 A)
    participant Remote as GitHub 원격 저장소 (origin/main)
    participant Local as 로컬 컴퓨터 (main)
    actor LocalUser as 로컬 작업자 (작업자 B)

    LocalUser->>Remote: 1. git clone
    Web->>Remote: 2. README.md 수정 & Commit
    LocalUser->>Local: 3. README.md 다르게 수정 & Commit
    LocalUser->>Remote: 4. git push 시도
    Remote-->>LocalUser: ❌ Push 거절 (Non-fast-forward / Diverged)
    
    Note over Local,Remote: 원격 변경사항 가져오기 및 병합
    LocalUser->>Remote: 5. git fetch origin
    LocalUser->>Local: 6. git merge origin/main
    Local-->>LocalUser: ⚠️ Merge Conflict 발생 (README.md)
    
    Note over LocalUser,Local: 충돌 해결 작업
    LocalUser->>Local: 7. 충돌 표시 제거 및 내용 결정 (Both Changes 수용)
    LocalUser->>Local: 8. git add README.md
    LocalUser->>Local: 9. git commit -m "README 충돌 해결"
    LocalUser->>Remote: 10. git push
    Remote-->>LocalUser: ✅ Push 성공!




    
```mermaid
sequenceDiagram
    participant A as 작업자 A
    participant G as GitHub
    participant B as 작업자 B

    Note over A,B: 두 작업자는 같은 최신 상태에서 시작

    A->>A: README.md 수정
    A->>A: git add + git commit
    A->>G: git push

    Note over G: A의 커밋이 origin/main에 반영됨

    B->>B: README.md 같은 문장을 다르게 수정
    B->>B: git add + git commit

    B->>G: git push
    G-->>B: Push 거절 (fetch first)

    Note over B,G: B의 로컬에는 A의 최신 커밋이 없음

    B->>G: git fetch origin
    G-->>B: A의 최신 커밋 정보 전달

    B->>B: git merge origin/main
    Note over B: README.md 충돌 발생

    B->>B: README.md 충돌 직접 해결
    B->>B: git add README.md
    B->>B: git commit (Merge Commit)

    B->>G: git push

    Note over G: 충돌 해결 결과가 origin/main에 반영됨

    A->>G: git fetch origin
    G-->>A: B의 Merge Commit 정보 전달
    A->>A: git merge origin/main

    Note over A,B: A / B / GitHub 모두 최신 상태
```
