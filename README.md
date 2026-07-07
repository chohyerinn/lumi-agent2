# Day 2 Mission — Docker 컨테이너화 및 GCE VM 배포

## 환경 설정

```bash
# 의존성 설치
uv sync

# 환경변수 설정
cp .env.example .env
```

## 실행

```bash
# Docker 이미지 빌드
docker build -t lumi-agent .

# Docker Compose로 실행
docker-compose up --build

# 백그라운드 실행
docker-compose up -d

# 로그 확인
docker-compose logs -f

# 종료
docker-compose down
```

## 미션

### 1. `app/api/routes/health.py` - 헬스체크 엔드포인트

| TODO | 내용 |
|------|------|
| v TODO 1 | APIRouter 인스턴스 생성 |
| v TODO 2 | 헬스체크 엔드포인트 구현 |

### 2. `Dockerfile` - 컨테이너 이미지 구성

| TODO | 내용 |
|------|------|
| v TODO 1 | 의존성 설치 명령어 작성 |
| v TODO 2 | 보안 설정 - non-root 유저 생성 및 권한 설정 |
| v TODO 3 | 헬스체크 설정 |
| v TODO 4 | 서버 실행 명령어 작성 (uv run) |

### 3. `docker-compose.yml` - 로컬 실행 환경

| TODO | 내용 |
|------|------|
| v TODO 1 | 이미지 및 빌드 설정 |
| v TODO 2 | 포트 매핑 (호스트:컨테이너) |
| v TODO 3 | 환경변수 설정 |
| v TODO 4 | 헬스체크 및 재시작 정책 설정 |

### 4. `.github/workflows/cd.yml` - CD 파이프라인

| TODO | 내용 |
|------|------|
| v TODO 1 | 트리거 설정 |
| v TODO 2 | GHCR Push 권한 설정 |
| v TODO 3 | GHCR 로그인 |
| v TODO 4 | 이미지 빌드 & Push |

### 7.1 심화과제 - 멀티 환경 배포 (staging, production 분리)

| TODO | 내용 |
|------|------|
| v TODO 1 | `docker-compose.staging.yml` 작성 |
| v TODO 2 | `docker-compose.production.yml` 작성 |
| v TODO 3 | staging/production 환경변수 분리 |
| v TODO 4 | 환경별 포트와 컨테이너명 분리 |
| v TODO 5 | 환경별 Compose 설정 검증 |

#### 구현 요약

- `docker-compose.staging.yml`: staging 검증용 compose 파일입니다. `ENVIRONMENT=staging`으로 실행하고, 기본 호스트 포트는 `8001`입니다.
- `docker-compose.production.yml`: 운영 배포용 compose 파일입니다. `ENVIRONMENT=production`, `DEBUG=false`로 실행하고, 기본 이미지는 GHCR의 `ghcr.io/ai-pre/lumi-agent2/lumi-agent:latest`입니다.
- 두 파일 모두 헬스체크와 `restart: unless-stopped`를 포함해 환경별로 독립 실행할 수 있게 구성했습니다.

```bash
# staging 실행
docker-compose -f docker-compose.staging.yml up --build

# production 실행
docker-compose -f docker-compose.production.yml up -d
```
