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
### v0.1.0 - First Model
- Kaggle Malicious URLS Dataset을 불러왔습니다.
- `benign`은 0, `pishing`, `malware`, `defacement`는 1로 변환하여 이진 분류 문제로 구성했습니다.
- URL 문자열에서 기본적인 숫자 특징을 추출했습니다.
  - URL 길이
  - 도메인 길이
  - 경로 길이
  - `.` 개수
  - `-` 개수
  - 숫자 개수
  - HTTPS 사용 여부
  - IP 주소 포함 여부
- Random Forest Classifier를 이용해 첫 번째 모델을 학습했습니다.
- Accuracy, Confusion Matrix, Classification Report를 출력했습니다.
- 새로운 URl을 입력하면 정상/악성 여부를 예측하는 함수를 구현했습니다.
#### Issue
1차 모델에서는 URL을 그대로 `urlparse()`에 입력했습니다. 하지만 데이터셋에는 `http://` 또는 `https://`가 없는 URL이 많았고, 이 경우 도메인과 경로가 제대로 분리되지 않는 문제가 있었습니다.
이로 인해 모델이 악성 URL의 실제 특징이 아니라, 데이터셋 URL 저장 형식 차이를 학습할 가능성이 있었습니다.

---

### v0.2.0 - URL Parsing Fix
- URL 파싱 방식을 개선했습니다.
- `http://` 또는 `https://`가 없는 URL에는 임시로 `http://`를 추가하며 파싱하도록 수정했습니다.
- `urlparse()`가 처리하지 못하는 비정상 URL에 대해 예외 처리를 추가했습니다.
- 도메인 길이와 경로 길이가 더 일관되게 추출되도록 수정했습니다.
- 수정 후 Accuracy와 F1-score가 1차 모델보다 향상되었습니다.
#### Remaining Issue
일부 정상 URL, 예를 들어 `https://www.naver.com`, 을 여전히 악성 URL로 판단하는 문제가 남아 있습니다. 현재 모델은 URL 문자열 기반 특징만 사용하기 때문에, 도메인 평판, WHOIS 정보, 도메인 생성일, 검색 엔진 신뢰도 같은 정보를 반영하지 못합니다. 따라서 유명한 정상 도메인이라도 문자열 패턴만 보고 잘못 판단할 수 있습니다.

---

19s 동안 생각함

아래 그대로 README에 넣으면 돼.
위치는 ## Patch Notes 섹션으로 만들면 깔끔함.

## Patch Notes

### v0.1.0 - First Model

- Kaggle Malicious URLs Dataset을 불러와 악성 URL 탐지 모델을 구현했습니다.
- 기존 라벨을 이진 분류 형태로 변환했습니다.
  - `0`: benign URL
  - `1`: malicious URL
- URL 문자열에서 기본적인 숫자 특징을 추출했습니다.
  - URL length
  - domain length
  - path length
  - number of dots
  - number of hyphens
  - number of digits
  - HTTPS usage
  - IP address usage
- Random Forest Classifier를 사용해 첫 번째 모델을 학습했습니다.
- Accuracy, Confusion Matrix, Classification Report를 이용해 모델 성능을 평가했습니다.
- 새로운 URL을 입력하면 정상/악성 여부를 예측하는 함수를 구현했습니다.

#### Issue

1차 모델에서는 URL을 그대로 `urlparse()`에 입력하여 분석했습니다.  
하지만 데이터셋에는 `http://` 또는 `https://`가 없는 URL이 많았고, 이 경우 hostname과 path가 일관되게 분리되지 않는 문제가 있었습니다.

이로 인해 모델이 악성 URL의 실제 특징이 아니라, 데이터셋의 URL 저장 형식 차이를 학습할 가능성이 있었습니다.

---

### v0.2.0 - URL Parsing Fix

- URL 파싱 방식을 개선했습니다.
- `http://` 또는 `https://`가 없는 URL에는 임시로 `http://`를 추가한 뒤 파싱하도록 수정했습니다.
- `urlparse()`가 처리하지 못하는 비정상 URL에 대해 예외 처리를 추가했습니다.
- hostname length와 path length가 더 일관되게 추출되도록 수정했습니다.
- 수정 후 Accuracy와 F1-score가 1차 모델보다 향상되었습니다.

#### Remaining Issue

일부 정상 URL, 예를 들어 `https://www.naver.com`, 을 여전히 악성 URL로 판단하는 문제가 남아 있습니다.

현재 모델은 URL 문자열 기반 특징만 사용하기 때문에, 도메인 평판, WHOIS 정보, 도메인 생성일, DNS 정보, 실제 웹페이지 콘텐츠 등을 반영하지 못합니다.

---

### v0.3.0 - Lexical Feature Expansion

- 논문 [Learning to Detect Malicious URLs](https://dl.acm.org/doi/abs/10.1145/1961189.1961202)에서 설명한 lexical feature 개념을 참고하여 특징 추출 함수를 확장했습니다.
- URL을 단순 문자열 하나로 보지 않고, hostname, path, query, token 단위로 나누어 분석하도록 수정했습니다.
- 다음과 같은 특징을 추가했습니다.
  - hostname length
  - path length
  - query length
  - TLD length
  - primary domain length
  - subdomain count
  - hostname token count
  - path token count
  - query token count
  - average token length
  - maximum token length
  - path depth
  - query parameter count
  - digit ratio
  - special character ratio
  - suspicious word count
  - URL entropy
  - hostname entropy
- 데이터셋 형식 편향을 줄이기 위해 `uses_https` feature는 제거했습니다.
- path 안에 `http` 문자열이 포함되는지 확인하는 feature를 추가했습니다.
- 수정 후 Accuracy와 F1-score가 이전 모델보다 향상되었습니다.

#### Remaining Issue

`https://www.naver.com`의 악성 확률은 감소했지만, 여전히 악성 URL로 분류되는 문제가 남아 있습니다.

이는 현재 모델이 URL 문자열의 형태만 보고 판단하기 때문입니다.
즉, `naver.com`이 실제로 신뢰할 수 있는 유명 도메인이라는 외부 정보를 알지 못합니다.

향후에는 host-based feature 또는 외부 검증 데이터셋을 추가하여 일반화 성능을 개선할 계획입니다.
