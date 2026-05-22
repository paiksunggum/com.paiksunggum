# Git — 모노레포 + 서브모듈

이 저장소(`com.ragwatson`)는 **래퍼**이고, 실제 코드는 **서브모듈**에 있습니다.

| 경로 | 저장소 | 커밋 위치 |
|------|--------|-----------|
| `frontend/` | com.paiksunggum.www | **`cd frontend`** 후 `git add` / `git commit` |
| `backend/` | com.ragwatson.api | **`cd backend`** 후 `git add` / `git commit` |
| `docs/` | com.paiksunggum.docs | **`cd docs`** 후 `git add` / `git commit` |
| 루트 (`.cursorrules`, `AGENTS.md` 등) | com.ragwatson | **저장소 루트**에서 `git commit` |

## `frontend`에서 `nothing to commit`이 나올 때

1. 변경이 **이미 커밋됐는지** 확인: `git log -1 --oneline`
2. 파일이 **저장됐는지** 확인 (에디터에서 Accept/저장)
3. 루트에서 서브모듈 포인터만 안 올라간 경우 → **루트**에서 `git add frontend && git commit`

## 권장 순서

```bash
# 1) 각 서브모듈에서 커밋
cd frontend && git add . && git commit -m "메시지"
cd ../backend && git add . && git commit -m "메시지"

# 2) 루트에서 서브모듈 포인터 + 루트 파일 커밋
cd ..
git add frontend backend .cursorrules AGENTS.md
git commit -m "서브모듈 및 하네스 규칙 갱신"
```
