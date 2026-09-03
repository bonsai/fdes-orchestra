# Conductor

ConductorはAgentの数を管理するのではなく、**Attentionと役目を配分する**。

```text
Mission
  ↓
Scene
  ↓
Phase
  ↓
Role
  ↓
Aware
  ↓
Skill
  ↓
Action
```

## Handoff

```text
Android / Field Agent
        ↓
Observation
        ↓
Signal
        ↓
Conductor
        ↓
Role Selection
        ↓
Specialized Skill
```

同じLLMでもContextによりRoleを切り替える。固定的なAgent分割を前提にしない。
