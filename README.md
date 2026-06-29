
# KYSS 통합매출 도구 — 클라우드 배포 가이드

PC를 꺼도 24시간 돌아가게, 클라우드(Render 등)에 올리는 방법입니다.

---

## 이 꾸러미에 들어있는 것

| 파일 | 역할 |
|------|------|
| `server.py` | 수정 완료된 서버 (포트·로그인정보를 환경변수로 읽도록 변경됨) |
| `Dockerfile` | 클라우드가 Chromium까지 자동 설치해서 띄우는 설명서 |
| `requirements.txt` | 필요한 파이썬 패키지 목록 |
| `.env.example` | 등록할 환경변수 예시 |

> **중요:** 이 4개 파일을 **기존 CFS 폴더(모든 .html 파일이 있는 곳)에 같이 넣어야** 합니다.
> HTML 도구들은 손댈 필요 없어요. 클라우드 주소로 알아서 작동하게 이미 되어 있습니다.

---

## 가장 빠른 길 (추천)

운송팀에서 이미 `kyss-dashboard.onrender.com`을 Render에 올려본 분이 있습니다.
그분께 **이 폴더(server.py + Dockerfile + requirements.txt + 모든 html)** 를 통째로 건네면,
이미 같은 문제(KYSS 로그인 + Render)를 해결해본 사람이라 금방 올려줄 수 있어요.
그분께 아래 "환경변수 2개"만 알려주면 됩니다:

- `KYSS_ID` = KYSS 아이디
- `KYSS_PW` = KYSS 비밀번호

---

## 직접 하기 (Render 기준)

### 1단계 — 폴더 준비
`server.py`, `Dockerfile`, `requirements.txt` 를 기존 CFS 폴더(index.html 등 모든 html이 있는 곳)에 복사해 넣습니다.

### 2단계 — GitHub에 올리기 (반드시 **비공개/Private**)
- github.com 가입 → New repository → **Private 선택** (공개로 하면 안 됩니다)
- 그 폴더를 업로드 (GitHub Desktop 앱을 쓰면 드래그로 가능)

> 비공개로 하는 이유: 혹시 코드에 정보가 남아도 외부에 노출되지 않게 하기 위함입니다.

### 3단계 — Render에서 배포
1. render.com 가입 → **New +** → **Web Service**
2. 방금 만든 GitHub 저장소 연결
3. **Language(또는 Runtime)** 를 **Docker** 로 선택 (Dockerfile을 자동 인식합니다)
4. **Environment** 메뉴에서 환경변수 2개 등록:
   - `KYSS_ID` → KYSS 아이디
   - `KYSS_PW` → KYSS 비밀번호
5. **Create Web Service** 클릭 → 빌드 시작 (Chromium 설치 때문에 첫 빌드는 몇 분 걸립니다)

### 4단계 — 주소 받기
배포가 끝나면 `https://이름.onrender.com` 주소가 나옵니다.
이 주소를 **PC 브라우저·모바일 어디서든** 열면 됩니다. 이제 PC를 꺼도 돼요.

---

## 알아둘 점

- **무료 등급 절전:** Render 무료는 15분간 아무도 안 쓰면 잠듭니다. 다음 접속 때 첫 화면이 30초~1분 느릴 수 있어요(이후엔 정상). 항상 빠르게 하려면 유료(월 $7 정도)로 올리거나, 5분마다 한 번씩 주소를 호출해주는 무료 핑(ping) 서비스를 거는 방법이 있습니다.
- **메모리:** 자동 로그인(Chromium)이 메모리를 꽤 먹습니다. 무료 등급에서 가끔 로그인이 실패하면, 통합매출 도구의 **관리자 세션 설정**에 JSESSIONID를 직접 붙여넣는 방법으로 우회할 수 있어요.
- **계정 변경:** 나중에 다른 KYSS 계정으로 바꾸려면, Render의 Environment에서 `KYSS_ID` / `KYSS_PW` 값만 바꾸고 재배포하면 됩니다. 코드는 안 건드려도 됩니다.
- **PC와 동시 사용:** 클라우드와 PC가 같은 KYSS 계정으로 동시에 로그인하면 서로 로그아웃될 수 있어요. 그럴 땐 한쪽만 쓰거나, 한쪽을 다른 계정으로 설정하세요.

---

## 다른 클라우드도 가능

이 Dockerfile은 표준 방식이라 Railway, Fly.io, Google Cloud Run 등에서도 거의 같은 방식으로 올라갑니다.
(공통: 저장소 연결 → Docker 인식 → 환경변수 KYSS_ID/KYSS_PW 등록 → 배포)
