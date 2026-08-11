# 2026-git-start
2026-git-start


로컬 컴퓨터에서 추가한 내용입니다.
GitHub 웹에서 추가한 내용입니다.

# 작업자 A·B의 Fetch, Merge 및 충돌 해결 — 시퀀스 다이어그램

​
    participant A as 작업자 A
    participant G as GitHub origin main
    participant B as 작업자 B

    Note over A,B: 0. 사전 준비 - 두 로컬 저장소 상태 동기화 및 커밋 작성자 설정

    rect rgb(240, 248, 255)
    Note over A,B: 1차 실습 - 충돌 없는 협업
    A->>A: worker-a.md 생성
    A->>G: git add commit push
    B->>G: git fetch origin
    G-->>B: origin main 갱신 정보 전달
    B->>B: git merge origin main (worker-a.md 반영)
    B->>B: worker-b.md 생성
    B->>G: git add commit push
    A->>G: git fetch origin
    G-->>A: origin main 갱신 정보 전달
    A->>A: git merge origin main (worker-b.md 반영)
    end

    rect rgb(255, 245, 238)
    Note over A,B: 2차 실습 - 같은 파일 수정으로 충돌 발생
    A->>A: README.md에 공통 문장 추가
    A->>G: git add commit push
    B->>G: git fetch origin
    B->>B: git merge origin main (공통 문장 반영)

    Note over A: README.md 같은 문장 수정
    A->>G: git add commit push (선반영 성공)

    Note over B: README.md 같은 문장을 다르게 수정 (fetch 전)
    B->>B: git add commit
    B->>G: git push
    G-->>B: rejected fetch first (브랜치 diverged)

    B->>G: git fetch origin
    G-->>B: origin main 최신 커밋 전달
    B->>B: git merge origin main
    Note over B: CONFLICT content README.md
    B->>B: 충돌 표시 확인 후 해결
    B->>B: git add README.md
    B->>B: git commit (Merge Commit 생성)
    B->>G: git push
    G-->>G: origin main에 충돌 해결 결과 반영

    A->>G: git fetch origin
    G-->>A: 작업자 B의 Merge 커밋 전달
    A->>A: git merge origin main (최종 결과 반영)
    end

    Note over A,B: 최종적으로 작업자 A와 작업자 B와 GitHub가 동일한 최신 커밋 상태로 동기화됨
​```

## 참고: 핵심 흐름 요약
- 다른 작업자의 변경은 git fetch 후 git merge를 실행해야만 로컬에 반영된다.
- 같은 파일 같은 줄을 서로 다르게 수정하면 git merge 시 충돌(CONFLICT)이 발생한다.
- 충돌은 먼저 push가 거절된 쪽(merge를 수행하는 작업자)의 로컬 저장소에서 해결된다.
- 충돌 해결 순서: 충돌 표시 삭제 → git add → git commit(Merge Commit) → git push.
