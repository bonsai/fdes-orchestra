# Scene

SceneはAgentが現在どの「場」にいるかを宣言する。

```text
Scene = Where
```

例:

```text
customer_site
construction_site
factory
office
production
remote
unknown
```

Sceneが不明なら、最初のActionはScene Discoveryとする。

```text
UNKNOWN → OBSERVE → IDENTIFY SCENE
```
