# 初期化

## 初期値

### DB の状態

```typescript
[]
```

--------------------------------------------------------------------------------

## 1. DB にデータ追加

### パラメータ

| パラメータ | 値                                   | 変換後                 | 備考 |
| ---------- | ------------------------------------ | ---------------------- | ---- |
| ownerId    | 00000000-0000-0000-0000-000000000000 | AAAAAAAAAAAAAAAAAAAAAA |      |
| now        | 2022-01-01T00:00:00+09:00            | 1640962800000          |      |

### DB の状態

```typescript
[
    // ScoreSummary
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        c: 0,
        ca: 1640962800000,
        ua: 1640962800000
    },
    // ItemSummary
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        t: 0,
        c: 0,
        ca: 1640962800000,
        ua: 1640962800000
    }
]
```