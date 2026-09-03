# Scene × Phase × Role × Aware × Skill

FDE Agentの教育単位はAgentそのものではなく、**場面とフェーズに応じた役目の宣言**である。

## Core Formula

```text
Scene × Phase × Role × Aware → Skill → Action
```

- **Scene**: どこにいるか
- **Phase**: 何をしている段階か
- **Role**: 何に責任を持つか
- **Aware**: 何に注意を向けるか
- **Skill**: その注意から何ができるか

## Example: 現場・観察

```yaml
scene: factory
phase: observation
role: field_engineer
aware:
  - people
  - machine
  - information_flow
  - waiting
  - exception
  - hesitation
  - workaround
skill:
  - observe
  - listen
  - detect_friction
  - record_signal
must_not:
  - optimize_before_observation
```

## Example: 運用・改善

```yaml
scene: production
phase: improvement
role: field_engineer
aware:
  - deviation
  - recurrence
  - new_exception
  - user_behavior
  - operational_cost
skill:
  - compare_expected_actual
  - classify_deviation
  - update_model
  - propose_experiment
must_not:
  - discard_deviation
```

## Principle

同じLLMでも、Scene / Phase / Role / Awareを前提として与えるだけで、異なる役目として振る舞える。

したがって、不要にAgentを分割しない。

> **Agentを作るのではなく、役目を宣言する。**

## Orchestra

ConductorはSceneとPhaseを認識し、必要なRoleへAttentionを配分する。RoleはAware対象を持ち、Skillを発火させる。

```text
CONDUCTOR
   ↓
Scene / Phase
   ↓
Role
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
   └────→ Aware
```

これが `palantir-orchestra` におけるFDEモデルの基本単位である。
