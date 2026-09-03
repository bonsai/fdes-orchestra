# palantir-orchestra

## FDE Orchestra

このリポジトリは、Palantirの実装を移植するものではない。
移植するのは **Intent（意図）** と **Attention（注意）** だけである。

FDE（Field Development Engineer）の思考を、エージェントが実行可能な教育モデルへ変換する。

```text
FIELD → SENSE → SYMBOL → STRUCTURE → MODEL → COMPUTE → OPERATE → IMPROVE
   ↑                                                              │
   └────────────────────── deviation / learning ─────────────────┘
```

### 基本思想

- フィールドに降りる
- 感覚する
- 記号化する
- 構造化する
- モデル化する
- 計算する
- 運用する
- 改善する

**言語は手段であり、現場理解と改善が目的。**

### Orchestra

Goを「実装言語の中心」ではなく、**Attentionを配分する指揮者**として扱う。
Python、TypeScript、LLM、DBなどは必要に応じて演奏者になる。

```text
                CONDUCTOR
                    │
              Attention Allocation
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
    FIELD         MODEL       OPERATION
       │            │            │
       └──────── Intent ─────────┘
                    │
                 IMPROVE
```

### Palantirから学ぶ範囲

| 対象 | 扱い |
|---|---|
| 実装 | 移植しない |
| API | 移植しない |
| コード | 移植しない |
| アーキテクチャ | 模倣しない |
| Intent | 抽出する |
| Attention | 抽出する |

詳細は `AGENTS.md` と `training/` を参照。
