# RideVaultData

자전거 컴포넌트 토크 데이터베이스

## 프로젝트 구조

```
data/
├── components/
│   ├── index.json          # 브랜드 목록 및 카테고리 관리
│   ├── shimano.json        # Shimano 컴포넌트
│   ├── sram.json           # SRAM 컴포넌트
│   ├── magura.json         # Magura 컴포넌트
│   ├── renthal.json        # Renthal 컴포넌트
│   ├── nukeproof.json      # Nukeproof 컴포넌트
│   ├── oneup.json          # OneUp Components
│   └── wolf-tooth.json     # Wolf Tooth 컴포넌트
├── brands/                 # 완성차 브랜드 데이터
└── ...
```

---

## Claude Code 프롬프팅 가이드

신제품 추가 시 Claude Code에 아래 형식으로 요청하면 데이터가 올바르게 추가됩니다.

---

### 1. 기존 브랜드에 신제품 추가

#### 프롬프트 템플릿

```
[브랜드명]에 [제품명] [카테고리]를 추가해줘.

제품 정보:
- 제품명: [정식 제품명]
- 변형 이름: [검색 가능한 다른 이름들]
- 카테고리: [brake_lever / shifter / crankset / rear_derailleur / bottom_bracket / pedals / handlebar / stem / dropper_remote / chain_guide]

볼트 정보:
1. [볼트명]: [토크값]Nm (최대 [최대토크]Nm), [수량]개, [사이즈], [주의사항]
2. [볼트명]: ...

출처: [매뉴얼명 또는 URL]
```

#### 예시 프롬프트

```
Shimano에 Deore M5100 브레이크 레버를 추가해줘.

제품 정보:
- 제품명: Deore M5100
- 변형 이름: Deore M5100, Deore, BL-M5100
- 카테고리: brake_lever

볼트 정보:
1. Clamp Bolt: 4Nm (최대 5Nm), 1개, M4

출처: Shimano Dealers Manual
```

---

### 2. 새로운 브랜드 추가

#### 프롬프트 템플릿

```
새 브랜드 [브랜드명]을 추가하고 [제품명]을 등록해줘.

브랜드 정보:
- ID: [영문 소문자, 하이픈 사용] (예: hope, race-face)
- 이름: [브랜드 정식명]
- 카테고리: [취급 카테고리 목록]

제품 정보:
- 제품명: [정식 제품명]
- 변형 이름: [검색 가능한 다른 이름들]
- 카테고리: [카테고리]

볼트 정보:
1. [볼트명]: [토크값]Nm, [사이즈]
...

출처: [출처]
```

#### 예시 프롬프트

```
새 브랜드 Hope를 추가하고 Tech 4 V4 브레이크 레버를 등록해줘.

브랜드 정보:
- ID: hope
- 이름: Hope
- 카테고리: brake_lever

제품 정보:
- 제품명: Tech 4 V4
- 변형 이름: Tech 4 V4, Tech4 V4, Hope Tech 4
- 카테고리: brake_lever

볼트 정보:
1. Clamp Bolt: 4Nm (최대 5Nm), 2개, T25, 상단 볼트 먼저 조임
2. Reach Adjust: 0.5Nm, T10

출처: Hope Official Manual
```

---

### 3. 카테고리별 필수 정보

| 카테고리 | 일반적인 볼트 | 비고 |
|---------|-------------|------|
| `brake_lever` | Clamp Bolt, Reach Adjust, Contact Adjust | |
| `shifter` | Clamp Bolt | |
| `crankset` | Crank Arm Bolt, Chainring Bolt, Preload Cap | 체인링 볼트는 대각선 조임 |
| `rear_derailleur` | B-Bolt (Mount), Pulley Bolt | |
| `bottom_bracket` | BB Cup | 드라이브 사이드 역나사 주의 |
| `pedals` | Pedal Spindle | 좌측 페달 역나사 주의 |
| `handlebar` | Clamp Bolt | 카본은 낮은 토크 |
| `stem` | Steerer Clamp, Faceplate Bolt, Top Cap | X패턴 조임 |
| `dropper_remote` | Clamp Bolt | |
| `chain_guide` | ISCG Mount Bolt | |

---

### 4. 데이터 스키마 참조

#### 브랜드 파일 구조 (예: shimano.json)

```json
{
  "brand": "브랜드명",
  "lastUpdated": "YYYY-MM-DD",
  "components": {
    "카테고리": {
      "제품명": {
        "variants": ["검색용 이름1", "검색용 이름2"],
        "bolts": [
          {
            "name": "볼트명",
            "torqueNm": 4,
            "torqueNmMax": 5,
            "count": 2,
            "size": "T25",
            "note": "주의사항",
            "warningIcon": "warning"
          }
        ],
        "source": "출처"
      }
    }
  }
}
```

#### 인덱스 파일 구조 (index.json)

```json
{
  "version": "1.0.x",
  "lastUpdated": "YYYY-MM-DD",
  "brands": [
    {
      "id": "brand-id",
      "name": "Brand Name",
      "categories": ["brake_lever", "shifter"]
    }
  ]
}
```

---

### 5. 볼트 정보 필드 설명

| 필드 | 필수 | 타입 | 설명 |
|-----|-----|------|------|
| `name` | O | string | 볼트 이름 (예: "Clamp Bolt") |
| `torqueNm` | O | number | 권장 토크값 (Nm) |
| `torqueNmMax` | X | number | 최대 토크값 (Nm) |
| `count` | X | number | 볼트 수량 (기본값: 1) |
| `size` | O | string | 공구 사이즈 (예: "T25", "M5", "8mm Hex") |
| `note` | X | string | 추가 주의사항 |
| `warningIcon` | X | string | 아이콘 타입: "warning" (주의) / "info" (정보) |

---

### 6. 주의사항 예시 (note 필드)

일반적으로 사용되는 주의사항:

- `"대각선 순서로 조임"` - 체인링, 핸들바 클램프
- `"X패턴 조임"` - 스템 페이스플레이트
- `"별모양 패턴 조임"` - SRAM 체인링
- `"드라이브 사이드: 역나사"` - BB컵
- `"좌측 페달: 역나사"` - 페달 스핀들
- `"카본 - 낮은 토크 권장"` - 카본 핸들바
- `"상단 볼트 먼저 조임"` - 브레이크 레버 클램프
- `"매우 낮은 토크 - 과조임 주의"` - 블리드 포트 등

---

### 7. 빠른 추가 프롬프트 (간략 버전)

간단한 제품은 아래처럼 요청할 수 있습니다:

```
Shimano XTR M9100 브레이크 레버 추가해줘.
Clamp Bolt 4-5Nm, M4. 출처: Shimano Dealers Manual
```

```
SRAM에 Maven 브레이크 추가.
- Clamp Bolt: 4Nm, T25
- Reach Adjust: 0.5Nm, T10
출처: SRAM Maven Manual
```

---

### 8. 검증 요청

추가 후 검증을 요청할 수 있습니다:

```
방금 추가한 [제품명] 데이터가 올바른지 확인해줘.
JSON 형식이 맞는지, 필수 필드가 다 있는지 체크해줘.
```

---

### 9. 일괄 추가

여러 제품을 한 번에 추가할 수 있습니다:

```
Shimano에 다음 브레이크 레버들을 추가해줘:

1. XTR M9100: Clamp Bolt 4-5Nm, M4
2. SLX M7100: Clamp Bolt 4-5Nm, M4
3. Deore M6100: Clamp Bolt 4-5Nm, M4

모두 출처는 Shimano Dealers Manual
```

---

### 10. 유용한 추가 요청

```
# 특정 브랜드의 모든 제품 목록 확인
Shimano에 등록된 모든 제품 목록을 보여줘.

# 특정 카테고리의 모든 제품 확인
brake_lever 카테고리에 등록된 모든 제품을 보여줘.

# 데이터 검색
"Code RSC"의 토크 정보를 찾아줘.

# index.json 업데이트
index.json의 버전을 1.0.2로 업데이트해줘.
```
