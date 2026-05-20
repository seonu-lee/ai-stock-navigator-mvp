# ai-stock-navigator-mvp 프로젝트 파일구조

## 개요

본 구조는 MVP 기획안의 핵심 파이프라인을 기반으로 설계되었다.

```
데이터 수집 → 분석 모델링 → AI 설명 레이어 → UI(Streamlit)
```

각 단계가 독립적인 폴더로 분리되어 있어, 단계별 개발 및 디버깅이 용이하다.

---

## 전체 폴더 구조

```
ai-stock-navigator-mvp/
│
├── data/                        # 수집 및 처리된 데이터 저장소
│   ├── raw/                     # 수집 원본 데이터 (가공 전)
│   └── processed/               # 전처리 완료 데이터
│
├── database/                    # SQLite DB 파일 저장
│   └── stock_data.db
│
├── src/                         # 핵심 소스 코드
│   ├── collection/              # 1단계: 데이터 수집
│   │   ├── fetch_ohlcv.py       # pykrx 주가 OHLCV 수집
│   │   ├── fetch_foreign.py     # 외국인·기관 순매수 수집
│   │   └── news_crawler.py      # 네이버 금융 뉴스 크롤링
│   │
│   ├── analysis/                # 2단계: 분석 모델링
│   │   ├── technical.py         # 이동평균, RSI, 볼린저밴드 산출
│   │   ├── volume_anomaly.py    # 거래량 이상치 탐지 (Z-score)
│   │   ├── sentiment.py         # 뉴스 감성 분석 (KR-FinBERT)
│   │   └── sector_network.py    # 업종별 상관관계 네트워크
│   │
│   ├── ai_layer/                # 3단계: AI 설명 레이어
│   │   ├── prompt_builder.py    # 분석 결과 → 프롬프트 변환
│   │   └── api_caller.py        # Gemini/Claude API 호출
│   │
│   └── database/                # DB 연결 및 적재
│       ├── db_init.py           # SQLite 테이블 초기화
│       └── db_loader.py         # 분석 결과 DB 적재
│
├── app/                         # 4단계: Streamlit UI
│   ├── main.py                  # 앱 진입점
│   ├── pages/                   # 멀티페이지 구성
│   │   ├── 01_market_briefing.py    # 오늘의 시장 브리핑
│   │   ├── 02_stock_analysis.py     # 종목 분석 + 차트 해석
│   │   └── 03_qna.py               # AI Q&A
│   └── components/              # 재사용 UI 컴포넌트
│       ├── chart.py             # plotly 차트 컴포넌트
│       └── disclaimer.py        # 면책 고지 컴포넌트
│
├── scheduler/                   # 배치 스케줄링
│   └── daily_batch.py           # 하루 1회 전체 파이프라인 실행
│
├── notebooks/                   # 분석 탐색용 Jupyter 노트북
│   ├── 01_data_collection_eda.ipynb
│   ├── 02_technical_analysis.ipynb
│   ├── 03_sentiment_analysis.ipynb
│   └── 04_ai_layer_test.ipynb
│
├── tests/                       # 단위 테스트
│   └── test_technical.py
│
├── docs/                        # 문서
│   └── blog/                    # 기술블로그 초안 마크다운 보관
│
├── .env                         # API 키 등 환경변수 (Git 제외)
├── .gitignore
├── requirements.txt             # 패키지 목록
└── README.md
```

---

## 각 폴더 역할 요약

| 폴더 | 역할 | MVP 기획안 단계 |
|------|------|----------------|
| `src/collection/` | pykrx, 크롤링으로 원본 데이터 수집 | 4.1 데이터 수집 |
| `src/analysis/` | 기술적 지표, 이상치 탐지, 감성 분석 | 4.2 분석 모델링 |
| `src/ai_layer/` | 분석값을 프롬프트로 변환 후 API 호출 | 4.3 AI 설명 레이어 |
| `app/` | Streamlit 기반 대시보드 UI | 배포 |
| `scheduler/` | 하루 1회 배치 자동화 | 배치 처리 |
| `notebooks/` | 단계별 탐색 및 실험 | 개발 보조 |
| `database/` | SQLite DB 파일 저장 | 데이터 적재 |
| `docs/blog/` | 기술블로그 초안 보관 | 기록 |

---

## 개발 시작 전 세팅 순서

```
1. GitHub에서 ai-stock-navigator-mvp 레포 생성
2. 로컬에 클론
3. venv 가상환경 생성 및 활성화
4. requirements.txt 기반 패키지 설치
5. .env 파일 생성 후 API 키 입력
6. notebooks/ 에서 단계별 탐색 시작
```
