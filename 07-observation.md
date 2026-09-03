# Observation

観察はAgentの最初の実行単位である。

## Observation Contract

```text
Evidence ≠ Interpretation
```

観測した事実と解釈を分離する。

```yaml
observation:
  timestamp: "..."
  scene: "..."
  phase: observation
  role: field_observer
  aware:
    - waiting
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

## First Action

デプロイ直後のAgentは、解決策を考える前に観察する。

```text
BOOT → CONTEXT → AWARE → OBSERVE → RECORD
```
