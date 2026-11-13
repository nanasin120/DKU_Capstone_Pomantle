# 🎮 포맨틀 (Pomantle)

> **"매일 가볍게 즐기는 모션 인식 포즈 게임!"**
>
> 단어의 의미적 유사도를 추론하는 '꼬맨틀'에서 착안하여, 텍스트가 아닌 **'신체 포즈'를 추론**하는 웹 기반 인터랙티브 미니게임입니다.  
> 별도의 설치나 장비 없이 웹캠만으로 즉시 이용 가능하며, 바쁜 현대인들에게 '오늘의 스트레칭'처럼 기분 환기와 신체 활동의 시간을 제공합니다.

<br>

## 🔗 바로가기 & 최종 산출물

| 구분 | 링크 |
| :--- | :--- |
| 🎥 **시연 영상** | [YouTube 시연 영상 보기](유튜브_URL_입력) |
| 📄 **최종 결과보고서** | [결과보고서 PDF](./docs/Pomantle_결과보고서.pdf) |
| 🖼️ **전시회 포스터** | [포스터 PDF](./docs/Pomantle_포스터.pdf) |

<br>

## 📱 주요 화면 (Screenshots)

| 1. 메인 & 튜토리얼 | 2. 실시간 포즈 인식 & 촬영 | 3. 유사도 판정 결과 | 4. 오늘의 기록 (Top 5) |
| :---: | :---: | :---: | :---: |
| <img src="./docs/images/main.png" width="200"/> | <img src="./docs/images/game.png" width="200"/> | <img src="./docs/images/result.png" width="200"/> | <img src="./docs/images/record.png" width="200"/> |
| 튜토리얼 및 게임 시작 | MediaPipe 관절 추출 및 오버레이 | 부위별 유사도 분석 팝업 | LocalStorage 기반 Top 5 조회 |

<br>

## 🕹️ How To Play

1. **포즈 상상 및 진입**: 매일 새롭게 주어지는 '오늘의 정답 포즈'를 유추하며 메인 화면의 'GO' 버튼을 클릭합니다.
2. **타이머 촬영**: 우측 상단에서 3초 또는 5초 타이머를 선택한 뒤, 화면의 스켈레톤 가이드에 맞춰 온몸을 움직여 스냅샷을 캡처합니다.
3. **결과 및 기록 확인**: 정답 포즈와의 싱크로율(유사도 점수) 팝업을 즉시 확인하며, 90점 이상 시 클리어됩니다. 기록 탭에서 당일 시도한 최고 점수 상위 5개를 조회할 수 있습니다.

<br>

## 🧠 Core System & Algorithm (핵심 기술)

### 1. 실시간 3D 관절 추출 (MediaPipe)
클라이언트 브라우저 환경에서 **Google MediaPipe 프레임워크**를 활용하여 어깨, 팔꿈치, 손가락 마디의 3차원 관절 좌표(Landmark)를 실시간으로 추출하고 화면에 오버레이합니다.

### 2. 가중치 기반 포즈 유사도 연산 (Flask API)
추출된 신체 좌표는 **Flask 기반의 경량 API 서버**로 전송됩니다. 
- 단순히 좌표의 거리만 재는 것이 아니라, 정답 포즈와의 **사잇각 및 3차원 위치 관계를 분석**하는 독자적인 유사도 연산 알고리즘을 수행합니다. 
- 특히 제스처 판별에 있어 변별력이 높은 **손 영역에 독립적인 가중치**를 부여하여, 100점 만점 기준의 정밀한 유사도 점수를 산출합니다.

### 3. JSONB 최적화 및 무결성 보장 (PostgreSQL)
가변적이고 복잡한 다중 관절 좌표 데이터를 효율적으로 처리하기 위해 **PostgreSQL의 JSONB 타입**을 적용하여 최적화된 저장을 구현했습니다. 이를 통해 일별 정답 포즈 배정과 사용자 기록의 무결성을 보장합니다.

<br>

## 🛠️ Tech Stack (기술 스택)

| 구분 | 사용 기술 |
| :--- | :--- |
| **Frontend (User / Admin)** | React, TypeScript, CSS3 |
| **AI / Vision** | Google MediaPipe Holistic |
| **Backend** | Python, Flask, SQLAlchemy |
| **Database** | PostgreSQL |

<br>

## 📂 Project Structure (프로젝트 구조)

```text
DKU_Capstone_Pomantle/
├── frontend/                       # 사용자용 포맨틀 게임 웹 (React + TS)
│   ├── src/components/             # MediaPipe 카메라, 결과 팝업, 오버레이 UI
│   └── src/managers/               # Game, Camera, Record(로컬스토리지), Time 관리
├── frontend_admin/                 # 관리자용 출제 시스템 웹 (React + TS)
│   └── src/components/             # 일자별 캘린더 관리, 포즈 등록/수정 팝업(AdminCalendar)
├── backend/                        # Python/Flask 백엔드 & 유사도 판정 서버
│   ├── api/                        # User 라우터 (routes.py) & Admin 라우터 분리
│   ├── database/                   # PostgreSQL JSONB 및 SQLAlchemy ORM 모델
│   └── services/                   # 관절 좌표 정규화 및 가중치 유사도 연산 엔진
└── docs/                           # 산출물 및 에셋 (결과보고서, 발표자료, 포스터)

```

*본 프로젝트는 단국대학교 소프트웨어학과 캡스톤 디자인(졸업작품)으로 제작되었습니다.*
