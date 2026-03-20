# 스펙트럼 감산 기반 노이즈 제거와 하이브리드 특징 추출을 이용한 음성 신호 분류

[cite_start]**팀명: fak3killer** **저자: 김준수(인천대학교 컴퓨터공학부), 김예강(인천대학교 컴퓨터공학부)** [cite: 1, 2]

---

## 1. 개요 (Introduction)
[cite_start]인공지능이 생성한 결과와 실제 결과를 구분하지 못하는 문제를 해결하기 위해, 실제 무음 구간을 활용한 노이즈 제거와 하이브리드 특징 추출 기반의 음성 분류 모델을 제안합니다[cite: 2, 4]. [cite_start]테스트 데이터 확인 결과 **98.10%의 정확도**를 달성하여 노이즈 제거와 차원 축소의 성능 개선 효과를 입증했습니다[cite: 3].

## 2. 주요 방법론 (Methodology)

### 2.1 노이즈 제거 (Noise Reduction)
* [cite_start]**구간 설정**: 평균 소리 세기가 0.01 미만인 구간을 분석하여 초반 무음 비율을 확인했습니다 (Real: 0.324, Fake: 0.2303)[cite: 7].
* [cite_start]**적용**: 모든 음성 데이터의 0초부터 0.2초를 잡음 구간으로 설정하여 전체 데이터에서 노이즈를 제거했습니다[cite: 8].

### 2.2 하이브리드 특징 추출 (Feature Extraction)
* [cite_start]**CQCC**: 음성의 주파수 특성을 효과적으로 사용하기 위해 CQT 기반의 CQCC 특징을 도입했습니다[cite: 10].
* [cite_start]**MFCC Delta**: CQCC만으로는 확인하기 어려운 음성의 시간 변화를 보완하기 위해 MFCC 델타를 함께 추출했습니다[cite: 11].
* [cite_start]**벡터 구성**: 최종적으로 CQCC 19차원 + CQCC 델타 19차원 + MFCC 델타 19차원의 벡터를 구성했습니다[cite: 12].

### 2.3 전처리 및 차원 축소 (PCA)
* [cite_start]**정규화**: `StandardScaler`를 사용하여 특징 벡터의 스케일 차이를 보정하고 학습 속도를 개선했습니다[cite: 13].
* [cite_start]**차원 축소**: 계산 효율성을 위해 PCA를 수행하여 분산이 큰 **40개의 주성분**만을 선택했습니다[cite: 14, 15].

### 2.4 분류기 (SVM)
* [cite_start]**커널**: 비선형 패턴 분류에 우수한 **RBF 커널**을 사용했습니다[cite: 16].
* [cite_start]**하이퍼파라미터**: $C=1$, $gamma=scale$ 설정을 통해 일반화 성능을 높이고 적응형 커널 폭 조정을 적용했습니다[cite: 17, 18].
* [cite_start]**모델 경량화**: 전체 파라미터 수 39,279개, 약 **319.7KB**의 크기로 소형 모델에서도 높은 성능을 구현했습니다[cite: 19, 20].

---

## 3. 실험 및 결과

### 3.1 실험 환경
| 구분 | 사양 |
| :--- | :--- |
| **CPU** | [cite_start]AMD 라이젠 7 5800X [cite: 22] |
| **RAM** | [cite_start]16.0GB [cite: 22] |
| **GPU** | [cite_start]NVIDIA GeForce RTX 3080 [cite: 22] |
| **OS** | [cite_start]Windows 11 [cite: 22] |
| **Python** | [cite_start]3.12.3 [cite: 22] |

### 3.2 하이퍼파라미터 별 실험 결과 (Accuracy)
| C | gamma | Accuracy |
| :--- | :--- | :--- |
| 0.1 | scale | [cite_start]0.9630 [cite: 26] |
| **1** | **scale** | [cite_start]**0.9810** [cite: 26] |
| 2 | scale | [cite_start]0.9805 [cite: 26] |
| 10 | scale | [cite_start]0.9745 [cite: 26] |

---

## 4. 결론
[cite_start]본 연구는 실제 노이즈 구간을 이용한 제거 방법과 CQCC, MFCC 특징 결합이 음성 분류 성능 향상에 기여함을 입증했습니다[cite: 27]. [cite_start]특히 **적응형 커널 폭 조정(gamma=scale)**이 효과적임을 확인하였으며, 제안한 경량화 모델이 음성 인식 시스템 구축에 기여할 수 있음을 보여주었습니다[cite: 25, 28].

## 5. 참고 문헌
* Todisco, M., et al. (2017). [cite_start]Constant Q cepstral coefficients: A spoofing countermeasure for automatic speaker verification[cite: 29].
* Boll, S. F. (1979). [cite_start]Suppression of acoustic noise in speech using spectral subtraction[cite: 30].
