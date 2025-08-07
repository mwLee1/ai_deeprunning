# 🎯 학습률(Learning Rate)

---

## ✅ 1. 학습률은 경사 하강법의 스텝 크기 (Step Size)

### 📌 기본 수식 (Gradient Descent):

$$
\theta_{t+1} = \theta_t - \eta \cdot \nabla_\theta J(\theta_t)
$$

* $\theta_t$: 현재 파라미터 (가중치 $w$, 편향 $b$ 등 포함)
* $\eta$: **학습률**, 스칼라 상수
* $\nabla_\theta J(\theta)$: 손실 함수의 기울기 (gradient)

### 🔍 의미:

* 기울기는 **방향(direction)** 을 알려주고,
* 학습률은 그 방향으로 **얼마나 멀리 이동할지를 조절**하는 **스케일러**입니다.

---

## ✅ 2. 학습률은 수렴성과 안정성 모두에 영향

### 🎯 학습률 $\eta$가 **너무 작으면**:

* 수렴은 보장되지만 **매우 느림**
* 학습이 비효율적 (Local Minima 근처에서 진동)
* 더 큰 에폭 수가 필요

### 🎯 학습률 $\eta$가 **너무 크면**:

* 손실 함수가 감소하지 않고 **발산(diverge)**
* 또는 진동(Oscillation), 불안정(Instability)
* 파라미터 업데이트가 Overshooting

---



## ✅ 3. Adaptive Learning Rate: 동적으로 학습률을 조절

고정된 학습률은 복잡한 손실 지형에 비효율적입니다.

그래서 다음과 같은 **적응형 옵티마이저**들이 등장했습니다:

| Optimizer   | 핵심 아이디어                           |
| ----------- | --------------------------------- |
| **AdaGrad** | 학습률을 gradient의 크기에 따라 축소          |
| **RMSProp** | 최근 제곱 gradient의 이동 평균을 반영해 학습률 조절 |
| **Adam**    | 1차 모멘텀(속도) + 2차 모멘텀(스케일) 동시 적용    |

→ 이들은 각 파라미터마다 **개별 학습률**을 적용하는 구조입니다:

$$
\theta_{t+1} = \theta_t - \eta \cdot \frac{m_t}{\sqrt{v_t} + \epsilon}
$$

---

## ✅ 4. Learning Rate Scheduling: 학습률을 시간에 따라 변화

고정 학습률은 대부분 suboptimal.
다음과 같은 **스케줄링 전략**이 널리 사용됩니다:

| 스케줄                   | 설명                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------ |
| **Step Decay**        | 특정 epoch마다 학습률을 줄임                                                                   |
| **Exponential Decay** | $\eta_t = \eta_0 \cdot \exp(-\lambda t)$                                             |
| **Cosine Annealing**  | $\eta_t = \eta_{\min} + \frac{1}{2}(\eta_{\max} - \eta_{\min})(1 + \cos(\pi t / T))$ |
| **Warmup**            | 초반엔 천천히 증가시키고, 이후 decay                                                              |
| **ReduceLROnPlateau** | validation loss가 개선되지 않으면 학습률 줄임                                                     |

---

## ✅ 5. Learning Rate는 Optimization Landscape에 따라 최적값이 다름

* 평탄한 지형 (flat loss surface) → 큰 학습률도 괜찮음
* 급경사 또는 saddle point → 작은 학습률 필요
* **Ill-conditioned problem**일수록 학습률의 민감도가 커짐

> ✔️ 따라서 학습률은 **최적화 지형의 local geometry**와 관련 깊음

---

## ✅ 6. 실전 전략 (Best Practices)

| 상황                | 권장 학습률                        |
| ----------------- | ----------------------------- |
| Adam 사용 시         | $\eta \in [10^{-4}, 10^{-3}]$ |
| SGD + momentum    | $\eta \in [10^{-2}, 10^{-1}]$ |
| Transformer 기반 모델 | warmup + cosine decay 필수      |
| 학습이 발산함           | 학습률 ↓ 줄이기                     |
| 학습이 너무 느림         | 학습률 ↑ 키우기 or scheduler 도입     |

---

## ✅ 7. Advanced: 학습률 튜닝을 위한 시각화 기법

### 🔍 Learning Rate Finder (LR Range Test)

* 초기 학습률을 아주 작게 시작해서 점차 증가시키며 loss 관찰
* loss가 급격히 줄어들기 시작하는 구간 → 좋은 초기 학습률 후보

> 📘 이 기법은 **fast.ai 라이브러리**와 **PyTorch Lightning**에서 널리 사용

---

## ✅ 요약: Learning Rate는 단순한 하이퍼파라미터가 아님

| 역할              | 설명                                    |
| --------------- | ------------------------------------- |
| 최적화 속도 조절       | 경사를 따라 얼마나 크게 이동할지 결정                 |
| 수렴 안정성 조절       | 수렴 실패 or 발산을 막는 핵심 요소                 |
| 손실 곡면에 민감       | Hessian의 고유값, local curvature에 따라 달라짐 |
| Adaptive 전략과 결합 | Adam, RMSProp 등과 결합하면 동적 제어 가능        |
| 학습률 스케줄링        | 성능 향상을 위한 중요한 실전 기법                   |

---

## 🔚 요약

> 학습률은 단순한 “값”이 아니라,
> **모델이 손실 공간에서 “어떻게 움직일 것인가”를 결정짓는 다이내믹한 제어 인자**입니다.
> 특히, 딥러닝에서는 고차원, 비선형, saddle point가 많은 환경에서
> **학습률 튜닝이 최적화 성패를 좌우하는 가장 중요한 요소 중 하나**입니다.


