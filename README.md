# Process Log Analyzer · 사용 가이드

> **설비 메인트 엔지니어 전용 · sLLM 초기 버전**  
> 엔지니어 레벨에 관계없이, 누구나 빠르고 직관적으로 공정 이상을 분석하고 즉각 조치할 수 있도록 설계된 웹 기반 분석 툴입니다.

---

## 📌 이 툴이 만들어진 이유

기존 공정 로그 분석은 숙련된 엔지니어가 수백 줄의 로그를 직접 눈으로 읽고 판단해야 했습니다.  
이 툴은 그 과정을 자동화하여 **주니어 엔지니어도 베테랑 수준의 초동 대응**이 가능하도록 지원합니다.

- ✅ 로그 파일만 있으면 즉시 분석 가능
- ✅ 이상 센서 자동 감지 및 심각도 분류
- ✅ Inspection Checklist로 조치 순서 안내
- ✅ 엔지니어 도메인 지식이 툴에 축적되는 구조
- ✅ 향후 SOP/Manual 연동, 알람 자동화, 인터락 연계 확장 예정

---

## 🚀 빠른 시작 (Quick Start)

### 1단계 · 툴 열기
브라우저에서 HTML 파일을 열거나, GitHub Pages 링크에 접속합니다.

> 페이지가 열리면 **사이드바에 샘플 로그(REF/CMP)가 자동으로 로드**됩니다.  
> 별도 파일 업로드 없이 바로 버튼을 클릭할 수 있습니다.

### 2단계 · 분석 실행
좌측 사이드바 하단의 버튼을 클릭합니다.

```
▶  Run Comparison Analysis
```

### 3단계 · 결과 확인
상단 탭을 클릭해 각 분석 화면으로 이동합니다.

| 탭 | 내용 |
|---|---|
| 📊 Summary | 전체 Pass/Fail 비교, Step Timeline, 주요 차이 요약 |
| ⚠️ Anomaly Detection | 이상 센서 목록, 심각도(CRIT/WARN), Inspection Checklist |
| 📈 Waveform Compare | 센서별 파형 비교 그래프 (REF vs CMP) |
| 📋 Value Compare | Step별 평균값 수치 비교 테이블 |

---

## 🖥️ 화면 구성 설명

```
┌─────────────────────────────────────────────────────┐
│  ◈ PROCESS LOG ANALYZER        [Grid] [Font] [Re-Analyze] │  ← 상단 Top Bar
├───────────┬─────────────────────────────────────────┤
│           │  📊 Summary │ ⚠️ Anomaly │ 📈 Waveform │ 📋 Table │  ← 탭
│  사이드바  ├─────────────────────────────────────────┤
│           │                                         │
│ REF 파일  │              분석 결과 화면               │
│ CMP 파일  │                                         │
│           │                                         │
│ [▶ 분석]  │                                         │
│           │                                         │
│ STEP 범례 │                                         │
└───────────┴─────────────────────────────────────────┘
```

### 사이드바 (좌측)

| 영역 | 설명 |
|---|---|
| **REF · Reference Wafer** | 정상(PASS) 기준 로그. 비교의 기준선 역할 |
| **CMP · Compare Wafer** | 분석 대상(주로 FAIL) 로그 |
| **Run Comparison Analysis** | 두 로그가 로드되면 활성화. 클릭 시 전체 분석 실행 |
| **STEP Legend** | 분석 후 나타나는 공정 Step별 색상 범례 |

> 💡 **파일 교체 방법**: 드롭존에 새 로그 파일을 드래그 앤 드롭하거나 클릭해서 업로드하면 됩니다.

### 상단 Top Bar

| 컨트롤 | 설명 |
|---|---|
| **Lot / 결과** | 분석 중인 Lot ID와 REF/CMP 결과 표시 |
| **Grid** | 차트 열 수 조절 (2 / 3 / 4열) |
| **Font** | 글자 크기 조절 (S / M / L / XL) |
| **▶ Re-Analyze** | 분석 완료 후 재실행 버튼 |

---

## 📊 탭별 상세 사용법

### 탭 1 · Summary (요약)

분석 결과의 전체 개요를 한눈에 확인합니다.

**Process Result Comparison 카드**
- REF / CMP 결과 (PASS / FAIL)
- CMP 발생 알람 수
- 각 로그의 데이터 행 수, Step 수

**Step Timeline**
- REF와 CMP의 공정 진행 타임라인을 색상 바로 시각화
- 각 Step이 어느 시점에서 얼마나 진행됐는지 한눈에 파악
- Step 색상 의미:

| 색상 | Step |
|---|---|
| 🔵 파란색 | Stability |
| 🟡 노란색 | Transition |
| 🟢 초록색 | ME2 (Main Etch) |
| 🩷 분홍색 | Flash |
| 🟣 보라색 | Vent |
| 🔴 빨간색 | Abort_Vent (비정상 종료) |

**Key Differences**
- 센서별 최댓값 차이를 자동 계산해 심각도 순으로 나열
- CRITICAL / WARN / DIFF 배지로 우선순위 구분
- 클릭 시 Waveform Compare 탭으로 바로 이동

---

### 탭 2 · Anomaly Detection (이상 감지) ⚠️

**이 탭이 핵심입니다.**  
CMP 로그에서 임계값을 초과한 센서를 자동으로 감지하고, 조치 방법까지 안내합니다.

**심각도 분류**

| 배지 | 의미 | 기준 |
|---|---|---|
| 🔴 **CRIT** | 즉시 조치 필요 | 임계값 40% 이상 초과 |
| 🟠 **WARN** | 모니터링 필요 | 임계값 15~40% 초과 |

**알람 카드 구성**

```
┌──────────────────────────────────────────────┐
│ [CRIT]  RF 27 Reflected        Flash Step    │
│         07:48:00                             │
│                                              │
│  REF MAX      CMP MAX      DIFF              │
│   92.1W        258.4W      +180.5%           │
│                                              │
│ 💡 Inspection Checklist                     │
│  • 27MHz reflected power >200W → 임피던스    │
│    미스매치. Match Network 동작 로그 확인    │
│  • Flash Step 플라즈마 모드 전환 실패 의심   │
│  • Throttle Valve 및 챔버 월 클리닝 상태 확인│
└──────────────────────────────────────────────┘
```

> 💡 **View Waveform →** 버튼 클릭 시 해당 센서의 파형 상세 화면으로 이동

---

### 탭 3 · Waveform Compare (파형 비교) 📈

REF(파란선)와 CMP(빨간선)의 센서 파형을 나란히 비교합니다.

**미니 차트 그리드**
- 8개 센서 파형을 한 화면에 표시
- 이상 감지된 센서는 테두리 강조:
  - 🔴 빨간 테두리: CRITICAL
  - 🟠 주황 테두리: WARNING

**상세 차트 (클릭 시 열림)**
- 미니 차트를 클릭하면 하단에 확대 차트 표시
- REF MAX / CMP MAX / REF AVG / CMP AVG / DIFF% 수치 표시
- 해당 센서 Inspection Checklist 자동 표시

**모니터링 센서 목록**

| 센서 | 단위 | WARN 기준 | CRIT 기준 |
|---|---|---|---|
| BICEP Voltage | V | < -600 | < -1000 |
| RF 13MHz Fwd Power | W | > 520 | > 600 |
| RF 27MHz Fwd Power | W | > 220 | > 260 |
| RF 27MHz Reflected | W | > 100 | > 200 |
| Chamber Pressure | mTorr | > 34 | > 36 |
| Chamber Temp | °C | > 40 | > 43 |
| ESC Voltage | V | — | — |
| He Backside Pressure | Torr | > 10 | > 11 |

---

### 탭 4 · Value Compare (수치 비교) 📋

Step별 · 센서별 평균값을 테이블로 상세 비교합니다.

| 컬럼 | 설명 |
|---|---|
| STEP | 공정 Step 이름 |
| SENSOR | 센서 이름 |
| REF AVG | 기준 웨이퍼 해당 Step 평균값 |
| CMP AVG | 비교 웨이퍼 해당 Step 평균값 |
| DIFF | 절대 차이값 |
| DIFF% | 퍼센트 차이 |
| STATUS | OK / WARN / CRIT 자동 판정 |

---

## 📁 로그 파일 형식

이 툴은 **LAM Research ETCH001** 장비 로그 형식을 지원합니다.

**파일명 규칙 (권장)**
```
log_PASS_LOTID_SLOT.txt   ← REF (정상 기준)
log_FAIL_LOTID_SLOT.txt   ← CMP (분석 대상)
```

**로그 헤더 예시**
```
=== LAM RESEARCH ETCH001 PROCESS LOG ===
Lot: ABCD011.1  Wafer: S22  Recipe: OXIDE_ETCH_V7  Result: PASS
Date: 2026-04-20  Start: 08:00:02  End: 08:12:15
Tool: ETCH001  Chamber: PM2  Operator: AUTO
```

**데이터 행 형식**
```
[HH:MM:SS] STEP=숫자  NAME=스텝명
  센서명=값  센서명=값  ...
  ALARM=알람코드   (이상 발생 시)
```

---

## 🔮 향후 확장 로드맵

이 툴은 **엔지니어 도메인 지식이 축적되는 sLLM 구조**를 지향합니다.

### 현재 버전 (v1)
- [x] PASS/FAIL 로그 자동 비교 분석
- [x] 이상 센서 자동 감지 및 심각도 분류
- [x] Inspection Checklist 내장
- [x] 센서별 파형 시각화

### 다음 버전 예정
- [ ] **SOP / Manual 첨부 및 바로가기** — 알람 카드에서 관련 표준 절차서 직접 열기
- [ ] **COMMENT 기능** — 엔지니어가 분석 결과에 코멘트 추가 및 누적 저장
- [ ] **도메인 지식 축적** — 조치 이력 기반 자동 추천 고도화
- [ ] **실시간 이상 감지** — 장비 연동을 통한 라이브 모니터링
- [ ] **인터락 연계** — 임계값 초과 시 자동 알람/인터락 트리거

---

## ❓ 자주 묻는 질문

**Q. 분석 버튼이 비활성화되어 있어요.**  
A. REF와 CMP 두 파일이 모두 로드되어야 버튼이 활성화됩니다. 사이드바에서 파일 카드가 두 개 표시되는지 확인하세요.

**Q. 내 로그 파일을 업로드하려면?**  
A. 사이드바의 REF 또는 CMP 드롭존에 `.txt` 또는 `.log` 파일을 드래그 앤 드롭하거나, 드롭존을 클릭해서 파일을 선택하세요. 기존 데이터가 교체됩니다.

**Q. 차트가 잘려 보여요.**  
A. 상단 Grid 셀렉터에서 열 수를 줄여보세요 (4열 → 2열).

**Q. 글자가 너무 작아요.**  
A. 상단 Font 셀렉터에서 크기를 키워보세요 (M → L → XL).

---

## 📞 문의 및 기여

- **개발**: JANG SEMIN
- **문의**: roasterhealing@gmail.com  
- **GitHub**: https://jjangse1.github.io/skhynix

> 이 툴에 추가하고 싶은 Checklist 항목, 센서 임계값, SOP 링크가 있다면 언제든지 제안해주세요.  
> 엔지니어의 경험이 쌓일수록 툴도 함께 성장합니다. 🔧
