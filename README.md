# 2026-git-start
2026-git-start


로컬 컴퓨터에서 추가한 내용입니다.
GitHub 웹에서 추가한 내용입니다.



작업자 A·B의 Fetch, Merge 및 충돌 해결 — 시퀀스 다이어그램
작업자 B
GitHub origin main
작업자 A
작업자 B
GitHub origin main
작업자 A
0. 사전 준비 - 두 로컬 저장소 상태 동기화 및 커밋 작성자 설정
1차 실습 - 충돌 없는 협업
2차 실습 - 같은 파일 수정으로 충돌 발생
README.md 같은 문장 수정
README.md 같은 문장을 다르게 수정 (fetch 전)
CONFLICT content README.md
최종적으로 작업자 A와 작업자 B와 GitHub가 동일한 최신 커밋 상태로 동기화됨
worker-a.md 생성
git add commit push
git fetch origin
origin main 갱신 정보 전달
git merge origin main (worker-a.md 반영)
worker-b.md 생성
git add commit push
git fetch origin
origin main 갱신 정보 전달
git merge origin main (worker-b.md 반영)
README.md에 공통 문장 추가
git add commit push
git fetch origin
git merge origin main (공통 문장 반영)
git add commit push (선반영 성공)
git add commit
git push
rejected fetch first (브랜치 diverged)
git fetch origin
origin main 최신 커밋 전달
git merge origin main
충돌 표시 확인 후 해결
git add README.md
git commit (Merge Commit 생성)
git push
origin main에 충돌 해결 결과 반영
git fetch origin
작업자 B의 Merge 커밋 전달
git merge origin main (최종 결과 반영)
참고: 핵심 흐름 요약
다른 작업자의 변경은 git fetch 후 git merge를 실행해야만 로컬에 반영된다.
같은 파일 같은 줄을 서로 다르게 수정하면 git merge 시 충돌(CONFLICT)이 발생한다.
충돌은 먼저 push가 거절된 쪽(merge를 수행하는 작업자)의 로컬 저장소에서 해결된다.
충돌 해결 순서: 충돌 표시 삭제 → git add → git commit(Merge Commit) → git push.
