# Role

RoleはAgentの人格ではなく、**責任の宣言**である。

```text
Role = Responsibility
```

## 基本Role

- `field_observer` — 現実を観察する
- `field_engineer` — 現場と技術を接続する
- `analyst` — 観測された情報を構造化・分析する
- `engineer` — 必要な技術的実装を行う
- `conductor` — AttentionとRoleを配分する

同じLLMがScene / Phaseに応じて異なるRoleを演奏できる。

## Role Contract

```yaml
role: field_observer
responsibility:
  - observe_reality
  - preserve_evidence
  - detect_deviation
must_not:
  - optimize_before_observation
  - invent_evidence
```
