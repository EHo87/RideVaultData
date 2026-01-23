# RideVault 토크 데이터 업데이트 가이드

## 개요

이 문서는 Claude Code를 사용하여 RideVaultData GitHub 저장소의 컴포넌트 토크 데이터를 추가하거나 수정하는 방법을 설명합니다.

**GitHub 저장소**: https://github.com/EHo87/RideVaultData

---

## 1. 데이터 구조

### 1.1 폴더 구조

```
data/
├── version.json              # 버전 정보 (lastUpdated 포함)
└── components/               # 컴포넌트 토크 데이터
    ├── index.json            # 브랜드 목록 인덱스
    ├── renthal.json
    ├── nukeproof.json
    ├── sram.json
    ├── shimano.json
    └── {brand}.json
```

### 1.2 index.json 구조

```json
{
  "version": "1.0.0",
  "lastUpdated": "2026-01-23",
  "brands": [
    {
      "id": "renthal",
      "name": "Renthal",
      "categories": ["handlebar", "stem"]
    },
    {
      "id": "sram",
      "name": "SRAM",
      "categories": ["brake_lever", "shifter", "crankset", "rear_derailleur"]
    }
  ]
}
```

### 1.3 브랜드 파일 구조 ({brand}.json)

```json
{
  "brand": "Renthal",
  "lastUpdated": "2026-01-23",
  "components": {
    "handlebar": {
      "Fatbar 35": {
        "variants": ["Fatbar 35", "Fatbar35", "Fatbar 35mm"],
        "bolts": [
          {
            "name": "Clamp Bolt",
            "torqueNm": 5,
            "count": 4,
            "size": "M5",
            "note": "대각선 순서로 조임 (1-3-2-4)",
            "warningIcon": "warning"
          }
        ],
        "source": "Renthal 공식 매뉴얼 (2024)"
      }
    },
    "stem": {
      "Apex 35": {
        "variants": ["Apex 35", "Apex35"],
        "bolts": [
          {
            "name": "Steerer Clamp",
            "torqueNm": 8,
            "count": 2,
            "size": "M5"
          },
          {
            "name": "Faceplate Bolt",
            "torqueNm": 5,
            "count": 4,
            "size": "M5",
            "note": "X패턴 조임",
            "warningIcon": "warning"
          }
        ],
        "source": "Renthal 공식 매뉴얼 (2024)"
      }
    }
  }
}
```

---

## 2. 컴포넌트 카테고리 목록

| 카테고리 키 | 설명 | 예시 브랜드 |
|------------|------|------------|
| `handlebar` | 핸들바 | Renthal, Race Face, Deity |
| `stem` | 스템 | Renthal, Race Face, Deity |
| `brake_lever` | 브레이크 레버 | SRAM, Shimano, Magura |
| `shifter` | 변속 레버 | SRAM, Shimano |
| `dropper_remote` | 드롭퍼 리모트 | OneUp, Wolf Tooth, PNW |
| `emtb_remote` | E-MTB 리모트 | Shimano, Bosch |
| `crankset` | 크랭크셋 | SRAM, Shimano, Race Face |
| `rear_derailleur` | 리어 드레일러 | SRAM, Shimano |
| `chain_guide` | 체인 가이드 | MRP, OneUp, e*thirteen |
| `bottom_bracket` | 비비 | SRAM, Shimano, Race Face |
| `pedals` | 페달 | Race Face, Crankbrothers, Shimano |

---

## 3. Claude Code로 데이터 추가하기

### 3.1 새 브랜드 추가

**프롬프트 예시**:
```
RideVaultData 저장소에 Deity 브랜드의 토크 데이터를 추가해줘.

Deity Blacklabel 800 핸들바:
- Clamp Bolt: 5Nm, 4개, M5, 대각선 순서로 조임
- 출처: Deity 공식 매뉴얼

Deity Copperhead 스템:
- Steerer Clamp: 8Nm, 2개, M5
- Faceplate Bolt: 5Nm, 4개, M5, X패턴 조임
- 출처: Deity 공식 매뉴얼
```

**Claude Code 작업**:
1. `data/components/deity.json` 파일 생성
2. `data/components/index.json`에 Deity 브랜드 추가
3. `data/version.json`의 `lastUpdated` 업데이트
4. Git commit 및 push

### 3.2 기존 브랜드에 제품 추가

**프롬프트 예시**:
```
RideVaultData 저장소의 sram.json에 Code RSC 브레이크 레버 데이터를 추가해줘.

SRAM Code RSC:
- Clamp Bolt: 4-5Nm, T25
- Reach Adjust: 0.5Nm, T10
- Contact Adjust: 0.5Nm, 2.5mm Hex
- 출처: SRAM Code 매뉴얼
```

### 3.3 토크값 수정

**프롬프트 예시**:
```
RideVaultData 저장소의 renthal.json에서 Fatbar 35의 Clamp Bolt 토크값을 5Nm에서 4-5Nm 범위로 수정해줘.
출처: Renthal 2025 매뉴얼 업데이트
```

---

## 4. 볼트 스펙 필드 정의

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| `name` | String | O | 볼트 위치/명칭 |
| `torqueNm` | Float | O | 토크값 (Nm) |
| `torqueNmMax` | Float | X | 최대 토크값 (범위 표시용) |
| `count` | Int | X | 볼트 개수 (기본값: 1) |
| `size` | String | X | 볼트 사이즈 (M5, T25, 4mm Hex 등) |
| `note` | String | X | 주의사항, 조임 순서 등 |
| `warningIcon` | String | X | 아이콘 타입 ("warning", "info") |

### 4.1 warningIcon 사용 기준

- **"warning"**: 조임 순서가 중요한 경우, 과토크 위험이 있는 경우
- **"info"**: 팁, 권장사항 (예: 그리스 도포, 탄소 페이스트 사용)

---

## 5. 데이터 검증 체크리스트

새 데이터 추가 시 확인 사항:

- [ ] 공식 소스(제조사 매뉴얼)에서 토크값 확인
- [ ] 토크값이 볼트 사이즈에 맞는 합리적인 범위인지 확인
  - M3: 0.5-1.5 Nm
  - M4: 1.5-4 Nm
  - M5: 4-8 Nm
  - M6: 8-15 Nm
- [ ] JSON 문법 유효성 검증
- [ ] `variants` 배열에 일반적인 표기 변형 포함
- [ ] `source` 필드에 출처 명시

---

## 6. Git 커밋 규칙

### 6.1 커밋 메시지 형식

```
[torque] Add {브랜드} {카테고리} data

- Added {제품명} torque specs
- Source: {출처}
```

예시:
```
[torque] Add Deity handlebar/stem data

- Added Blacklabel 800 handlebar specs
- Added Copperhead stem specs
- Source: Deity official manual
```

### 6.2 업데이트 커밋

```
[torque] Update {브랜드} {제품명}

- Updated torque value: {이전} -> {이후}
- Source: {출처}
```

---

## 7. 자주 사용하는 프롬프트 템플릿

### 7.1 전체 브랜드 추가

```
RideVaultData 저장소에 {브랜드명} 토크 데이터를 추가해줘.

{제품 카테고리} - {제품명}:
- {볼트명}: {토크값}Nm, {개수}개, {사이즈}
- 출처: {출처}

작업 완료 후:
1. data/components/{brand}.json 생성
2. data/components/index.json 업데이트
3. data/version.json의 lastUpdated 업데이트
4. 변경사항 커밋
```

### 7.2 토크값 일괄 확인/수정

```
RideVaultData 저장소의 {brand}.json 파일을 읽고,
{제조사} 공식 매뉴얼 기준으로 토크값을 확인해줘.

확인할 제품:
- {제품1}
- {제품2}

변경이 필요하면 수정하고 커밋해줘.
```

### 7.3 새 카테고리 제품 추가

```
RideVaultData의 {brand}.json에 {새 카테고리} 섹션을 추가해줘.

{제품명}:
- {볼트 스펙 목록}
- 출처: {출처}

index.json의 해당 브랜드 categories 배열도 업데이트해줘.
```

---

## 8. 참고 자료

### 8.1 제조사 매뉴얼 링크

| 제조사 | 매뉴얼 URL |
|--------|-----------|
| SRAM | https://www.sram.com/en/service |
| Shimano | https://si.shimano.com |
| Renthal | https://www.renthal.com/support |
| Race Face | https://www.raceface.com/support |
| Fox | https://www.ridefox.com/content.php/Service/TechDoc |
| RockShox | https://www.sram.com/en/rockshox/support |

### 8.2 관련 문서

- [COMPONENT_DATA_SPEC.md](COMPONENT_DATA_SPEC.md) - 데이터 스키마 상세
- [TORQUE_SPEC_DATA_GUIDE.md](TORQUE_SPEC_DATA_GUIDE.md) - 데이터 수집 가이드
- [PRD.md](PRD.md) - 제품 요구사항

---

## 변경 이력

| 버전 | 날짜 | 변경 내용 |
|------|------|----------|
| 1.0 | 2026-01-23 | 최초 작성 |
