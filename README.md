# MCP Services

MCP (Model Context Protocol) 연습 및 실습을 위한 프로젝트입니다.

다양한 외부 API를 MCP 서버로 래핑하여 AI 어시스턴트에서 활용할 수 있도록 구성한 서비스 모음입니다.

## 현재 구현된 서비스

### 🏛️ Tourism Service
- **한국 관광공사 API** 연동
- 관광지, 숙박시설, 음식점, 축제/행사 등 관광 정보 조회
- MCP 도구로 제공되어 AI 어시스턴트에서 바로 활용 가능

### 🌤️ Weather Service  
- **기상청 API** 연동
- 현재 날씨, 단기 예보, 초단기 예보 조회
- 위경도 좌표 기반 날씨 정보 제공

## 프로젝트 구조

```
mcp-services/
├── .env                    # 환경 변수 (실제 API 키 포함)
├── env.example            # 환경 변수 예시 파일
├── docker-compose.yml     # 통합 Docker Compose 설정
├── services/
│   ├── tourism/          # Tourism MCP 서비스
│   │   ├── Dockerfile
│   │   ├── src/mcp_tourism/
│   │   ├── tests/
│   │   └── pyproject.toml
│   └── weather/          # Weather MCP 서비스
│       ├── Dockerfile
│       ├── korea_weather.py
│       └── pyproject.toml
└── README.md
```

## MCP란?

**Model Context Protocol (MCP)**는 AI 어시스턴트가 외부 도구와 데이터에 안전하게 연결할 수 있도록 하는 개방형 표준입니다.

- 🔧 **도구 통합**: 외부 API를 AI가 사용할 수 있는 도구로 변환
- 📊 **데이터 접근**: 실시간 정보를 AI에게 제공
- 🔒 **보안**: 안전하고 제어된 방식으로 외부 시스템 연결

## 설치 및 실행

### 1. 환경 설정

```bash
# 환경 변수 파일 복사
cp env.example .env

# .env 파일을 편집하여 실제 API 키를 입력
# KOREA_TOURISM_API_KEY=실제_관광공사_API_키  
# KOREA_WEATHER_API_KEY=실제_기상청_API_키
```

### 2. API 키 발급

- **한국 관광공사 API**: https://www.data.go.kr/ 에서 "한국관광공사_국문 관광정보 서비스_GW" 검색 후 활용신청
- **기상청 API**: https://www.data.go.kr/ 에서 "기상청_단기예보" 검색 후 활용신청

### 3. 서비스 실행

#### 개별 서비스 실행 (stdio 모드)
```bash
# Tourism 서비스만 실행
docker-compose --profile tourism-stdio up

# Weather 서비스만 실행  
docker-compose --profile weather-stdio up
```

#### HTTP 모드로 실행
```bash
# Tourism 서비스 (포트 5555)
docker-compose --profile tourism-http up

# Weather 서비스 (포트 5556)
docker-compose --profile weather-http up

# 모든 HTTP 서비스
docker-compose --profile http up
```

#### 개발 모드
```bash
# 모든 서비스를 개발 모드로 실행
docker-compose --profile dev up
```

## 사용 방법

### Tourism Service Tools

- `search_tourist_spots()`: 관광지 검색
- `search_accommodations()`: 숙박시설 검색  
- `search_restaurants()`: 음식점 검색
- `search_festivals()`: 축제/행사 검색
- `get_area_codes()`: 지역 코드 조회

### Weather Service Tools

- `get_nowcast_observation(lon, lat)`: 현재 날씨 조회
- `get_nowcast_forecast(lon, lat)`: 초단기 예보 (6시간)
- `get_short_term_forecast(lon, lat)`: 단기 예보 (3일)

## 환경 변수

| 변수명 | 설명 | 기본값 |
|--------|------|--------|
| `KOREA_TOURISM_API_KEY` | 관광공사 API 키 | 필수 |
| `KOREA_WEATHER_API_KEY` | 기상청 API 키 | 필수 |
| `MCP_TOURISM_DEFAULT_LANGUAGE` | 기본 언어 | ko |
| `MCP_TOURISM_CACHE_TTL` | 캐시 TTL (초) | 3600 |
| `MCP_LOG_LEVEL` | 로그 레벨 | INFO |

## 개발

### 로컬 개발 환경

```bash
# Tourism 서비스 개발
cd services/tourism
python -m venv venv
source venv/bin/activate
pip install -e .

# Weather 서비스 개발  
cd services/weather
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 새로운 서비스 추가

1. `services/` 디렉토리에 새 서비스 폴더 생성
2. Dockerfile과 소스코드 추가
3. `docker-compose.yml`에 서비스 정의 추가

## 라이센스

MIT License

## 기여

Pull Request와 Issue는 언제나 환영합니다!
