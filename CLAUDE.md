# RideVaultData - Claude 작업 안내

## 프레임 토크 데이터 작업 전 필수 확인

- Git 밖 공식 매뉴얼 보관소: `D:\Personal\MTB\App\reference-manuals\frame-torque`
- 폴더·PDF 검증·감사 절차: `D:\Personal\MTB\App\files\docs\FRAME_TORQUE_MANUAL_LIBRARY.md`
- 앱 미러 데이터: `D:\Personal\MTB\App\files\data\frame`

`data/frame/**` 값은 정확한 모델·세대의 제조사 공식 문서 관련 페이지를 시각 확인한 경우에만 추가·유지한다. 제조사가 모델별 PDF 토크표를 제공하지 않고 공식 지원 페이지에만 값을 제공하는 경우 `source`에 `(official web-only; PDF torque table unavailable)`를 반드시 남긴다. 리뷰, 포럼, 판매점, 비공식 미러, 일반 볼트 토크표로 보완하지 않는다.

공식 원본과 모델별 `AUDIT.md`는 외부 매뉴얼 보관소에 저장하고 Git에는 추가하지 않는다. 데이터 변경 시 이 저장소와 앱 미러를 의미상 동일하게 반영하고 `data/frame/index.json`, `data/version.json`, 모델 수와 토크 범위를 검증한다.

세대별 레코드는 앱에서 자전거 연식으로 필터링한다. 소재별 값이 다르지만 Bike 데이터에 소재가 없는 경우 복수 후보를 유지해 no-data로 처리하고, Carbon 또는 Alloy 중 하나를 임의 기본값으로 선택하지 않는다.
