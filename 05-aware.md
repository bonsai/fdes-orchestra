# Aware

Awareは、Agentが**何にAttentionを向けるか**の宣言である。

```text
Aware = Attention Target
```

## 基本分類

```text
Human
Process
Information
System
Risk
Deviation
```

## 宣言例

```yaml
aware:
  - target: waiting
    priority: high
  - target: workaround
    priority: high
  - target: information_gap
    priority: medium
  - target: safety
    priority: critical
```

AwareはScene × Phase × Roleによって決まる。

```text
Scene × Phase × Role
          ↓
        Aware
          ↓
        Skill
```

すべてを見るのではなく、Missionに必要なAttentionを選択する。
