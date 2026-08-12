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

sequenceDiagram
    autonumber
    actor A as 작업자 A (GitHub 웹)
    participant G as GitHub 원격 저장소 (origin/main)
    actor B as 작업자 B (로컬 컴퓨터)

    Note over A,B: 두 작업자 모두 동기화된 상태에서 시작

    rect rgb(240, 248, 255)
        Note over A,G: 1. GitHub 웹에서 먼저 수정
        A->>G: README.md 수정 후 커밋 & 반영
    end

    rect rgb(255, 240, 245)
        Note over B,G: 2. 로컬에서 동시 수정 후 Push 시도
        B->>B: README.md 같은 위치를 다르게 수정
        B->>B: git add & git commit
        B->>G: git push 시도
        G-->>B: ❌ Push 거절 (fetch first)
    end

    rect rgb(245, 255, 250)
        Note over B,G: 3. 원격 변경사항 가져오기 및 충돌 해결
        B->>G: git fetch origin (원격 이력 가져오기)
        B->>B: git merge origin/main
        Note over B: ⚠️ README.md 충돌 발생!
        B->>B: 충돌 내용 직접 수정 (Both Changes)
        B->>B: git add README.md
        B->>B: git commit -m "README 충돌 해결"
        B->>G: git push
        G-->>B: ✅ Push 성공!
    end

    rect rgb(240, 248, 255)
        Note over A,G: 4. 작업자 A도 최신 상태로 동기화
        A->>G: git fetch & git merge
    end
