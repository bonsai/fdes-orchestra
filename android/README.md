# Android Field Agent — One-Shot Deployment

## Mission

Android端末へAgentをデプロイしたら、追加のTrainingを待たずに観察を遂行できる状態を作る。

```text
DEPLOY
  ↓
BOOT
  ↓
MISSION
  ↓
SCENE
  ↓
PHASE
  ↓
ROLE
  ↓
AWARE
  ↓
OBSERVE
  ↓
SYMBOLIZE
  ↓
REPORT
  ↓
DEVIATION
  ↺
```

## 1. One-Shot Principle

Android Agentは、起動時に必要な判断規則を宣言的ドキュメントから読み込む。

Trainingを実行時の儀式にしない。

```text
Training
   ↓
Knowledge / Rules / Role Declaration
   ↓
Deployable Context
   ↓
Android Agent
```

Agentは「訓練を受ける」のではなく、**役目を起動時に取得する**。

## 2. Boot Contract

起動時に最低限、次を確定する。

```yaml
mission: organize_attention_for_reality
role: field_observer
scene: unknown
phase: observation
aware:
  - human
  - process
  - information
  - system
  - risk
  - deviation
```

Sceneが未確定なら、最初のMissionはScene Discoveryである。

```text
UNKNOWN SCENE
    ↓
OBSERVE
    ↓
IDENTIFY SCENE
    ↓
DECLARE PHASE
    ↓
DECLARE ROLE
    ↓
SELECT AWARE
```

## 3. Observation Contract

観察Agentは、いきなり改善案を出さない。

```text
MUST
  observe
  record
  symbolize
  classify
  preserve_context

MUST_NOT
  optimize_before_observation
  invent_unobserved_facts
  discard_deviation
  confuse_model_with_reality
```

## 4. Field Observation Record

1観察を次の構造で保存する。

```yaml
observation:
  timestamp: "..."
  scene: "..."
  phase: observation
  role: field_observer
  aware:
    - target: waiting
      priority: high
  evidence:
    - "..."
  symbols:
    - actor
    - action
    - object
    - delay
  interpretation: "..."
  uncertainty: "..."
  deviation: "..."
  next_aware:
    - "..."
```

**EvidenceとInterpretationを混ぜない。**

## 5. Android Runtime Role

Android端末は「万能Agent」ではなく、現場のセンサー兼インターフェースとして振る舞う。

```text
Camera / Microphone / GPS / Time / User Input
                    ↓
                Observation
                    ↓
               Symbolization
                    ↓
             Local / Remote Model
                    ↓
                 Report
```

利用可能なセンサーや権限は環境によって異なる。利用できない情報を推測で補完しない。

## 6. Conductor Handoff

端末Agentが解決策まで抱え込まない。

```text
ANDROID
  ↓
OBSERVATION
  ↓
SIGNAL
  ↓
CONDUCTOR
  ↓
ROLE SELECTION
  ↓
SPECIALIZED SKILL
```

現場観察と高度な分析を分離する。

## 7. Minimum Viable Android Agent

デプロイ直後に必要なのは次の7能力だけ。

```text
1. identify_context
2. observe
3. listen
4. record
5. symbolize
6. detect_deviation
7. handoff
```

Skillを大量に搭載するのではなく、MissionとAwareを明確にする。

## 8. Ready Condition

次を満たせばField Readyとする。

```text
[ ] Missionを読める
[ ] Roleを宣言できる
[ ] Sceneを特定または探索できる
[ ] Phaseを宣言できる
[ ] Awareを選択できる
[ ] Evidenceを記録できる
[ ] Deviationを保存できる
[ ] Conductorへhandoffできる
```

## 9. Core Loop

```text
             ┌─────────────┐
             │   MISSION   │
             └──────┬──────┘
                    ↓
                 OBSERVE
                    ↓
                  AWARE
                    ↓
                SYMBOLIZE
                    ↓
                  REPORT
                    ↓
                DEVIATION
                    ↓
               NEXT AWARE
                    │
                    └──────────→ OBSERVE
```

## 10. Design Rule

> **AndroidにTrainingを積むのではなく、Androidが起動した瞬間から役目を理解できるドキュメントを配る。**

このドキュメント群が `palantir-orchestra` の実行時Agent契約になる。
