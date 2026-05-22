# 에이전트 운영 규칙

## 로컬 백엔드 (FastAPI / 포트 8000)

백엔드는 **사용자가 `backend` 폴더에서 직접 명령을 실행할 때만** 띄운다.

| 하지 말 것 | 대신 |
|------------|------|
| `python run.py` / `uvicorn`을 에이전트가 임의 실행 | 사용자가 요청했을 때만 실행, 또는 실행 방법 안내 |
| 검증을 위해 백그라운드 Shell로 서버 기동 | 사용자에게 터미널에서 실행 요청 |
| 포트 8000이 비어 있으면 자동으로 서버 시작 | 필요 시 `Get-NetTCPConnection` 등으로 **조회만** |

**사용자 실행 예 (backend 폴더):**

```bash
conda activate venv
python run.py
```

Swagger: `http://localhost:8000/docs`
