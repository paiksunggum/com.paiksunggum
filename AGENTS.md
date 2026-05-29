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

## Titanic / 헥사고날 — SOLID·추상화 (ports)

백엔드 `apps/titanic` 및 동일 패턴 모듈은 **SOLID**와 **포트·어댑터**를 따른다.

### 레이어

| 경로 | 역할 | 허용 |
|------|------|------|
| `app/ports/input/` | 유스케이스 **계약** | `ABC` + `@abstractmethod`만 |
| `app/ports/output/` | 리포지토리 **계약** | `ABC` + `@abstractmethod`만 |
| `app/use_cases/` | 유스케이스 **구현** | input port 상속, output port 호출 |
| `adapter/inbound/` | HTTP·DI·파싱 | 추상 타입(`*UseCase`)에만 의존 |
| `adapter/outbound/` | DB·메모리 **구현** | output port `ABC` 상속 |

### 규칙

- 포트 파일에는 **구현·로깅·`*Impl` 클래스를 두지 않는다.**
- 로그 prefix는 **구현 클래스명만** (`JamesCommand`, `JamesPgRepository` 등). `*UseCase`, `*Repository` ABC 이름·추상 계층은 로그에 쓰지 않는다.
- 로그는 **매개체( adapter / use_cases )로 데이터가 넘어갈 때만** — ports에는 없음, return 경로 중복 로그 지양.
- 포트 추상 메서드 시그니처에는 **`self`를 넣지 않는다** (구현체 `use_cases`·`adapter`에는 `self` 유지).
- 입력·출력 포트는 **`Protocol`이 아닌 `ABC`** 를 쓴다.
- 라우터는 repository / PG adapter를 **직접 호출하지 않고** use case를 주입한다.
- James(쓰기)·Walter(읽기)처럼 **역할별로 포트를 분리**한다 (Interface Segregation).

### SOLID 대응

- **S**: 라우터 = HTTP, use case = 오케스트레이션, repository adapter = I/O
- **O**: 저장소·DB 변경은 `adapter/outbound`만 수정
- **L**: adapter 구현이 port 계약을 그대로 지킴
- **I**: command / query 포트 분리
- **D**: 상위 레이어는 구체 클래스가 아닌 **추상 port**에 의존
