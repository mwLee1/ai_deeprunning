

# 🧠 Maximum Likelihood Estimation (MLE)

---

## 1. 🎯 목표: 파라미터를 어떻게 추정할 것인가?

우리는 어떤 데이터셋 \$\mathcal{D} = {x^{(1)}, x^{(2)}, ..., x^{(m)}}\$ 를 관찰했을 때,
이 데이터를 생성했다고 가정되는 **확률 분포의 파라미터 \$\theta\$** 를 추정하고자 합니다.

### 예시

* 동전 던지기 데이터 → \$\theta\$: 앞면이 나올 확률
* 키 측정 데이터 → \$\theta\$: 평균 \$\mu\$, 분산 \$\sigma^2\$
* 로지스틱 회귀 → \$\theta = (w, b)\$

---

## 2. 🧪 기본 아이디어: 데이터를 가장 그럴듯하게 설명하는 파라미터

### 직관

MLE는 이렇게 말합니다:

> "**관측된 데이터를 생성했을 가능성이 가장 높은 파라미터** \$\theta\$ 를 선택하자!"

이때 "가능성이 높다"는 것은 **데이터의 우도(likelihood)가 최대**가 된다는 의미입니다.

---

## 3. 🔍 수학적 정의

### 3.1 데이터가 주어졌을 때 우도(Likelihood)

관측 데이터 \$\mathcal{D} = {x^{(1)}, ..., x^{(m)}}\$ 와 확률 모델 \$P(x \mid \theta)\$ 에 대해,
**전체 데이터가 나올 확률** (즉, likelihood)은 다음과 같습니다 (독립 데이터 가정):

$$
\mathcal{L}(\theta) = P(\mathcal{D} \mid \theta) = \prod_{i=1}^m P(x^{(i)} \mid \theta)
$$

MLE는 이 likelihood를 최대화하는 \$\theta\$를 찾습니다:

$$
\theta_{\text{MLE}} = \arg\max_\theta \mathcal{L}(\theta)
$$

---

### 3.2 로그 우도(Log-Likelihood) 사용

곱 연산은 계산상 불편하고, 미분이 어렵기 때문에 **로그**를 취합니다:

$$
\log \mathcal{L}(\theta) = \sum_{i=1}^m \log P(x^{(i)} \mid \theta)
$$

**Log-likelihood**는 **likelihood를 최대화하는 것과 동일한 목적**을 가집니다 (단조 증가 함수이므로).

결국:

$$
\theta_{\text{MLE}} = \arg\max_\theta \sum_{i=1}^m \log P(x^{(i)} \mid \theta)
$$

---

## 4. 📘 예시 1: 동전 던지기 (베르누이 분포)

### 문제

* 앞면이 나올 확률 \$\theta\$인 동전을 10번 던졌더니, 앞면이 7번 나왔다.
* \$\theta\$를 추정하라 (즉, 이 동전이 공정한지 판단).

### 모델: 베르누이 분포

각 시행은 베르누이 분포를 따름:

$$
P(x \mid \theta) = \theta^x (1 - \theta)^{1 - x}, \quad x \in \{0, 1\}
$$

### 전체 우도:

$$
\mathcal{L}(\theta) = \theta^7 (1 - \theta)^3
$$

### 로그 우도:

$$
\log \mathcal{L}(\theta) = 7 \log \theta + 3 \log(1 - \theta)
$$

이걸 \$\theta\$에 대해 미분하고 0으로 두면 최적의 \$\theta\$:

$$
\theta_{\text{MLE}} = \frac{7}{10} = 0.7
$$

> ✅ 즉, **관측된 앞면 비율 자체가 MLE 해**가 됩니다.

---

## 5. 📚 예시 2: 로지스틱 회귀에서의 MLE

로지스틱 회귀는 **이진 분류기**로서, 출력이 0 또는 1입니다.

### 모델:

$$
P(y \mid x; w, b) = \hat{y}^y (1 - \hat{y})^{1 - y}, \quad \hat{y} = \sigma(w^T x + b)
$$

* 베르누이 분포를 기반으로 한 확률 모델
* 각 샘플의 확률을 **조건부 확률**로 해석

### 전체 로그 우도:

$$
\log \mathcal{L}(w, b) = \sum_{i=1}^m \left[ y^{(i)} \log \hat{y}^{(i)} + (1 - y^{(i)}) \log(1 - \hat{y}^{(i)}) \right]
$$

### 비용 함수로 전환 (음수 부호):

$$
J(w, b) = - \frac{1}{m} \sum_{i=1}^m \left[ y^{(i)} \log \hat{y}^{(i)} + (1 - y^{(i)}) \log(1 - \hat{y}^{(i)}) \right]
$$

> 따라서 **로지스틱 회귀의 비용 함수는 MLE에서 유도된 cross-entropy 손실 함수**입니다.

---

## 6. 🧠 MLE의 성질

### ✅ 장점

* 통계적 직관이 명확함 ("데이터를 가장 그럴듯하게 설명하는 파라미터")
* 많은 확률 모델에서 자연스럽게 도출됨
* 대수의 법칙에 의해 샘플 수가 커지면 **MLE는 진짜 파라미터에 수렴** (일관성)

### ⚠️ 단점

* 이상치에 민감함 (우도는 이상값에 의해 급격히 작아짐)
* 사전 정보(prior knowledge)를 반영하지 못함 → **MAP(최대 사후 확률 추정)** 과 비교됨
* 계산이 복잡한 경우 **수치 최적화(예: gradient descent)** 가 필요

---

## 7. ✅ 정리

| 항목    | 설명                                                                                |
| ----- | --------------------------------------------------------------------------------- |
| 정의    | 데이터를 가장 가능성 높게 생성했을 파라미터를 찾는 방법                                                   |
| 기본 공식 | \$\theta\_{\text{MLE}} = \arg\max\_\theta \prod\_{i=1}^m P(x^{(i)} \mid \theta)\$ |
| 계산 방식 | 로그 우도를 최대화하는 파라미터를 찾음                                                             |
| 적용 예시 | 베르누이, 가우시안, 로지스틱 회귀 등                                                             |
| 비용 함수 | 로지스틱 회귀의 Cross-Entropy Loss는 MLE의 음수 로그 우도                                        |
| 대안    | MAP (Maximum a Posteriori Estimation): 사전 확률을 반영한 방법                              |

---

## 📌 참고 자료

* Ian Goodfellow et al., **"Deep Learning"**, Chapter 5 (Probability and MLE)
* C. Bishop, **"Pattern Recognition and Machine Learning"**, Chapter 1–3
* 통계학 입문서: Casella & Berger, "Statistical Inference"

---


필요하신 방향 말씀 주세요.
