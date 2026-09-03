# FDE Ontology

このディレクトリは、FDEエージェントが「何を認識し、何に注意を向け、どの役目で、どのSkillを発火するか」を機械可読にするための定義層である。

Palantirの実装を移植する場所ではない。ここで扱うのは **Intent / Attention / Role / Scene / Phase / Skill** という抽象概念である。

## 1. Ontologyの目的

FDEでは、Agentの能力をモデルやプログラミング言語だけで定義しない。

同じモデルでも、置かれた「場面」と「フェーズ」が変われば、見るべきもの、判断すべきこと、行動すべきことが変わる。

したがって、Agentの振る舞いを次の関係で記述する。

```text
Scene
  ×
Phase
  ×
Role
  ×
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
Attention update
```

## 2. Core Concepts

### Scene — 場

Agentが現在活動している世界・状況。

例:

- factory
- construction_site
- office
- customer_meeting
- production_system
- incident_site
- development_project

Sceneは単なる場所ではない。**観測可能な世界の境界**である。

### Phase — フェーズ

そのSceneで現在何をしているか。

標準FDEフェーズ:

```text
observe
sense
symbolize
structure
model
compute
operate
improve
```

Phaseは時間順序と行動順序を与える。

### Role — 役目

その場面・フェーズで何に責任を持つか。

Roleは人格ではない。

```text
Role = Responsibility
```

同じLLMが、ある場面ではField Engineer、別の場面ではReviewer、さらに別の場面ではConductorになれる。

### Aware — 注意対象

Agentが認識・観測・追跡すべき対象。

```text
Aware = Attention Target
```

Awareには、直接データ化されていないものも含める。

- friction
- hesitation
- exception
- workaround
- waiting
- human judgement
- deviation
- recurrence

### Skill — 能力

Awareした対象に対して実行できる行動。

```text
Skill = Action Capability over Attention
```

例えば、`waiting` にAwareなら `detect_bottleneck` がSkillになる。

### Intent — 意図

何を実現しようとしているのか。

IntentはAttentionの上位概念であり、「何のために見るのか」を定義する。

### Deviation — 差分

期待したモデル・計算結果と、現場で実際に起きたこととの差。

FDEではDeviationをエラーとして消去しない。

```text
Deviation → New Observation → New Symbol → New Structure → Model Update
```

## 3. FDE Core Ontology

```text
Scene
 └─ contains → Observation

Observation
 └─ becomes → Symbol

Symbol
 └─ participates_in → Structure

Structure
 └─ represented_by → Model

Model
 └─ evaluated_by → Computation

Computation
 └─ informs → Operation

Operation
 └─ produces → Result

Result
 └─ compared_with → Expectation

Expectation × Result
 └─ produces → Deviation

Deviation
 └─ updates → Attention
```

## 4. Attention is the control surface

このOntologyの中心はAgentではなく **Attention** である。

```text
Model
  ↓
Context
  ↓
Role
  ↓
Attention
  ↓
Skill selection
  ↓
Action
```

Conductorは全てを自分で実行しない。

現在のScene / Phaseを認識し、必要なAttentionを配分し、そのAttentionを持つRoleへSkillを渡す。

## 5. Skill Declaration

Skillは「どう実装するか」ではなく、「何をAwareして、何を可能にするか」を宣言する。

```yaml
skill: detect_friction
aware:
  - waiting
  - repetition
  - workaround
  - hesitation
action:
  - observe
  - record
  - classify
output:
  - friction_signal
```

この宣言により、実装言語やLLMの種類からSkillを切り離す。

## 6. Scene / Phase matrix

```text
                 OBSERVE   MODEL   OPERATE   IMPROVE
factory             ●        ○        ●         ●
construction        ●        ○        ●         ●
customer_meeting    ●        ○        ○         ●
incident_site       ●        ○        ●         ●
```

`●` は主要Attention、`○` は補助Attentionを表す。

同じ `exception` でも、Phaseによって意味が違う。

```text
OBSERVE  → exceptionを発見する
MODEL    → exceptionをモデル化する
OPERATE  → exceptionに対応する
IMPROVE  → exceptionを新しい知識にする
```

## 7. Role assignment

Roleは固定Agentではなく、Scene × Phaseから導出できる。

```text
(Scene, Phase, Intent)
          ↓
        Role
          ↓
       Attention
          ↓
         Skill
```

したがって、不要な専門Agentを大量に生成しない。

**役目を宣言し、必要なときだけ演奏させる。**

## 8. Orchestraとの接続

```text
                         INTENT
                           │
                           ▼
                     ┌───────────┐
                     │ CONDUCTOR │
                     └─────┬─────┘
                           │
                    Scene / Phase
                           │
                      Attention
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
            Role         Role         Role
              │            │            │
            Skill        Skill        Skill
              │            │            │
              └────────────┼────────────┘
                           ▼
                         Action
                           │
                         Result
                           │
                       Deviation
                           │
                           └──────→ Attention
```

ここでGoなどの言語はConductorを実装するための**手段**にすぎない。

## 9. Agent教育への利用

教育では「正解」を覚えさせるより、Attentionの向け方を訓練する。

悪い訓練:

```text
問題 → 答え → 実装
```

FDE訓練:

```text
場面 → 観察 → Aware → 記号化 → 構造化 → モデル化 → 計算 → 運用 → 差分
```

評価するのは最終回答だけではない。

- 何を見たか
- 何を見落としたか
- なぜそれにAttentionを向けたか
- どのRoleを選んだか
- どのSkillを発火したか
- 現場との差分をどう扱ったか

を評価する。

## 10. 最重要原則

> **Agentを作るのではなく、役目を宣言する。**

> **Skillを増やすのではなく、Awareを明示する。**

> **モデルを現実だと思わず、Deviationを次のAttentionへ戻す。**

> **言語は手段。組織化が目的。**

> **FDEとは、現場とモデルの間を往復することである。**
