# Mission — Orchestra Ontology

## 目的

`Mission` は、Agent / Orchestra が「何のために存在し、何を実現するのか」を宣言する。

MissionはTaskやActionではない。

```text
Mission = Why
Intent  = What we want to achieve
Role    = Who is responsible
Aware   = What to pay attention to
Skill   = What can be done
Action  = What is actually done
```

したがって、MissionはOrchestra全体の最上位に置く。

---

## 1. Mission Hierarchy

```text
MISSION
   ↓
 INTENT
   ↓
 SCENE
   ↓
 PHASE
   ↓
 ROLE
   ↓
 AWARE
   ↓
 SKILL
   ↓
 ACTION
   ↓
 RESULT
   ↓
 DEVIATION
   ↓
 LEARNING
   ↺
```

Missionは「行動を細かく指定する」のではなく、下位の判断に一貫した方向を与える。

---

## 2. Orchestra Mission

`palantir-orchestra` のMissionは、特定の企業・製品・言語を再現することではない。

> **現場とモデルの間を往復し、必要なAttentionを適切な役目へ配分することで、現実の問題を発見・構造化・改善できるAgent組織をつくる。**

短く表現すると、

```text
MISSION
= Organize Attention for Reality.
```

日本語では、

```text
現実に対するAttentionを組織化する。
```

---

## 3. Mission と Language

Missionは特定のプログラミング言語に依存しない。

```text
Go
Python
TypeScript
Java
LLM
SQL
Shell
```

これらはすべて手段である。

```text
Mission
   ↓
Organization
   ↓
Role / Aware / Skill
   ↓
Technology Selection
```

> **言語は手段。組織化が目的。**

したがって、技術選定がMissionを決めてはいけない。
Missionに必要な能力を見て、後から技術を選ぶ。

---

## 4. Mission と FDE

FDE（Field × Engineering × AI × Business）のMissionは、技術を導入することではない。

```text
Field
 ↓
Reality
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
Business Value
```

重要なのは、ModelをRealityそのものとみなさないことである。

```text
Reality ≠ Model

Reality
  ↓
 Model
  ↓
 Operation
  ↓
 Deviation
  ↓
 Reality
```

Deviationが発生したらMissionを見失わず、Attentionを更新する。

---

## 5. Mission / Intent / Goal の違い

| 概念 | 問い | 例 |
|---|---|---|
| Mission | なぜ存在する？ | 現実に対するAttentionを組織化する |
| Intent | 何を実現したい？ | 現場のボトルネックを発見する |
| Goal | どこまで達成する？ | 待ち時間を20%削減する |
| Task | 何をする？ | 作業時間を計測する |
| Action | 今何をする？ | 作業者に質問する |

MissionはGoalより上位にある。

Goalが達成できなくても、Missionに沿っていれば次のIntentを生成できる。

---

## 6. Mission は固定、Intent は変化する

Missionは比較的安定している。
Intentは現場の状況によって変化する。

```text
Mission
  │
  ├── Intent A
  │      └── Deviation
  │
  ├── Intent B
  │      └── Deviation
  │
  └── Intent C
```

例えば、最初は

```text
Intent = 業務を効率化する
```

だったとしても、現場観察によって

```text
Deviation = システムではなく情報伝達がボトルネック
```

と分かれば、Intentを

```text
Intent = 情報伝達の構造を明らかにする
```

へ変更する。

Missionは変えない。

---

## 7. Mission と Aware

Missionは「何を見るか」を直接指定しない。

ConductorがMissionからIntentを定め、Scene / Phase / Roleを経由してAwareを選ぶ。

```text
Mission
 ↓
Intent
 ↓
Scene
 ↓
Phase
 ↓
Role
 ↓
Aware
```

つまり、AwareはMissionの局所的な実装である。

```text
Mission
「現実に対するAttentionを組織化する」

        ↓

Aware
「この場面では何に注意すべきか」
```

---

## 8. Mission と Orchestra

Orchestraでは、全Agentが同じことをする必要はない。

むしろ、異なる役目がMissionを共有する。

```text
                    MISSION
                       │
            ┌──────────┼──────────┐
            ↓          ↓          ↓
         Field      Analyst    Engineer
            │          │          │
          Aware      Aware      Aware
            │          │          │
          Skill      Skill      Skill
            └──────────┼──────────┘
                       ↓
                    Result
                       ↓
                   Deviation
                       ↓
                  Conductor
                       ↓
                Attention更新
```

このため、Agentの数を増やすこと自体には価値がない。

価値は、**Missionに対して必要なAttentionが組織化されていること**にある。

---

## 9. Mission Contract

Agentは実行前にMissionとの整合性を確認する。

```yaml
mission:
  purpose: organize_attention_for_reality
  principles:
    - reality_first
    - attention_before_action
    - deviation_is_signal
    - language_is_a_means
```

Role側では、これを具体化する。

```yaml
role:
  name: field_engineer
  mission_alignment:
    - observe_reality
    - discover_deviation
    - symbolize_field_knowledge
```

Skill側では、さらにActionへ落とす。

```yaml
skill:
  name: detect_friction
  supports_mission:
    - organize_attention_for_reality
  aware:
    - waiting
    - repetition
    - workaround
  action:
    - observe
    - record
    - classify
```

---

## 10. Mission Guardrails

Missionに反する行動を防ぐ。

### 禁止

```text
・技術ありきで問題を定義する
・観測前に解決策を固定する
・モデルを現実そのものとみなす
・Deviationをノイズとして捨てる
・Roleを固定的な人格として扱う
・Skillを目的化する
```

### 推奨

```text
・現実から始める
・Attentionを宣言する
・役目を明示する
・必要なSkillを選ぶ
・結果を観測する
・Deviationを次のAttentionへ戻す
```

---

## 11. Mission Loop

Missionは一方向の工程ではなくLoopである。

```text
             ┌──────────────┐
             │    Mission   │
             └──────┬───────┘
                    ↓
                  Intent
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
                    │
                    └────────→ Attention更新
                                      │
                                      └──→ Intent更新
```

これを `Mission Loop` と呼ぶ。

---

## 12. Mission の成功条件

Missionの成功は、単純なTask完了数では測らない。

評価するのは、

```text
1. 現実を正しく観測できたか
2. 必要なAttentionを選べたか
3. Roleが適切だったか
4. SkillがAwareに対応していたか
5. Deviationを検出できたか
6. DeviationからAttentionを更新できたか
7. Business / Fieldの価値につながったか
```

最終的には、

```text
Attention Quality
        ×
Reality Alignment
        ×
Improvement
```

で評価する。

---

## 13. Core Statement

`palantir-orchestra` のMissionを一文にする。

> **現実を観測し、Attentionを組織化し、役目とSkillを適切に割り当て、Deviationから学習し続ける。**

そして、そのための基本原則を次のように固定する。

```text
MISSION
  ↓
現実を起点にする
  ↓
ATTENTION
  ↓
何を見るかを宣言する
  ↓
ROLE
  ↓
役目を宣言する
  ↓
SKILL
  ↓
必要な能力だけ使う
  ↓
ACTION
  ↓
現場を動かす
  ↓
DEVIATION
  ↓
差異を学習信号にする
  ↓
MISSION
```

**Agentとは、Missionを理解して答える存在ではない。Missionに沿ってAttentionを配分し、現実とのズレによって自らの次の行動を変えられる存在である。**
