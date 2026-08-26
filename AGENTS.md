# RideVaultData Agent Guide

## 프레임 토크 데이터 작업

`data/frame/**`를 조사하거나 수정하기 전에 다음 자료를 확인한다.

- 공식 매뉴얼 보관소: `D:\Personal\MTB\App\reference-manuals\frame-torque`
- 공용 감사 가이드: `D:\Personal\MTB\App\files\docs\FRAME_TORQUE_MANUAL_LIBRARY.md`
- 앱 미러 데이터: `D:\Personal\MTB\App\files\data\frame`

매뉴얼 보관소는 Git 밖에 있으며 브랜드 → 적용 연식/세대 → 모델 → 공식 PDF와 `AUDIT.md` 순으로 구성한다. 정확한 모델·세대의 제조사 공식 PDF 또는 명시적으로 `official web-only`라고 표시한 제조사 공식 지원 페이지 외에는 프레임 토크 근거로 사용하지 않는다.

프레임 토크 JSON 변경 시 배포본과 앱 미러를 함께 갱신하고, frame index/버전/날짜, JSON 파싱, 모델 수, 양수 토크, 범위 순서, 중복을 검증한다. PDF·HTML·렌더 이미지는 이 저장소에 커밋하지 않는다.

동일 canonical 모델 별칭이 세대별 레코드에 반복되면 앱이 자전거 연식으로 정확히 한 건을 고를 수 있어야 한다. 소재별 값이 다른데 소재 정보가 없으면 복수 후보를 유지해 no-data가 되도록 하며 임의 소재값을 반환하지 않는다.
