# AAMAS2027 Baseline Selection Review

> 状态：⚡️进行中
> 目的：冻结 challenger onboarding baseline taxonomy。

## Baseline Taxonomy

## 1. Incumbent-only

Purpose:
- defines incremental onboarding utility;
- measures whether challenger adoption is actually useful.

Mandatory.

## 2. Classical Exploration

Methods:
- UCB1
- Thompson Sampling
- LinUCB
- LinTS

Purpose:
- test whether standard bandit exploration solves onboarding.

## 3. Relative Advantage Modeling

Method:
- Action-Centered Contextual Bandit / ACTS

Purpose:
- covers treatment-effect / residual advantage formulation.

Important because:

new challenger advantage:

Delta(x)=r_new(x)-r_inc(x)

is not novel by itself.

## 4. Nonlinear Contextual Routing

Methods:
- PAK-UCB
- RFF-UCB

Purpose:
- remove linear-model limitation objection.

## 5. Transfer Methods

Methods:
- P-LinUCB
- P-TS

Purpose:
- test whether historical tools provide enough transfer signal.

## 6. Runtime Newcomer Onboarding

Method:
- ParetoBandit-style onboarding

Purpose:
- closest existing runtime model arrival comparison.

## Recommended Main Table

| Category | Method |
|---|---|
| Current system | Incumbent-only |
| Contextual CB | LinUCB |
| Bayesian CB | LinTS |
| Residual effect | ACTS |
| Nonlinear | RFF-UCB |
| Transfer | P-LinUCB |
| Newcomer onboarding | ParetoBandit-style |

## Appendix

- UCB1
- Beta-TS
- P-TS
- conservative variants

## Remaining Requirement

Before Ours design:
- verify all implementation fidelity;
- freeze parameters;
- define which methods appear main paper vs appendix.
