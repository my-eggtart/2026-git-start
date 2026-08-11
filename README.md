# 2026-git-start
2026-git-start


로컬 컴퓨터에서 추가한 내용입니다.
GitHub 웹에서 추가한 내용입니다.

# 작업자 A·B의 Fetch, Merge 및 충돌 해결 — 시퀀스 다이어그램

​
    
```mermaid
sequenceDiagram
    participant G as GitHub 원격 저장소
    participant A as 작업자 A 로컬
    participant B as 작업자 B 로컬

    Note over G,B: 1. 작업 환경 준비

    G->>A: git clone
    G->>B: git clone

    Note over A: 독립된 로컬 main
    Note over B: 독립된 로컬 main

    Note over G,B: 2. 충돌 없는 협업

    A->>A: worker-a.md 작성
    A->>A: git add
    A->>A: git commit
    A->>G: git push

    Note over B: A의 변경은 아직 없음

    B->>G: git fetch origin
    G-->>B: origin/main 최신 정보 전달
    B->>B: git merge origin/main

    Note over B: worker-a.md 반영

    B->>B: worker-b.md 작성
    B->>B: git add
    B->>B: git commit
    B->>G: git push

    A->>G: git fetch origin
    G-->>A: origin/main 최신 정보 전달
    A->>A: git merge origin/main

    Note over A,B: A와 B 모두 최신 상태

    Note over G,B: 3. 충돌 발생 준비

    A->>A: README.md 같은 문장 수정
    A->>A: git add
    A->>A: git commit
    A->>G: git push

    Note over G: origin/main에 A 변경 반영

    B->>B: 같은 README.md 문장을<br/>A와 다르게 수정
    B->>B: git add
    B->>B: git commit

    B->>G: git push
    G--xB: Push 거절<br/>(fetch first)

    Note over B,G: 원격에는 B가 가지고 있지 않은<br/>A의 커밋이 존재함

    Note over G,B: 4. 원격 변경 확인

    B->>G: git fetch origin
    G-->>B: A의 커밋 정보 가져오기

    Note over B: local main = B 변경<br/>origin/main = A 변경

    B->>B: git merge origin/main

    Note over B: README.md Merge Conflict 발생<br/>main|MERGING

    Note over G,B: 5. 충돌 해결

    B->>B: README.md 충돌 내용 확인

    Note over B: <<<<<<< HEAD<br/>B의 내용<br/>=======<br/>A의 내용<br/>>>>>>>> origin/main

    B->>B: A와 B의 내용을 검토
    B->>B: 최종 내용 직접 작성
    B->>B: 충돌 표시 삭제

    B->>B: git add README.md
    Note over B: 충돌 해결 완료 표시

    B->>B: git commit
    Note over B: Merge Commit 생성

    B->>G: git push
    Note over G: 충돌 해결된 최종 결과 반영

    Note over G,B: 6. 다른 작업자 동기화

    A->>G: git fetch origin
    G-->>A: Merge Commit 정보 가져오기

    A->>A: git merge origin/main

    Note over G,B: GitHub / 작업자 A / 작업자 B<br/>모두 같은 최신 커밋 상태
```
