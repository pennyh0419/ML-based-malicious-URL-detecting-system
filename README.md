# ML-based Malicious URL Detection System
머신러닝을 활용하여 URL이 정상 URL인지 악성 URL인지 분류하는 프로젝트입니다. URL 문자열에서 특징을 추출하고, Random Forest 모델을 이용해 악성 URL 탐지를 수행합니다.
## Dataset
본 프로젝트에서는 Kaggle의 공개 악성 URL 데이터셋을 사용했습니다.
- Dataset: [Malicious URLs Dataset](https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset)
- Classes:
  - benign
  - pishing
  - malware
  - defacement
 현재 모델에서는 문제를 단순화하기 위해 이진 분류 방식으로 라벨을 변환했습니다.
| Original Label | Converted Label |
| --- | --- |
| benign | 0 |
| pishing | 1 |
| malware | 1 |
| defacement | 1 |
즉, `0`은 정상 URL, `1`은 악성 URL을 의미합니다.
> 공개 데이터셋 특성상 일부 라벨 오류, 중복 URL, 오래된 URL이 포함될 가능성이 있습니다.
> 따라서 본 프로젝트에서는 해당 데이터셋을 초기 모델 개발 및 특징 추출 실험용으로 사용했습니다.
## Feature Extraction
머신러닝 모델은 URL 문자열을 직접 이해할 수 없기 때문에, URL의 특징을 숫자 형태로 변환했습니다.
현재 사용한 주요 특징은 다음과 같습니다.
- URL 길이
- 도메인 길이
- `.` 개수
- `-` 개수
- `@` 개수
- `?` 개수
- `=` 개수
- 숫자 개수
- HTTPS 사용 여부
- IP 주소 포함 여부
## Model
현재는 기본 모델로 Random Forest Classfier를 사용했습니다.
``` python
RandomForestClassifier(
  n_estimators=100,
  random_state=42,
  n_jobs=-1
)
```
## Current Progress
현재까지 구현한 내용은 다음과 같습니다.
- Kaggle 데이터셋 불러오기
- 정상/악성 이진 라벨 변환
- URL 문자열 기반 특징 추출
- Random Forest 모델 학습
- Accuracy, Confusion Matrix, Classification Report 출력
- 새로운 URL 입력 시 정상/악성 예측
- URL 파싱 방식 개선
- 비정상 URL에 대한 예외 처리 추가
## Patch Notes
