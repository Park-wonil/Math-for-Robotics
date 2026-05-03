# Math for Robotics

> AI 로보틱스 대학원 진학을 목표로, 수학 개념을 코드와 시각화로 직접 구현하며 공부하는 포트폴리오입니다.
> 단순 공식 암기가 아닌, **"왜 이 수식이 로봇에 쓰이는가"** 를 연결하는 것을 목표로 합니다.

---

## 📌 학습 로드맵

```
Phase 1 │ 수학 기초 시각화        ← 현재 진행 중
Phase 2 │ 과학 컴퓨팅 (SciPy, 제어이론)
Phase 3 │ 로보틱스 핵심 이론 (기구학, ROS2)
Phase 4 │ AI + 로보틱스 융합 (RL, SLAM)
Phase 5 │ 연구 수준 프로젝트 (논문 재현)
```

---

## 📂 구조

```
math-for-robotics/
├── 01_linear_algebra/          # ← 현재 진행 중
│   ├── 01_vectors_and_operations.ipynb
│   ├── 02_linear_transformations.ipynb
│   ├── 03_eigenvalues_eigenvectors.ipynb
│   ├── 04_svd.ipynb            
│   ├── 05_3d_rotations.ipynb   
│   ├── 06_least_squares.ipynb  
│   └── assets/
├── 02_calculus/                
├── 03_differential_equations/  
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

**📓 노트북:** [`01_vectors_and_operations.ipynb`](01_linear_algebra/01_vectors_and_operations.ipynb)

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

**📓 노트북:** [`02_linear_transformations.ipynb`](01_linear_algebra/02_linear_transformations.ipynb)

---

### 03 — 고유값 분해 (EVD)

$$A\vec{v} = \lambda\vec{v}$$

고유벡터는 변환 후에도 방향이 바뀌지 않는다. 이를 이용해 데이터의 주축(PCA)을 찾거나, 로봇 링크의 관성 주축을 구한다.

| 개념 | 로보틱스 활용 |
|------|--------------|
| 고유벡터 | 관성 텐서 주축, 포인트 클라우드 물체 방향 |
| 고유값 | 진동 세기, 분산 크기 |
| PCA | LiDAR 물체 방향 추출 |

**📓 노트북:** [`03_eigenvalues_eigenvectors.ipynb`](01_linear_algebra/03_eigenvalues_eigenvectors.ipynb)

---

### 04 — 특이값 분해 (SVD) ⏳

$$A = U \Sigma V^T$$

이미지 압축으로 직관을 잡고, 로봇 자세 추정(ICP 알고리즘)에서의 활용까지 연결한다.

**📓 노트북:** `04_svd.ipynb` (예정)

---

### 05 — 3D 회전 표현 

$$R \in SO(3), \quad \text{오일러각}, \quad q \in \mathbb{H}$$

회전행렬·오일러각·쿼터니언을 비교하고, 짐벌락(Gimbal Lock) 문제를 시각화한다.
쿼터니언은 ROS2의 표준 회전 표현.

**📓 노트북:** `05_3d_rotations.ipynb` (예정)

---

### 06 — 최소제곱법 

$$\min_x \|Ax - b\|^2 \Rightarrow x = (A^TA)^{-1}A^Tb$$

라이브러리 없이 NumPy로 직접 구현. 로봇 센서 캘리브레이션에서의 활용까지 연결한다.

**📓 노트북:** `06_least_squares.ipynb` (예정)

---

##  실행 방법

```bash
git clone https://github.com/YOUR_USERNAME/math-for-robotics.git
cd math-for-robotics
pip install -r requirements.txt
jupyter notebook
```

---

## 🛠 사용 도구

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat&logo=scipy)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

---

## 🎯 최종 목표

AI 로보틱스 분야 대학원 진학을 위한 포트폴리오 구축.
수학 → 제어이론 → 로봇 기구학 → 강화학습 기반 로봇 제어까지 단계적으로 구현합니다.

---

*컴퓨터공학과 2학년 | 업데이트: 진행 중*
