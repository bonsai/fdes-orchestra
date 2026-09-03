# One-Shot Agent Specification

## 目的

`palantir-orchestra` は、AgentをTraining済みの人格として配布するのではなく、**Mission / Role / Aware / Skillを宣言した実行単位としてデプロイする**。

## Deployment Contract

```text
Document
   ↓
Agent Boot
   ↓
Mission Load
   ↓
Role Declaration
   ↓
Scene / Phase Detection
   ↓
Aware Selection
   ↓
Observation
   ↓
Handoff
```

## Required Documents

```text
ontology/mission.md
ontology/aware.md
ontology/role-aware.schema.json
ontology/scene-phase.json
ontology/aware-taxonomy.json
```

## Runtime Rule

起動時にすべてを記憶させる必要はない。Agentが参照すべき宣言を解決できればよい。

```text
MISSION = Why
ROLE    = Responsibility
AWARE   = Attention
SKILL   = Capability
```

## First Action

Agentの最初の行動は「回答」ではなく「観察」である。

```text
if scene == unknown:
    discover_scene()

if phase == unknown:
    declare_phase()

declare_role()
select_aware()
observe()
record_evidence()
```

## Training Directory Policy

旧 `training/` は実行時Agentの前提から外す。

Trainingは教材として分離して増殖させない。
必要な知識・役目・注意対象は `ontology/` と `docs/` の宣言へ統合する。

```text
OLD
training → Agent

NEW
ontology + docs → Agent Boot → Observation
```

## One-Shot Acceptance

```text
Deploy
  ↓
Boot
  ↓
Mission understood
  ↓
Role declared
  ↓
Aware selected
  ↓
First observation
```

この一連を追加Trainingなしで実行できればOne-Shot Readyとする。

## Non-Goals

- 特定LLMへの依存
- 特定プログラミング言語への依存
- Palantir実装のコピー
- Agent人格の固定
- 巨大なTraining Corpusの事前構築

## Core Principle

> **Training is not a prerequisite for observation. Declaration is.**

```text
言語は手段。
Trainingは手段。
Missionが方向を与え、AwareがAttentionを決める。
```
