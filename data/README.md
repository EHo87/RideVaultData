# RideVault Geometry Data

이 폴더는 GitHub에 업로드될 지오메트리 데이터의 **예시 구조**입니다.

실제 데이터는 별도의 GitHub 저장소 (https://github.com/EHo87/RideVaultData)에서 관리됩니다.

## 폴더 구조

```
data/
├── index.json                    # 브랜드/모델 인덱스
├── README.md                     # 이 파일
└── brands/
    ├── santa-cruz/
    │   ├── nomad-6-2024.json
    │   └── megatower-2024.json
    ├── specialized/
    │   └── enduro-2024.json
    ├── trek/
    │   └── slash-2024.json
    ├── canyon/
    │   └── spectral-2024.json
    └── yt-industries/
        └── capra-2024.json
```

## index.json 스키마

```json
{
  "version": "1.0.0",
  "lastUpdated": "2026-01-20",
  "totalBikes": 8,
  "brands": [
    {
      "name": "Santa Cruz",        // 표시 이름
      "slug": "santa-cruz",        // URL용 슬러그 (폴더명)
      "country": "USA",            // 국가 (선택)
      "models": [
        {
          "name": "Nomad 6",       // 모델 표시 이름
          "slug": "nomad-6",       // 모델 슬러그
          "type": "enduro",        // 자전거 종류
          "years": [2024, 2023],   // 가용 연도
          "file": "nomad-6"        // 파일명 프리픽스
        }
      ]
    }
  ]
}
```

## 자전거 지오메트리 파일 스키마

**파일명 규칙**: `{file}-{year}.json`

예시: `brands/santa-cruz/nomad-6-2024.json`

```json
{
  "brand": "Santa Cruz",
  "model": "Nomad 6",
  "year": 2024,
  "type": "enduro",
  "wheelSize": "29/27.5 MX",
  "travel": {
    "front": 170,
    "rear": 170
  },
  "source": "Geometry Geeks",
  "sourceUrl": "https://geometrygeeks.bike/bike/...",
  "lastUpdated": "2026-01-20",
  "sizes": [
    {
      "size": "L",                    // 사이즈 (필수)
      "reach": 470,                   // 리치 mm (필수)
      "stack": 626,                   // 스택 mm (필수)
      "headAngle": 63.5,              // 헤드 앵글 ° (필수)
      "seatAngle": 77.1,              // 시트 앵글 ° (필수)
      "seatAngleEffective": 77.1,     // 유효 시트 앵글 ° (선택)
      "chainstay": 435,               // 체인스테이 mm (필수)
      "wheelbase": 1260,              // 휠베이스 mm (필수)
      "bbDrop": 35,                   // BB 드롭 mm (선택)
      "bbHeight": 345,                // BB 높이 mm (선택)
      "seatTube": 430,                // 시트튜브 mm (선택)
      "topTubeActual": null,          // 탑튜브 실측 mm (선택)
      "topTubeEffective": 618,        // 유효 탑튜브 mm (선택)
      "standover": 770,               // 스탠드오버 mm (선택)
      "forkOffset": 44,               // 포크 오프셋 mm (선택)
      "forkLength": 584,              // 포크 A-C mm (선택)
      "headTube": 116,                // 헤드튜브 mm (선택)
      "trail": 128                    // 트레일 mm (선택)
    }
  ]
}
```

## 필드 설명

| 필드 | 단위 | 필수 | 설명 |
|------|------|------|------|
| size | - | Y | 프레임 사이즈 (S, M, L, XL 등) |
| reach | mm | Y | 리치 |
| stack | mm | Y | 스택 |
| headAngle | ° | Y | 헤드 앵글 |
| seatAngle | ° | Y | 시트 앵글 (실측) |
| seatAngleEffective | ° | N | 유효 시트 앵글 |
| chainstay | mm | Y | 체인스테이 길이 |
| wheelbase | mm | Y | 휠베이스 |
| bbDrop | mm | N | BB 드롭 |
| bbHeight | mm | N | BB 높이 |
| seatTube | mm | N | 시트튜브 길이 (C-T) |
| topTubeActual | mm | N | 탑튜브 실측 |
| topTubeEffective | mm | N | 유효 탑튜브 |
| standover | mm | N | 스탠드오버 높이 |
| forkOffset | mm | N | 포크 오프셋 |
| forkLength | mm | N | 포크 A-C 길이 |
| headTube | mm | N | 헤드튜브 길이 |
| trail | mm | N | 트레일 |

## 데이터 출처

- [Geometry Geeks](https://geometrygeeks.bike) - 94,000개 이상의 자전거 지오메트리 데이터베이스

## 앱에서 사용

앱은 다음 URL에서 데이터를 가져옵니다:

- **Index**: `https://raw.githubusercontent.com/EHo87/RideVaultData/main/index.json`
- **Bike**: `https://raw.githubusercontent.com/EHo87/RideVaultData/main/brands/{brand-slug}/{model-slug}-{year}.json`
