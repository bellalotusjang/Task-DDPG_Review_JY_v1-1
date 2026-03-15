# DDPG로 Pendulum-v1 풀기

> Deep Deterministic Policy Gradient(DDPG) 알고리즘을 직접 구현하고  
> Pendulum-v1 환경에서 진자 제어 문제를 학습시키는 실습 노트북.

논문 **Lillicrap et al. (2015) — *Continuous control with deep reinforcement learning*** 을 기반으로 작성했다.

---

## 프로젝트 소개

기존 강화학습 알고리즘(DQN)은 행동이 이산적인 문제만 풀 수 있다.  
DDPG는 **연속적인 행동 공간**을 다룰 수 있도록 Actor-Critic 구조에 딥러닝을 결합한 알고리즘이다.

이 프로젝트에서는 DDPG의 핵심 구성 요소를 처음부터 직접 구현하고,  
각 코드가 논문의 어떤 개념에 해당하는지 설명과 함께 작성했다.

| 논문 개념 | 구현 위치 |
|----------|----------|
| Actor Network | `Actor` 클래스 |
| Critic Network | `Critic` 클래스 |
| Replay Buffer | `ReplayBuffer` 클래스 |
| Target Network | `actor_target`, `critic_target` |
| Soft Update (τ) | `soft_update()` 함수 |
| Exploration Noise | `OUNoise` 클래스 |
| Bellman Equation | `train()` 내 `target_q` 계산 |

---

## 실습 환경 — Pendulum-v1

| 항목 | 내용 |
|------|------|
| 상태(State) | 진자의 cos, sin, 각속도 (3차원) |
| 행동(Action) | 토크 — **-2.0 ~ +2.0 연속값** |
| 보상(Reward) | 진자가 똑바로 서 있을수록 높음 |
| 목표 | 에피소드 보상 -200 이상 달성 |

---

## 설치 방법

### 요구 사항

```
Python >= 3.8
torch >= 2.0
gymnasium >= 0.26
numpy
matplotlib
```

### 설치

```bash
pip install gymnasium[classic-control] torch numpy matplotlib
```

---

## 사용법 / 실행 방법

### Google Colab에서 실행 (추천)

1. `DDPG_Pendulum_실습.ipynb` 파일을 Google Drive에 업로드한다
2. 파일을 우클릭 → **Colab으로 열기** 선택
3. 상단 메뉴 **런타임 → 모두 실행** 클릭
4. 약 10~20분 후 학습 곡선 확인

### 로컬에서 실행

```bash
git clone https://github.com/your-username/ddpg-pendulum.git
cd ddpg-pendulum
jupyter notebook DDPG_Pendulum_실습.ipynb
```

---

## 파일 구조

```
ddpg-pendulum/
│
├── DDPG_Pendulum_실습.ipynb   # 메인 실습 노트북
└── README.md                  # 프로젝트 설명
```

---

## 학습 결과

| 지표 | 값 |
|------|-----|
| 학습 에피소드 | 200 |
| 목표 보상 기준 | -200 이상 |
| 예상 소요 시간 | 약 10~20분 (CPU 기준) |

학습이 잘 진행되면 초반 -1500 수준에서 시작해 후반부에 -200 ~ -100 근처로 수렴한다.

---

## 핵심 하이퍼파라미터

| 파라미터 | 값 | 설명 |
|---------|-----|------|
| gamma (γ) | 0.99 | 미래 보상 할인율 |
| tau (τ) | 0.005 | Soft Update 비율 |
| Actor lr | 1e-4 | Actor 학습률 |
| Critic lr | 1e-3 | Critic 학습률 |
| Batch size | 64 | 샘플링 배치 크기 |
| Buffer size | 100,000 | Replay Buffer 최대 크기 |
| OU σ | 0.2 | 탐험 노이즈 크기 |

---

## 논문 참고 출처

- Lillicrap, T. P., et al. (2015). *Continuous control with deep reinforcement learning.* arXiv:1509.02971
- Tavakkoli, F., et al. *Model Free Deep Deterministic Policy Gradient Controller for Setpoint Tracking of Non-minimum Phase Systems.*
