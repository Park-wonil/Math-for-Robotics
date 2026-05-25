# Math for Robotics

> AI 로보틱스 대학원 진학을 목표로, 수학 개념을 코드와 시각화로 직접 구현하며 공부하는 포트폴리오입니다.
> 단순 공식 암기가 아닌, **"왜 이 수식이 로봇에 쓰이는가"** 를 연결하는 것을 목표로 합니다.

---

## 📌 학습 로드맵

```
Phase 1 │ 수학 기초 시각화        ← 현재 진행 중
Phase 2 │ 과학 컴퓨팅 (SciPy, 제어이론)
Phase 3 │ Probabilistic Robotics (Bayes Filter, Localization, SLAM)
Phase 4 │ AI + 로보틱스 융합 (RL, SLAM)
Phase 5 │ 연구 수준 프로젝트 (논문 재현)
```

---

## 📂 구조

```
math-for-robotics/
├── Linear_algebra/             # 선형대수
│   ├── 01_vectors_and_operations.ipynb
│   ├── 02_linear_transformations.ipynb
│   ├── 03_eigenvalues_eigenvectors.ipynb
│   ├── 04_svd.ipynb            
│   ├── 05_3d_rotations.ipynb   
│   ├── 06_least_squares.ipynb  
│   ├── 07_homogeneous_transforms.ipynb
│   ├── 08_jacobian.ipynb
│   ├── 09_rank_nullspace_conditioning.ipynb
│   ├── 10_covariance_gaussian_geometry.ipynb
│   └── assets/
├── calculus/                   # 미적분
├── differential_equations/     # 미분방정식·상태공간·Kalman Filter
├── 04_probabilistic_robotics/  # Bayes Filter·MCL·Mapping·SLAM·MDP
└── requirements.txt
```

---

## 01. 선형대수 (Linear Algebra)

### 01 — 벡터와 행렬 연산

벡터 덧셈·내적·외적의 기하학적 의미와 행렬 곱을 좌표 변환 합성으로 이해한다.

$$\vec{a} \cdot \vec{b} = |\vec{a}||\vec{b}|\cos\theta \qquad \vec{a} \times \vec{b} = |\vec{a}||\vec{b}|\sin\theta \cdot \hat{n}$$

| 연산 | 로보틱스 활용 |
|------|--------------|
| 내적 | 관절 정렬도, 작업공간 분석 |
| 외적 | 회전축 계산, 토크 방향 |
| 행렬 곱 | 좌표계 변환 합성 (순기구학) |

**📓 노트북:** [`01_vectors_and_operations.ipynb`](Linear_algebra/01_vectors_and_operations.ipynb)

---

### 02 — 선형변환 시각화

행렬이 격자 공간을 어떻게 변형하는지 애니메이션으로 확인한다.
행렬식(det)이 0이 되는 순간 = 로봇의 특이 자세(Singularity).

$$R(\theta) = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}, \quad \det(R) = 1$$

| 변환 | det | 로보틱스 의미 |
|------|-----|--------------|
| 회전 $R(\theta)$ | 1 | 관절 회전 (부피 보존) |
| 스케일 $S$ | $s_x s_y$ | 단위 변환 |
| 특이 행렬 | 0 | **로봇 특이 자세** ← 중요 |

**📓 노트북:** [`02_linear_transformations.ipynb`](Linear_algebra/02_linear_transformations.ipynb)

---

### 03 — 고유값 분해 (EVD)

$$A\vec{v} = \lambda\vec{v}$$

고유벡터는 변환 후에도 방향이 바뀌지 않는다. 이를 이용해 데이터의 주축(PCA)을 찾거나, 로봇 링크의 관성 주축을 구한다.

| 개념 | 로보틱스 활용 |
|------|--------------|
| 고유벡터 | 관성 텐서 주축, 포인트 클라우드 물체 방향 |
| 고유값 | 진동 세기, 분산 크기 |
| PCA | LiDAR 물체 방향 추출 |

**📓 노트북:** [`03_eigenvalues_eigenvectors.ipynb`](Linear_algebra/03_eigenvalues_eigenvectors.ipynb)

---

### 04 — 특이값 분해 (SVD)

$$A = U \Sigma V^T$$

이미지 압축으로 직관을 잡고, 로봇 자세 추정(ICP 알고리즘)에서의 활용까지 연결한다.

**📓 노트북:** [`04_svd.ipynb`](Linear_algebra/04_svd.ipynb)

---

### 05 — 3D 회전 표현 

$$R \in SO(3), \quad \text{오일러각}, \quad q \in \mathbb{H}$$

회전행렬·오일러각·쿼터니언을 비교하고, 짐벌락(Gimbal Lock) 문제를 시각화한다.
쿼터니언은 ROS2의 표준 회전 표현.

**📓노트북:** [`05_3d_rotations.ipynb`](Linear_algebra/05_3d_rotations.ipynb)

---

### 06 — 최소제곱법 

$$\min_x \|Ax - b\|^2 \Rightarrow x = (A^TA)^{-1}A^Tb$$

라이브러리 없이 NumPy로 직접 구현. 로봇 센서 캘리브레이션에서의 활용까지 연결한다.

**📓 노트북:** [`06_least_squares.ipynb`](Linear_algebra/06_least_squares.ipynb)

---

### 07 — 동차좌표계와 SE(3)

$$T = \begin{bmatrix} R & t \\ 0 & 1 \end{bmatrix} \in SE(3)$$

회전과 이동을 하나의 $4 \times 4$ 행렬로 묶어 로봇 링크 좌표계 변환을 표현한다.

**📓 노트북:** [`07_homogeneous_transforms.ipynb`](Linear_algebra/07_homogeneous_transforms.ipynb)

---

### 08 — 야코비안과 미분기구학

$$\dot{x} = J(\theta)\dot{\theta}$$

관절 속도와 엔드이펙터 속도의 관계, 특이 자세, 의사역행렬 역기구학, 조작 가능도 타원체를 다룬다.

**📓 노트북:** [`08_jacobian.ipynb`](Linear_algebra/08_jacobian.ipynb)

---

### 09 — 랭크, 영공간, 조건수

$$rank(A), \qquad Null(A)=\{x\mid Ax=0\}, \qquad \kappa(A)=\frac{\sigma_{max}}{\sigma_{min}}$$

특이 자세, redundant robot의 nullspace motion, condition number, damped least squares를 연결한다.

**📓 노트북:** [`09_rank_nullspace_conditioning.ipynb`](Linear_algebra/09_rank_nullspace_conditioning.ipynb)

---

### 10 — 공분산과 Gaussian 기하

$$\Sigma = E[(x-\mu)(x-\mu)^T], \qquad \Omega=\Sigma^{-1}$$

공분산 타원, 선형변환에 의한 공분산 전파, Mahalanobis distance, SLAM의 정보행렬을 다룬다.

**📓 노트북:** [`10_covariance_gaussian_geometry.ipynb`](Linear_algebra/10_covariance_gaussian_geometry.ipynb)

---

## 02. 미적분 (Calculus)

### 01 — 미분과 그라디언트

$$\nabla f = \begin{bmatrix}\partial f/\partial x \\ \partial f/\partial y\end{bmatrix}$$

수치 미분, gradient descent, potential field 기반 경로 생성으로 미분의 로보틱스 의미를 연결한다.

**📓 노트북:** [`01_derivatives_and_gradients.ipynb`](calculus/01_derivatives_and_gradients.ipynb)

---

### 02 — 테일러 전개와 선형화

$$f(x) \approx f(x_0) + J_f(x_0)(x-x_0)$$

비선형 differential drive 모델을 선형화하고, EKF/LQR에서 왜 Jacobian이 필요한지 확인한다.

**📓 노트북:** [`02_taylor_linearization.ipynb`](calculus/02_taylor_linearization.ipynb)

---

### 03 — 적분과 궤적 생성

$$x(t)=x(0)+\int_0^t v(\tau)d\tau$$

속도 적분, differential drive odometry, 최소 jerk 관절 궤적을 구현한다.

**📓 노트북:** [`03_integration_and_trajectory.ipynb`](calculus/03_integration_and_trajectory.ipynb)

---

## 03. 미분방정식과 상태추정

### 01 — 1차 미분방정식과 수치해

$$\dot{x}=f(x,t)$$

Euler와 RK4를 비교하고, 모터 1차 응답과 안정성 직관을 다룬다.

**📓 노트북:** [`01_first_order_ode.ipynb`](differential_equations/01_first_order_ode.ipynb)

---

### 02 — 상태공간 모델

$$\dot{x}=Ax+Bu, \qquad y=Cx+Du$$

질량-스프링-댐퍼 시스템, 위상평면, 제어가능성 행렬을 구현한다.

**📓 노트북:** [`02_state_space_systems.ipynb`](differential_equations/02_state_space_systems.ipynb)

---

### 03 — Kalman Filter 기초

$$K_k=P_{k|k-1}H^T(HP_{k|k-1}H^T+R)^{-1}$$

1D constant velocity 모델로 예측/업데이트, 공분산, Kalman Gain, $Q/R$ 튜닝 의미를 확인한다.

**📓 노트북:** [`03_kalman_filter.ipynb`](differential_equations/03_kalman_filter.ipynb)

---

## 04. Probabilistic Robotics

Thrun, Burgard, Fox의 *Probabilistic Robotics* 흐름을 축약해 반영한다.
기초 Bayes filter → Gaussian/nonparametric filters → motion/perception model → localization → mapping/SLAM → planning 순서로 진행한다.

### 01 — Bayes Filter와 Recursive State Estimation

$$bel(x_t)=\eta p(z_t\mid x_t)\int p(x_t\mid u_t,x_{t-1})bel(x_{t-1})dx_{t-1}$$

1D 복도 로봇 예제로 prediction/correction과 Markov assumption을 구현한다.

**📓 노트북:** [`01_bayes_filter.ipynb`](04_probabilistic_robotics/01_bayes_filter.ipynb)

---

### 02 — Gaussian Filters: KF와 EKF

$$bel(x)=\mathcal{N}(\mu,\Sigma)$$

Kalman Filter와 range-bearing 측정 EKF 업데이트를 구현한다.

**📓 노트북:** [`02_gaussian_filters_kf_ekf.ipynb`](04_probabilistic_robotics/02_gaussian_filters_kf_ekf.ipynb)

---

### 03 — Robot Motion Models

$$p(x_t\mid u_t,x_{t-1})$$

Velocity motion model과 odometry rot-trans-rot model을 샘플링으로 시각화한다.

**📓 노트북:** [`03_motion_models.ipynb`](04_probabilistic_robotics/03_motion_models.ipynb)

---

### 04 — Robot Perception: Sensor Models

$$p(z_t\mid x_t,m)$$

Likelihood field와 beam range finder mixture model을 구현한다.

**📓 노트북:** [`04_sensor_models.ipynb`](04_probabilistic_robotics/04_sensor_models.ipynb)

---

### 05 — Particle Filter와 Monte Carlo Localization

$$bel(x_t) \approx \{x_t^{[i]}, w_t^{[i]}\}_{i=1}^M$$

Landmark range measurement를 이용한 MCL의 sampling, weighting, resampling을 구현한다.

**📓 노트북:** [`05_particle_filter_mcl.ipynb`](04_probabilistic_robotics/05_particle_filter_mcl.ipynb)

---

### 06 — Occupancy Grid Mapping

$$l_i=\log\frac{p(m_i)}{1-p(m_i)}$$

Inverse sensor model과 log-odds 업데이트로 grid map을 만든다.

**📓 노트북:** [`06_occupancy_grid_mapping.ipynb`](04_probabilistic_robotics/06_occupancy_grid_mapping.ipynb)

---

### 07 — GraphSLAM 기초

$$x^*=\arg\min_x \sum_i e_i(x)^T\Omega_i e_i(x)$$

1D pose graph로 loop closure, information matrix, sparse normal equation 구조를 확인한다.

**📓 노트북:** [`07_graph_slam_intro.ipynb`](04_probabilistic_robotics/07_graph_slam_intro.ipynb)

---

### 08 — MDP와 불확실성 하의 의사결정

$$V(s)=\max_a \left[R(s,a)+\gamma\sum_{s'}p(s'\mid s,a)V(s')\right]$$

Stochastic grid world에서 value iteration으로 policy를 계산한다.

**📓 노트북:** [`08_mdp_planning.ipynb`](04_probabilistic_robotics/08_mdp_planning.ipynb)

---

### 09 — Markov Grid Localization

$$bel(x_t)=\eta p(z_t\mid x_t)\sum_{x_{t-1}}p(x_t\mid u_t,x_{t-1})bel(x_{t-1})$$

2D grid belief 전체를 유지하는 histogram filter 방식 localization을 구현한다.

**📓 노트북:** [`09_markov_grid_localization.ipynb`](04_probabilistic_robotics/09_markov_grid_localization.ipynb)

---

### 10 — EKF Localization

$$bel(x_t)=\mathcal{N}(\mu_t,\Sigma_t)$$

알려진 landmark map에서 motion/sensor Jacobian을 이용해 로봇 pose를 추정한다.

**📓 노트북:** [`10_ekf_localization.ipynb`](04_probabilistic_robotics/10_ekf_localization.ipynb)

---

### 11 — EKF-SLAM

$$x=[x_r,y_r,\theta_r,m_{1x},m_{1y},\dots]^T$$

로봇 pose와 landmark를 하나의 joint Gaussian state로 추정하고 cross-covariance를 확인한다.

**📓 노트북:** [`11_ekf_slam.ipynb`](04_probabilistic_robotics/11_ekf_slam.ipynb)

---

### 12 — FastSLAM

$$p(x_{1:t},m\mid z,u)=p(x_{1:t}\mid z,u)\prod_j p(m_j\mid x_{1:t},z)$$

Trajectory particle과 particle별 landmark EKF로 Rao-Blackwellized SLAM 구조를 구현한다.

**📓 노트북:** [`12_fastslam.ipynb`](04_probabilistic_robotics/12_fastslam.ipynb)

---

### 13 — POMDP와 Belief-Space Planning

$$b'(s')=\eta p(z\mid s')\sum_s p(s'\mid s,a)b(s)$$

Tiger problem으로 partially observable planning과 information gathering action을 다룬다.

**📓 노트북:** [`13_pomdp_belief_planning.ipynb`](04_probabilistic_robotics/13_pomdp_belief_planning.ipynb)

---

### 14 — Active Localization과 Exploration

$$H(b)=-\sum_x b(x)\log b(x)$$

Frontier exploration과 belief entropy로 정보획득 기반 행동 선택을 시각화한다.

**📓 노트북:** [`14_active_localization_exploration.ipynb`](04_probabilistic_robotics/14_active_localization_exploration.ipynb)

---

##  실행 방법

```bash
git clone https://github.com/YOUR_USERNAME/math-for-robotics.git
cd math-for-robotics
pip install -r requirements.txt
jupyter notebook
```

---

##  사용 도구

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat&logo=scipy)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

---

##  최종 목표

AI 로보틱스 분야 대학원 진학을 위한 포트폴리오 구축.
수학 → 제어이론 → 로봇 기구학 → 강화학습 기반 로봇 제어까지 단계적으로 구현합니다.
Kalman Filter 이해 목표

---

*컴퓨터공학과 2학년 | 업데이트: 진행 중*
