# Aware — Attention Ontology

## 目的

`Aware` は、Agent が「何に注意を向けるべきか」を宣言するための概念である。

Agent を増やすのではなく、**場面・フェーズに応じて Attention の対象を明示する**。

> Agentを作るのではなく、役目を宣言する。  
> Skillを増やすのではなく、Awareを明示する。

---

## 1. 定義

```text
Scene × Phase × Role
        ↓
      Aware
        ↓
      Skill
        ↓
      Action
        ↓
      Result
        ↓
    Deviation
        ↓
 Attention更新
```

| 概念 | 意味 |
|---|---|
| Scene | どの「場」にいるか |
| Phase | 今どの段階にいるか |
| Role | 何に責任を持つか |
| Aware | 何に注意を向けるか |
| Skill | Awareを観測・解釈・操作する能力 |
| Action | 実際に行うこと |
| Result | Actionの結果 |
| Deviation | 期待と現実の差 |

`Aware` は単なる監視対象ではない。
**意思決定に影響する可能性があるものへ Attention を配分するための宣言**である。

---

## 2. Aware の6分類

### Human

人に関するもの。

```text
worker
customer
operator
manager
user
expert
stakeholder
behavior
hesitation
intention
emotion
```

### Process

仕事・業務・流れに関するもの。

```text
workflow
waiting
handoff
repetition
bottleneck
rework
exception
workaround
sequence
lead_time
```

### Information

情報と意味に関するもの。

```text
missing_information
contradiction
ambiguity
assumption
knowledge_gap
signal
noise
context
source
```

### System

技術・設備・ソフトウェア・構造に関するもの。

```text
system
service
interface
data
latency
failure
capacity
dependency
configuration
```

### Risk

安全・品質・事業・運用上のリスク。

```text
safety
quality
security
cost
schedule
compliance
failure_risk
operational_risk
```

### Deviation

期待と現実の差。

```text
unexpected_behavior
unexpected_cost
unexpected_delay
unexpected_result
model_reality_gap
user_workaround
unexplained_variance
```

---

## 3. Aware は「対象」ではなく「注意の向き」

同じ対象でも、PhaseによってAwareは変わる。

例えば工場の「作業者」を見る場合でも、目的は異なる。

```text
Observation
  → worker_behavior

Diagnosis
  → worker_hesitation
  → workaround

Design
  → operator_need

Operation
  → safety
  → abnormal_behavior

Improvement
  → adoption
  → unintended_effect
```

したがって、

```text
Aware = Entity
```

ではなく、

```text
Aware = Attention Target in Context
```

と定義する。

---

## 4. Aware Declaration

Agentは実行前に、自分が何を見るかを宣言できる。

```yaml
scene: construction_site
phase: observation
role: field_engineer

aware:
  - target: worker_behavior
    priority: high
  - target: waiting
    priority: high
  - target: rework
    priority: medium
  - target: safety
    priority: critical
  - target: information_gap
    priority: medium
```

この宣言によって、LLMの自由な探索を**役目に沿ったAttention**へ制約する。

---

## 5. Aware → Skill

SkillはAwareから導出される。

```text
Aware: waiting
        ↓
Skill: detect_friction
        ↓
Action: observe → record → classify
        ↓
Result: friction_signal
```

別の例。

```text
Aware: information_gap
        ↓
Skill: identify_missing_information
        ↓
Action: compare → question → symbolize
        ↓
Result: knowledge_gap
```

つまりSkillは単独の能力ではなく、

> **特定のAttention対象に対して何ができるか**

として定義する。

---

## 6. Aware Priority

すべてに同じAttentionを配分しない。

```text
priority = urgency × impact × uncertainty
```

例：

```yaml
aware:
  - target: safety
    urgency: 1.0
    impact: 1.0
    uncertainty: 0.8

  - target: waiting
    urgency: 0.6
    impact: 0.8
    uncertainty: 0.5
```

Priorityは固定値ではなく、観測結果とDeviationによって更新される。

```text
Observation
    ↓
Attention Score
    ↓
Aware Priority
    ↓
Skill Selection
    ↓
Action
```

---

## 7. Aware と Deviation

FDEではDeviationを失敗として捨てない。

Deviationは、次にどこへAttentionを向けるべきかを示すSignalである。

```text
Expected
   ↓
Reality
   ↓
Deviation
   ↓
"なぜ違う？"
   ↓
New Aware
   ↓
New Observation
```

したがって、Agentの学習は

```text
正解を覚える
```

だけではなく、

```text
差異を発見する
→ Attentionを変える
→ 再観測する
```

ことになる。

---

## 8. Aware Budget

Agentは無限にすべてを見ることはできない。

そこで、SceneとPhaseからAttention Budgetを決める。

```yaml
attention_budget:
  critical: 2
  high: 3
  medium: 5
```

例：安全上の問題が検出された場合、通常の効率改善よりSafetyへAttentionを移す。

```text
normal
  ↓
Risk detected
  ↓
Attention reallocation
  ↓
Risk investigation
  ↓
Return to normal
```

これがOrchestraにおけるConductorの重要な役目になる。

---

## 9. Scene × Phase × Role × Aware

Awareは単独では意味を持たない。

```text
Scene
  ↓
Phase
  ↓
Role
  ↓
Aware
  ↓
Skill
```

同じ `waiting` でも、役目によって意味が変わる。

| Scene | Phase | Role | Aware |
|---|---|---|---|
| factory | observation | field_engineer | waiting |
| factory | diagnosis | analyst | bottleneck |
| factory | design | architect | handoff |
| factory | operation | operator | abnormality |
| factory | improvement | FDE | deviation |

これにより、同じLLMでもContextによって異なる役目を演奏できる。

---

## 10. Conductorとの関係

ConductorはAgentそのものではない。

Conductorの役目はAttentionを配分することである。

```text
Intent
  ↓
Scene
  ↓
Phase
  ↓
Role
  ↓
Aware Selection
  ↓
Skill Selection
  ↓
Action
```

つまりOrchestraとは、複数Agentを単純に並べる仕組みではなく、

> **必要なAttentionを、必要な役目へ配分する組織化機構**

である。

---

## 11. Agent Training

Agent Trainingでは、回答の正誤だけでなくAwareを評価する。

### 評価項目

```text
1. 何にAwareだったか
2. なぜそれにAttentionを向けたか
3. 重要な対象を見落とさなかったか
4. Scene / Phase / Roleと整合していたか
5. Awareから適切なSkillを選んだか
6. Deviationを次のAttentionへ反映したか
```

### Training Example

```yaml
scene: customer_site
phase: observation
role: field_engineer

aware:
  - customer_behavior
  - waiting
  - workaround
  - information_gap

observation:
  - customer manually exports data to csv
  - export takes 20 minutes
  - operator repeats the same operation daily

deviation:
  - system_usage differs from designed workflow

next_aware:
  - actual_workflow
  - hidden_cost
  - operator_intent
```

ここでは「CSVを自動化する」という解決策を最初から決めない。
まずAwareを変えながら現実を理解する。

---

## 12. 原則

### Principle 1 — Attention First

```text
解決策より先に、何を見るかを決める。
```

### Principle 2 — Contextual Awareness

```text
AwareはScene × Phase × Roleで決まる。
```

### Principle 3 — Skill Follows Aware

```text
Skillを先に増やさない。
必要なAwareからSkillを導出する。
```

### Principle 4 — Deviation is Signal

```text
Deviationを失敗として捨てない。
次のAttentionへ戻す。
```

### Principle 5 — Language is a Means

```text
言語は手段。
組織化が目的。
```

---

## 13. FDEとしてのAware

FDEの本質は、特定の技術を使うことではない。

```text
Field
  ↓
Aware
  ↓
Observation
  ↓
Symbolization
  ↓
Structure
  ↓
Model
  ↓
Compute
  ↓
Operation
  ↓
Deviation
  ↓
Aware
```

現場とモデルの間を往復しながら、
**どこへAttentionを向けるべきかを更新し続けること**。

これを `Aware Loop` と呼ぶ。

```text
        ┌──────────────┐
        │    Aware     │
        └──────┬───────┘
               ↓
          Observation
               ↓
            Action
               ↓
            Result
               ↓
          Deviation
               │
               └──────→ Aware
```

FDE Agentとは、答えを持つAgentではない。

**現場で何に気づくべきかを知り、気づいた差異によって次のAttentionを変えられるAgentである。**
